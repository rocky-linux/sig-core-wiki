---
title: Mirror Manager Maintenance
---

This SOP contains most if not all the information needed for SIG/Core to
maintain and operate Mirror Manager for Rocky Linux.

## Contact Information

| | |
| - | - |
| **Owner** | SIG/Core (Release Engineering & Infrastructure) |
| **Email Contact** | infrastructure@rockylinux.org |
| **Email Contact** | releng@rockylinux.org |
| **Mattermost Contacts** | `@label` `@neil` `@tgo` |
| **Mattermost Channels** | `~Infrastructure` |

## Introduction

So you made a bad decision and now have to do things to Mirror Manager. Good
luck.

## Pieces

| **Item**          | Runs on...       | Software                                          |
|-------------------|------------------|---------------------------------------------------|
| Mirrorlist Server | mirrormanager001 | https://github.com/adrianreber/mirrorlist-server/ |
| Mirror Manager 2  | mirrormanager001 | https://github.com/fedora-infra/mirrormanager2    |

### Mirrorlist Server

This runs two (2) instances. Apache/httpd is configured to send `/mirrorlist`
to one and `/debuglist` to the other.

* Every fifteen (15) minutes: Mirrorlist cache is regenerated

    * This queries the database for active mirrors and other information and writes a protobuf. The mirrorlist-server reads the protobuf and responds accordingly.

* Every twenty (20) minutes: Service hosting `/mirrorlist` is restarted
* Every twenty-one (21) minutes: Service hosting `/debuglist` is restarted

Note that the timing for the restart of the mirror list instances are arbitrary.

### Mirror Manager 2

This is a uwsgi service fronted by an apache/httpd instance. This is responsible
for everything else that is not `/mirrorlist` or `/debuglist`. This allows the
mirror managers to, well, manage their mirrors.

### CDN

Fastly sits in front of mirror manager. VPN is required to access the `/admin` endpoints.

If the backend of the CDN is down, it will attempt to guess what the user wanted to access and spit out a result on the dl.rockylinux.org website. For example, a request for AppStream-8 and x86_64 will result in a `AppStream/x86_64/os` directory on dl.rockylinux.org. Note that this isn't perfect, but it helps in potential down time or patching.

```
Fastly -> www firewall -> mirrormanager server
```

In reality, the flow is a lot more complex, and a diagram should be created to map it out in a more user-friendly manner (@TODO)

```
User -> Fastly -> AWS NLB over TLS, passthru -> www firewall cluster (decrypt TLS) -> mirrormanager server (Rocky CA TLS)
```

## Tasks

Below are a list of possible tasks to take with mirror manager, depending on the scenario.

### New Release

For the following steps, the following must be completed:

* Production rsync endpoints should have all brand new content
* New content root should be locked down to 750 (without this, mirror manager cannot view it)
* Disable mirrormanager user cronjobs

1. Update the database with the new content. This is run on a schedule normally (see previous section) but can be done manually.

    a. As the mirror manager user, run the following:

```
/opt/mirrormanager/scan-primary-mirror-0.4.2/target/debug/scan-primary-mirror --debug --config $HOME/scan-primary-mirror.toml --category 'Rocky Linux'
/opt/mirrormanager/scan-primary-mirror-0.4.2/target/debug/scan-primary-mirror --debug --config $HOME/scan-primary-mirror.toml --category 'Rocky Linux SIGs'
```

2. Update the redirects for `$reponame-$releasever`

    a. Use psql to mirrormanager server: `psql -U mirrormanager -W -h mirrormanager_db_host mirrormanager_db`

    b. Confirm that all three columns are filled and that the second and third columns are identical:
    ```
    select rr.from_repo AS "From Repo", rr.to_repo AS "To Repo", r.prefix AS "Target Repo" FROM repository_redirect AS rr LEFT JOIN repository AS r ON rr.to_repo = r.prefix GROUP BY r.prefix, rr.to_repo, rr.from_repo ORDER BY r.prefix ASC;`
    ```

    c. Change the `majorversion` redirects to point to the new point release, for example:
    ```
    update repository_redirect set to_repo = regexp_replace(to_repo, '9\.2', '9.3') where from_repo ~ '(\w+)-9-(debug|source)';`
    ```

    d. Insert new redirects for the major version expected by the installer

    ```
    insert into repository_redirect (from_repo,to_repo) select REGEXP_REPLACE(rr.from_repo,'9\.2','9.3'),REGEXP_REPLACE(rr.to_repo,'9\.2','9.3')FROM repository_redirect AS rr WHERE from_repo ~ '(\w+)-9.2';
    ```

