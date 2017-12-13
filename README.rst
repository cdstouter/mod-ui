mod-ui
======

This is my custom fork of the mod-ui repo. It adds a few new HTTP requests, as well as a few other changes.
I'm using it alongside ModMidi (https://github.com/cdstouter/ModMidi), a project to control the MOD seamlessly with a
Behringer FCB1010 controller. Here's my process to build & deploy the mod-ui package:

First, set up the Docker image, and a few other things::

    $ docker build docker-mod-ui

Note the id of the Docker image that was just created. Now build the package::

    $ docker run -ti -v PATH_TO_MOD_UI_REPO:/tmp/mod-ui DOCKER_IMAGE_ID
    (inside the Docker context)
    $ cd /tmp/mod-ui
    $ python3.4 setup.py clean
    $ python3.4 setup.py bdist -p mod-duo
    $ exit

Now that the package is built (it's in the dist folder) deploy it to the MOD::

    $ scp dist/mod-0.99.8.mod-duo.tar.gz root@modduo.local:~/
    $ ssh root@modduo.local
    (now we're in the MOD, and you probably want to back up the current mod-ui package)
    $ cd /
    $ tar -czf ~/mod-ui-backup.tar.gz /usr/bin/mod-ui /usr/lib/python3.4/site-packages/mod* /usr/share/mod
    (now we deploy the new package)
    $ mount -o remount,rw /
    $ systemctl stop mod-ui
    $ cd /
    $ tar -xzf ~/mod-0.99.8.mod-duo.tar.gz
    $ systemctl start mod-ui
    $ mount -o remount,ro /

If you want to see the output of mod-ui for debugging purposes, you can run it like this instead of using systemctl::

    $ mod-ui.run

Original documentation below:
-----------------------------

mod-ui
======

This is the UI for the MOD software. It's a webserver that delivers an HTML5 interface and communicates with mod-host.
It also communicates with the MOD hardware, but does not depend on it to run.

Install
-------

There are instructions for installing in a 64-bit Debian based Linux environment.
It will work in x86, other Linux distributions and Mac, but you might need to adjust the instructions.

The following packages will be required::

    $ sudo apt-get install phantomjs python-virtualenv python3-pip python3-dev git build-essential liblilv-dev

Start by cloning the repository::

    $ git clone git://github.com/moddevices/mod-ui
    $ cd mod-ui

Create a python virtualenv::

    $ virtualenv modui-env
    $ source modui-env/bin/activate

Install python requirements::

    $ pip3 install -r requirements.txt

Compile libmod_utils::

    $ cd utils
    $ make
    $ cd ..

Run
---

Before running the server, you need to activate your virtualenv
(if you have just done that during installation, you can skip this step, but you'll need to do this again when you open a new shell)::

    $ source modui-env/bin/activate

Run the server::

    $ ./server.py

Open your webkit based browser (I use Chromium) and point to http://localhost:8888
