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

Then run it

    ./kitout

## Kitout Your Machine

If you are installing Kitout on a personal machine, it is likely that you will
want to blacklist some Kitout defaults. To do so, first install kitout as
[described above](#kitout-a-workstation). Before running `./kitout` create
a `_blacklist` file, which is a list of units you want to exclude from
installation on your machine. You don't need to blacklist programs you
already have installed.  Kitout will check for a package before installing it.
An example `_blacklist` might look like:

    # Don't allow people to ssh into my machine
    remote-login
    # I installed chrome with the installer so it's in /Applications
    google-chrome
    # Same for sublime
    sublime

Now run kitout and blacklisted units will be skipped

    ./kitout


## Usage & Development

This is how you modify Kitout:

First, create a unit: `./create-unit unit-name`. A unit is a folder. A unit
sets up **one** aspect of the machine. That's why it's called a unit.

Units have four kind of files (that kitout cares about):

* `readme.md` - Describes why this unit is included in Kitout.
* `install.sh` - A shell script which installs the unit.
* `installed.sh` - A shell script which exits with zero when the unit is installed.
* `deps` - A file which lists the dependencies this unit has on other units.

Run your unit with `install-unit unit-name`. Run all units with `kitout`.

For convenience, you can specify which units should run first (instead of
just letting kitout work it out from the dependency graph) by putting their
names in the `priority-units` file.

## The Way of Kitout

Kitout is inspired by systems like [boxen], [sprout-wrap], [sprout], [chef],
[babushka] and [puppet].

Kitout is not better, just simpler. Kitout has no external dependencies. Kitout
has no server. There are no conferences about Kitout. There are no consultancies
that offer Kitout services. Kitout is designed to be simple enough to setup
workstations and student machines. It is not the right tool for many, many other
use cases.

Kitout is okay with units that require manual operator intervention as Kitout is
as much about documentation as it is about automation. In fact, if Kitout was a
person it would suggest you first write your units as the kind that instruct the
operator _before_ you investigate how to make them automated and non-interactive.

[boxen]: http://boxen.github.com
[sprout-wrap]: https://github.com/pivotal-sprout/sprout-wrap
[sprout]: https://github.com/pivotal-sprout/sprout
[babushka]: http://babushka.me
[chef]: http://www.opscode.com/chef
[puppet]: http://puppetlabs.com

## Dev on a VM

[Download VMware Fusion](http://www.vmware.com/products/fusion/)

Follow the VMWare Fusion's instructions to install OS X on the Virtual Machine.

As soon as the installer has finished, **take a snapshot of the VM in its
freshly installed state**. This snapshot will allow you to rooll back to a freshly installed state if a configuration state gets messed up.

Follow the install instructions above but instead of `./kitout` run

    sh remote-login/install.sh

Now you want edit the kitout repo on the host machine and sync the files over
to the VM before running them with something like:

    scp -r ~/kitout username@192.168.0.0:/usr/local/opt/kitout