3. Generate the mirrorlist cache and restart the debuglist and verify.

Once the bitflip is initiated, restart mirrorlist and reenable all cronjobs.

### Out-of-date Mirrors

1. Get current shasum of repomd.xml. For example: `shasum=$(curl https://dl.rockylinux.org/pub/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml | sha256sum)`
2. Compare against latest propagation log:

```
tail -latr /var/log/mirrormanager/propagation/rocky-9.3-BaseOS-x86_64_propagation.log.*`

export VER=9.3
awk -v shasum=$(curl -s https://dl.rockylinux.org/pub/rocky/$VER/BaseOS/x86_64/os/repodata/repomd.xml | sha256sum | awk '{print $1}') -F'::' '{split($0,data,":")} {if ($4 != shasum) {print data[5], data[6], $2, $7}}' < $(find /var/log/mirrormanager/propagation/ -name "rocky-${VER}-BaseOS-x86_64_propagation.log*" -mtime -1 | tail -1)'
```

This will generate a table. You can take the IDs in the first column and use the database to disable them by ID (table name: hosts) or go to https://mirrors.rockylinux.org/mirrormanager/host/ID and uncheck 'User active'.

Users can change user active, *but* they cannot change admin active. It is better to flip user active in this case.

Admins can also view https://mirrors.rockylinux.org/mirrormanager/admin/all_sites if necessary.

Example of table columns:

!!! Note

    These mirrors are here soley as an example and not to call anyone out, every mirror shows up on here at one point, for some reason, due to natural variations in how mirrors sync.

```
[mirrormanager@ord1-prod-mirrormanager001 propagation]$ awk -v shasum=$(curl -s https://dl.rockylinux.org/pub/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml | sha256sum | awk '{print $1}') -F'::' '{split($0,data,":")} {if ($4 != shasum) {print data[5], data[6], $2, $7}}' < rocky-9.3-BaseOS-x86_64_propagation.log.1660611632 | column -t
164  mirror.host.ag            http://mirror.host.ag/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml             404
173  rocky.centos-repo.net     http://rocky.centos-repo.net/9.3/BaseOS/x86_64/os/repodata/repomd.xml            403
92   rocky.mirror.co.ge        http://rocky.mirror.co.ge/9.3/BaseOS/x86_64/os/repodata/repomd.xml               404
289  mirror.vsys.host          http://mirror.vsys.host/rockylinux/9.3/BaseOS/x86_64/os/repodata/repomd.xml      404
269  mirrors.rackbud.com       http://mirrors.rackbud.com/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml        200
295  mirror.ps.kz              http://mirror.ps.kz/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml               200
114  mirror.liteserver.nl      http://rockylinux.mirror.liteserver.nl/9.3/BaseOS/x86_64/os/repodata/repomd.xml  200
275  mirror.upsi.edu.my        http://mirror.upsi.edu.my/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml         200
190  mirror.kku.ac.th          http://mirror.kku.ac.th/rocky-linux/9.3/BaseOS/x86_64/os/repodata/repomd.xml     404
292  mirrors.cat.pdx.edu       http://mirrors.cat.pdx.edu/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml        200
370  mirrors.gbnetwork.com     http://mirrors.gbnetwork.com/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml      404
308  mirror.ihost.md           http://mirror.ihost.md/rockylinux/9.3/BaseOS/x86_64/os/repodata/repomd.xml       404
87   mirror.freedif.org        http://mirror.freedif.org/Rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml         404
194  mirrors.bestthaihost.com  http://mirrors.bestthaihost.com/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml   404
30   mirror.admax.se           http://mirror.admax.se/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml            200
195  mirror.uepg.br            http://mirror.uepg.br/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml             404
247  mirrors.ipserverone.com   http://mirrors.ipserverone.com/rocky/9.3/BaseOS/x86_64/os/repodata/repomd.xml    404'
```
