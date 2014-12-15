# Kitout

Describe the configuration of a Mac OS workstation as a set of small shell scripts, then use
kitout to run those scripts and ensure the machine's configuration matches its description.

## Prerequsites

A clean install of the latest version of OS X (10.10 Yosemite). At this time no effort is made to
support previous versions.

## Kitout a Workstation

Install:

    # Create the directory where kitout will live
    sudo mkdir -p /usr/local/opt
    # Ensure non-root user can read/write everything in /usr/local
    sudo chown -R $USER /usr/local
    # Download kitout, the system may prompt you to install git
    git clone https://github.com/kitout/core.git /usr/local/opt/kitout
    cd /usr/local/opt/kitout
    # Run the installer
    ./install

Uninstall

    rm -rf /usr/local/opt/kitout /usr/local/bin/kitout
