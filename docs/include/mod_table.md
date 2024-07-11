=== "Rocky Linux 8"

    | Package Name          | Change Type     | Comment                                             |
    |-----------------------|-----------------|-----------------------------------------------------|
    | shim-unsigned-x64     | Self-managed    | Secure Boot                                         |
    | shim-unsigned-aarch64 | Self-managed    | Secure Boot                                         |
    | shim                  | Self-managed    | Secure Boot                                         |
    | fwupd                 | Patch           | Secure Boot                                         |
    | grub2                 | Patch           | Secure Boot                                         |
    | kernel                | Patch, Branding | Secure Boot                                         |
    | kernel-rt             | Patch, Branding | Secure Boot                                         |
    | rocky-release         | Self-managed    | Required for Rocky Linux to be itself                   |
    | rocky-logos           | Self-managed    | Required for Rocky Linux assets                         |
    | rocky-indexhtml       | Self-managed    | Required for Rocky Linux default index                  |
    | rocky-bookmarks       | Self-managed    | Required for Rocky Linux default browser bookmarks      |
    | abrt                  | Branding        | Add Rocky Support                                   |
    | anaconda              | Patch, Branding | Turn off Red Hat specific options                   |
    | anaconda-user-help    | Branding        | Ensure documenation references Rocky Linux          |
    | cockpit-composer      | Branding        | Replace RHEL with Enterprise Linux                  |
    | cloud-init            | Patch           | Ensure the managed user is cloud-user like upstream |
    | crash                 | Patch           | Replace Red Hat with Rocky                          |
    | dhcp                  | Patch           | Change bug tracker URL                              |
    | dnf                   | Patch           | Change bug tracker URL                              |
    | dotnet                | Branding        | Add Rocky Support                                   |
    | dotnet3.0             | Branding        | Add Rocky Support                                   |
    | firefox               | Patch           | Replace Red Hat settings with Rocky Linux settings  |
    | gcc                   | Patch           | Change bug tracker URL                              |
    | gdb                   | Patch           | Replace Red Hat with Rocky Linux                    |
    | gnome-boxes           | Patch           | Add Rocky Support                                   |
    | gnome-settings-daemon | Patch           | Remove subscription manager patch                   |
    | initial-setup         | Branding        | Replace Red Hat with Rocky Linux                    |
    | java-1.8.0-openjdk*   | Patch           | Ensure portables are buildable on all releases      |
    | java-11-openjdk*      | Patch           | Ensure portables are buildable on all releases      |
    | java-17-openjdk*      | Patch           | Ensure portables are buildable on all releases      |
    | java-21-openjdk*      | Patch           | Ensure portables are buildable on all releases      |
    | libdnf                | Patch           | Change bug tracker URL                              |
    | libguestfs            | Patch           | Add Rocky Support                                   |
    | libreoffice           | Branding        | Remove Red Hat branding to generic branding         |
    | libreport             | Patch           | Ensure Rocky Bug Tracker (mantis) is supported      |
    | lorax-templates-rocky | Self-managed    | Replacement for lorax-templates-rhel                |
    | nginx                 | Branding        | Replace Red Hat with Rocky Linux                    |
    | openscap              | Patch           | Ensure Rocky Linux is supported as a derivative     |
    | osbuild               | Patch           | Ensure Rocky Linux is supported                     |
    | osbuild-composer      | Patch           | Ensure Rocky Linux is supported                     |
    | oscap-anaconda-addon  | Branding        | Replace "Red Hat" with "Rocky"                      |
    | PackageKit            | Patch           | Change support URL's to Rocky Linux wiki            |
    | pcs                   | Branding        | Replace "Red Hat" logo                              |
    | plymouth              | Branding        | Replace "Red Hat Enterprise Linux"                  |
    | python2               | Patch           | Add Rocky Support                                   |
    | python3               | Patch           | Add Rocky Support                                   |
    | python-pip            | Patch           | Add Rocky Support                                   |
    | redhat-rpm-config     | Patch           | Add Rocky Support                                   |
    | scap-security-guide   | Patch           | Ensure Rocky Linux is supported as a derivative     |
    | subscription-manager* | Patch           | Remove Red Hat references                           |
    | systemd               | Patch           | Change support URL's to Rocky Linux                 |
    | thunderbird           | Patch           | Replace Red Hat settings with Rocky Linux settings  |
    | toolbox               | Patch           | Ensure Rocky Linux image is the default             |
    | WALinuxAgent          | Patch           | Ensure Rocky Linux is supported                     |

