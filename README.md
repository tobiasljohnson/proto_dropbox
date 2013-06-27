proto_dropbox
=============

A full-featured command line interface to Dropbox.

This python script provides a command-line interface to your Dropbox
account. I wrote it because I couldn't install Dropbox on the chroot I was running
on my ARM Chromebook. This script does not run in the background. Rather,
you run it, and it synchronizes your local Dropbox directory with your
Dropbox account. It will upload files if they've changed locally and download
them if they've changed on Dropbox. If they've changed on both, it will
rename your local copy in the usual style of Dropbox (e.g.,
"quant_ham_markov.tex (Tobias Johnson's conflicted copy)"), upload it,
and download the version in your Dropbox account. It will also delete
files, either locally or on Dropbox, as appropriate. In general, it
strives to reproduce the behavior of Dropbox as much as possible,
besides the part about running all the time and not being annoyingly slow.

The script will create new directories as appropriate, both locally and
on Dropbox. It refuses to delete directories, because that just seemed
too terrifying. If you delete a directory locally or on Dropbox, it will
warn you that you've done so when you sync, and tell you to delete the
other location's directory yourself. If don't do it, it will either
re-upload or re-download it the next time you synchronize.

The script tracks changes locally just by looking at file modification
times. For your Dropbox account, it tracks the changes the right way,
usingthe mechanism built into Dropbox. To interface with your Dropbox
account, it uses the Dropbox Core API, so it's safe and works nicely.

Installation
============

You will need Python 2.? (I don't know what version exactly is 
necessary). You'll need the dropbox and python-dateutil modules
for Python. Then just put the script somewhere in your path and
try it out. The first time you run it, it will ask you where you
want to keep your local Dropbox directory (~/Dropbox is a good
choice). You can also choose to only synchronize with a directory
in your Dropbox account, instead of the whole thing (put in
somewhere specific in instead of just / when it asks). The script
will instruct you to create a Dropbox App and enter its information
in, and to allow it access. Just follow the instructions, and it will
store all the information in a file called .proto_dropbox_account in
your home directory.

Some fuller instructions, if you're using some sort of Linux in which
you can install things by typing "sudo apt-get install ...", like Ubuntu
or Debian:

Download the script, put it somewhere in your path such as ~/bin,
and mark it as executable (chmod u+x proto_dropbox).

You almost certainly already have Python, but if not, type

sudo apt-get install python2.7

If you don't already have a program called "easy_install",
install it:

sudo apt-get install python-setuptools

Now, run the following two commands:

sudo easy_install dropbox
sudo easy_install python-dateutil

Now, run proto_dropbox and follow its directions.

Usage
=====

Typically, you just run the script and it synchronizes. If you want proto_dropbox
to completely ignore some files, make a new file called .proto_dropbox_ignore in the
directory where the file is found. Put the name of each file that you want to
be ignored in this file, one per line. You can also ignore directories this way;
proto_dropbox won't enter that directory and will ignore all of its subdirectories
too.

If you do the exact same thing with a file called .proto_dropbox_optional, proto_dropbox
will ignore whatever files and directories you list here unless you run it with the -a (long
form: --all) command. This is useful for speeding up the script: you can tell it to ignore
directories that are rarely updated.

You can see a few other command line options by running proto_dropbox with the -h or --help 
option.
