# Kitout

Describe the configuration of a Mac OS workstation as a set of small shell scripts, then use kitout
to run those scripts and ensure the machine's configuration matches its description.

## Prerequisites

A clean install of the latest version of OS X (10.10 Yosemite). At this time no effort is made to
support previous versions.

## Kitout a Workstation

Ensure the correct paths exist and have the right permissions:

    # Create the directory where kitout will live
    sudo mkdir -p /usr/local/opt
    # Ensure non-root user can read/write everything in /usr/local
    sudo chown -R $USER /usr/local

Install:

    # Download kitout, the system may prompt you to install git
    git clone https://github.com/kitout/kitout.git /usr/local/opt/kitout
    cd /usr/local/opt/kitout
    # Run the installer
     ./install

Uninstall

    rm -rf /usr/local/opt/kitout /usr/local/bin/kitout

## Developing Kitout

First delete `/usr/local/opt/kitout`. Now clone `kitout/kitout` somewhere:

    cd ~/Projects
    git clone https://github.com/kitout/kitout.git

Then clone `kitout/core` and `kitout/units-developer`:

    cd kitout
    git clone https://github.com/kitout/core.git
    git clone https://github.com/kitout/units-developer.git

Whether you're working on `kitout/kitout`, `kitout/core` or `kitout/units-developer` always work
from a branch and use the github pull request workflow for contributing.

## Writing Units

To create a new unit, run:

    /usr/local/opt/kitout/core/kitout-install-unit local my-unit

The appropriate unit files will be generated for you and you'll be dropped into `/usr/local/opt/kitout/units-local/my-unit`.

Units have four files (that kitout cares about):

* `readme.md` - Describes what the unit does
* `install.sh` - A bash script that includes commands for installing the unit
* `installed.sh` - A bash script that exits with zero if the unit is installed
* `deps` - A text file that lists each dependency of this unit on its own line

To install a unit run:

    /usr/local/opt/kitout/core/kitout-install-unit my-unit

The `/usr/local/bin/kitout` executable runs `kitout-install-unit` for every unit listed in the `/usr/local/opt/kitout/units` text file.

## The Way of Kitout

Kitout is inspired by systems like [boxen], [sprout-wrap], [sprout], [chef], [babushka] and
[puppet].

Kitout is not better, just simpler. Kitout has no external dependencies. Kitout has no server.
There are no conferences about Kitout. There are no consultancies that offer Kitout services.

Kitout is designed to be a simple tool for people in the software education space to quickly set
up new workstations. It is not the right tool for many, many other use cases.

The authors of kitout currently have one or two hours a month available to maintain it. So if you
want to do some Serious Business with kitout the right move is probably to use it as a starting
point or inspiration for building your own system.

[boxen]: http://boxen.github.com
[sprout-wrap]: https://github.com/pivotal-sprout/sprout-wrap
[sprout]: https://github.com/pivotal-sprout/sprout
[babushka]: http://babushka.me
[chef]: http://www.opscode.com/chef
[puppet]: http://puppetlabs.com