=== "Rocky Linux 9"

    | Package Name          | Change Type     | Comment                                                 |
    |-----------------------|-----------------|---------------------------------------------------------|
    | shim-unsigned-x64     | Self-managed    | Secure Boot                                             |
    | shim-unsigned-aarch64 | Self-managed    | Secure Boot                                             |
    | shim                  | Self-managed    | Secure Boot                                             |
    | fwupd                 | Patch           | Secure Boot                                             |
    | grub2                 | Patch           | Secure Boot                                             |
    | kernel                | Patch, Branding | Secure Boot                                             |
    | kernel-rt             | Patch, Branding | Secure Boot                                             |
    | rocky-release         | Self-managed    | Required for Rocky Linux to be itself                   |
    | rocky-logos           | Self-managed    | Required for Rocky Linux assets                         |
    | rocky-indexhtml       | Self-managed    | Required for Rocky Linux default index                  |
    | rocky-bookmarks       | Self-managed    | Required for Rocky Linux default browser bookmarks      |
    | anaconda              | Patch, Branding | Turn off Red Hat specific options                       |
    | anaconda-user-help    | Branding        | Ensure documenation references Rocky Linux              |
    | cloud-init            | Patch           | Ensure the managed user is cloud-user like upstream     |
    | cockpit-composer      | Branding        | Replace RHEL with Enterprise Linux                      |
    | crash                 | Patch           | Replace Red Hat with Rocky                              |
    | dhcp                  | Patch           | Change bug tracker URL                                  |
    | dnf                   | Patch           | Change bug tracker URL                                  |
    | firefox               | Patch           | Replace Red Hat settings with Rocky Linux settings      |
    | gcc                   | Patch           | Change bug tracker URL                                  |
    | gdb                   | Patch           | Replace Red Hat with Rocky Linux                        |
    | gnome-settings-daemon | Patch           | Remove subscription manager patch                       |
    | initial-setup         | Branding        | Replace Red Hat with Rocky Linux                        |
    | java-1.8.0-openjdk*   | Patch           | Ensure portables are buildable on all releases          |
    | java-11-openjdk*      | Patch           | Ensure portables are buildable on all releases          |
    | java-17-openjdk*      | Patch           | Ensure portables are buildable on all releases          |
    | java-21-openjdk*      | Patch           | Ensure portables are buildable on all releases          |
    | libdnf                | Patch           | Change bug tracker URL                                  |
    | libreoffice           | Branding        | Remove Red Hat branding to generic branding             |
    | libreport             | Patch           | Ensure Rocky Bug Tracker (mantis) is supported          |
    | lorax-templates-rocky | Self-managed    | Replacement for lorax-templates-rhel                    |
    | nginx                 | Branding        | Replace Red Hat with Rocky Linux                        |
    | openldap              | Patch           | Ensure openldap-servers is available in plus repo       |
    | openscap              | Patch           | Ensure Rocky Linux is supported as a derivative         |
    | osbuild               | Patch           | Ensure Rocky Linux is supported                         |
    | osbuild-composer      | Patch           | Ensure Rocky Linux is supported                         |
    | PackageKit            | Patch           | Change support URL's to Rocky Linux wiki                |
    | python-pip            | Patch           | Add Rocky Support                                       |
    | redhat-rpm-config     | Patch           | Add Rocky Support                                       |
    | rust                  | Patch           | Ensure that aarch64 and s390x can build rust (OOM)      |
    | scap-security-guide   | Patch           | Ensure Rocky Linux is supported as a derivative         |
    | subscription-manager* | Patch           | Remove Red Hat references                               |
    | systemd               | Patch           | Change support URL's to Rocky Linux                     |
    | thunderbird           | Patch           | Replace Red Hat settings with Rocky Linux settings      |
    | toolbox               | Patch           | Ensure Rocky Linux image is the default                 |
    | WALinuxAgent          | Patch           | Ensure Rocky Linux is supported                         |

