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
     ./install https://github.com/USER-OR-ORG/KITOUT-CONFIG-REPO

Uninstall

    rm -rf /usr/local/opt/kitout /usr/local/bin/kitout*

## Developing Kitout

Install kitout as above, then move `/usr/local/opt/kitout` somewhere:

    cd ~/Projects
    mv /usr/local/opt/kitout .

Then symlink kitout:

    ln -s ~/Projects/kitout /usr/local/opt/kitout

The `/`, `/core`, `/units-*` and `/config` folders in kitout are already their own git repos. If
you're working on a fork then you'll need to update the appropriate remotes to point to your forks.

## Writing Units

To create a new unit, run:

    kitout-create-unit local my-unit

The appropriate unit files will be generated for you at `/usr/local/opt/kitout/units-local/my-unit`.

Units have four files (that kitout cares about):

* `readme.md` - Describes what the unit does
* `install.sh` - A bash script that includes commands for installing the unit
* `installed.sh` - A bash script that exits with zero if the unit is installed
* `deps` - A text file that lists each dependency of this unit on its own line

To install a unit run:

    kitout-install-unit my-unit

To install all units `/usr/local/opt/kitout/config/units` just run:

    kitout

## The Way of Kitout

Kitout is inspired by systems like [boxen], [sprout-wrap], [sprout], [chef], [babushka] and
[puppet].

Kitout is not better, just simpler. Kitout has no external dependencies. Kitout has no server.
There are no conferences about Kitout. There are no consultancies that offer Kitout services.

Kitout is designed to be a simple tool for people in the software education space to quickly set
up new workstations. It is not the right tool for many, many other use cases.

The authors of kitout currently have 1-2 hours a month available to maintain it. So if you
want to do some Serious Business with kitout, the right move is probably to use it as a _starting
point_ or inspiration for building your own system.

[boxen]: http://boxen.github.com
[sprout-wrap]: https://github.com/pivotal-sprout/sprout-wrap
[sprout]: https://github.com/pivotal-sprout/sprout
[babushka]: http://babushka.me
[chef]: http://www.opscode.com/chef
[puppet]: http://puppetlabs.com

## Contributing Notes

Kitout is written in bash.

Every source file [ends with a blank line](http://unix.stackexchange.com/questions/18743/whats-the-point-in-adding-a-new-line-to-the-end-of-a-file).

Use [`#!/usr/bin/env bash`](http://en.wikipedia.org/wiki/Shebang_(Unix)#Portability) as the shebang.

Each script should begin with the ["unoffical bash strict mode"](http://redsymbol.net/articles/unofficial-bash-strict-mode/): `set -euo pipefail ; IFS=$'\n\t'`

Some scripts will need to know their expanded, non-symlink path. To retreive that use:

```
realpath="$(python -c 'import os,sys; print os.path.realpath(sys.argv[1])' "$0")"
```

(Yes. Its a hideous hack.)

Alias numeric arguments like `$1` to names that describe their contents like `$unit_name`. It's ok
to use $0.