=== "Rocky Linux 10"

    | Package Name          | Change Type     | Comment                                                     |
    |-----------------------|-----------------|-------------------------------------------------------------|
    | shim-unsigned-x64     | Self-managed    | Secure Boot                                                 |
    | shim-unsigned-aarch64 | Self-managed    | Secure Boot                                                 |
    | shim                  | Self-managed    | Secure Boot                                                 |
    | fwupd-efi             | Patch           | Secure Boot                                                 |
    | grub2                 | Patch           | Secure Boot                                                 |
    | kernel                | Patch, Branding | Secure Boot and Branding                                    |
    | kernel-rt             | Patch, Branding | Secure Boot and Branding                                    |
    | rocky-release         | Self-managed    | Required for Rocky Linux to be itself                       |
    | rocky-logos           | Self-managed    | Required for Rocky Linux assets                             |
    | rocky-indexhtml       | Self-managed    | Required for Rocky Linux default index                      |
    | rocky-bookmarks       | Self-managed    | Required for Rocky Linux default browser bookmarks          |
    | anaconda              | Patch, Branding | Turn off Red Hat specific options                           |
    | anaconda-user-help    | Branding        | Ensure documenation references Rocky Linux                  |
    | cloud-init            | Patch           | Ensure the managed user is cloud-user like upstream         |
    | cockpit-composer      | Branding        | Replace RHEL with Enterprise Linux                          |
    | crash                 | Patch           | Replace Red Hat with Rocky                                  |
    | dnf                   | Patch           | Change bug tracker URL                                      |
    | firefox               | Patch           | Replace Red Hat settings with Rocky Linux settings          |
    | gcc                   | Patch           | Change bug tracker URL                                      |
    | gdb                   | Patch           | Replace Red Hat with Rocky Linux                            |
    | java-17-openjdk*      | Patch           | Ensure portables are buildable on all releases              |
    | java-21-openjdk*      | Patch           | Ensure portables are buildable on all releases              |
    | libdnf                | Patch           | Change bug tracker URL                                      |
    | lorax-templates-rocky | Self-managed    | Replacement for lorax-templates-rhel                        |
    | nginx                 | Branding        | Replace Red Hat with Rocky Linux                            |
    | openldap              | Patch           | Ensure openldap-servers is built and available in plus repo |
    | openscap              | Patch           | Ensure Rocky Linux is supported as a derivative             |
    | osbuild               | Patch           | Ensure Rocky Linux is supported                             |
    | osbuild-composer      | Patch           | Ensure Rocky Linux is supported                             |
    | PackageKit            | Patch           | Change support URL's to Rocky Linux wiki                    |
    | redhat-rpm-config     | Patch           | Add Rocky Support                                           |
    | rust                  | Patch           | Ensure that aarch64 and s390x can build rust (OOM)          |
    | scap-security-guide   | Patch           | Ensure Rocky Linux is supported as a derivative             |
    | subscription-manager* | Patch           | Remove Red Hat references                                   |
    | systemd               | Patch           | Change support URL's to Rocky Linux                         |
    | thunderbird           | Patch           | Replace Red Hat settings with Rocky Linux settings          |
    | toolbox               | Patch           | Ensure Rocky Linux image is the default                     |
