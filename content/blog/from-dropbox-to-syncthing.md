{
    "date": "2014-11-25",
    "description": "Why I switched from Dropbox to Syncthing and how I organized my synchronized folders.",
    "slug": "from-dropbox-to-syncthing",
    "tags": "file synchronization, dropbox, syncthing, go",
    "title": "From Dropbox to Syncthing"
}

I've used [Dropbox](https://www.dropbox.com/) since about two and a half
years on my desktop and mobile devices and was pretty satisfied with it
most of the time. But, even before Condoleezza Rice \[1\]\_ was part of
[Dropbox's directors
board](https://blog.dropbox.com/2014/04/growing-our-leadership-team/), I
had a bad feeling about my data lying on their servers. Another downside
was the linux client, fortunately it's not half as bad as
[skype](http://www.skype.com/de/download-skype/skype-for-linux/). The
trayicon always disappeared under
[Gnome3](http://www.gnome.org/gnome-3/), clicking on a single
notification opened up a dozen instances of
[Nautilus](https://github.com/GNOME/nautilus) and using closed-source
software means that I had to install it manually or use it from the
[Arch User
Repository](https://wiki.archlinux.org/index.php/Arch_User_Repository)
which I try to avoid whenever possible. Also the android client seems to
forget my login credentials from time to time.

One might say that I only had to encrypt my Dropbox folder in order to
reduce my worries about security. That's what I've done and it works
well using [EncFS](http://en.wikipedia.org/wiki/EncFS) as long as you
don't use your Dropbox on different Operating Systems, especially on
mobile. Distrusting a service provider and at the same time using a
service that they offer is wrong from the ground up.

Not everything was bad, the synchronization worked well and I can't
remember a moment when their servers were down. Getting public links for
my files was also a nice feature. The camera upload feature was also a
nice addition.

Besides the security point of view I would have really liked to share
folders only with read access and to get a lot more storage space. I
could have solved the last two points with a premium account but this
wasn't an option because I had enough unused server resources to host my
own synchronization service.

I thought about using [Seafile](http://seafile.com/en/home/) or
[ownCloud](http://owncloud.org/), but both need a central server to work
and the latter is written in [php](http://php.net/), that is reason
enough for me to avoid it. Both of them are full-blown software and
provide a lot more features than simple file synchronisation, which
isn't bad at all, but I don't need those goodies.

The main difference between conventional file hosting services like
Dropbox and Syncthing is, that Syncthing is serverless. This means that
you can only sync files between two instances if both of them are online
at the same time. That's not very practical so I still have to use a
server that is running another Syncthing instance. Its lightweight,
doensn't have external dependencies and the basic configuration is done
in under a minute, so it doesn't hurt that much.

Installation
============

For Arch Linux: `pacman -S syncthing`. If you are running Windows
download the latest release from the [official github
repo](https://github.com/syncthing/syncthing/releases) and I strongly
recommend to download
[SyncthingTray](https://github.com/iss0/SyncthingTray/releases) as well.
To run it automatically after boot enable the systemd service:
`systemctl enable syncthing@USERNAME`.

Configuration
=============

Now you are ready to start (the service)
`systemctl start syncthing@USERNAME`. This should open your browser with
`http://localhost:8080/` and shows you the dashboard with looks
something like the image shown below, except that you should see only
one folder and device. The default sync folder is located under
`~/Sync`, you can remove it after configuring at least one more folder.

![the syncthing dashboard](/imgs/syncthing_dashboard.png){.kn-image}

At first it is not very easy to synchronize a folder but if you note
these few tips it will not be a problem at all \[2\]\_:

-   

    Add the devices you want to synchronize with by using the `Add Device` button in your Syncthing dashboard.

    :   Devices are identified by an ID that you can show when you click
        on the *gear* symbol in the menu bar of the dashboard. You need
        the IDs of all the machines that you want to add and vice versa.

-   

    Add the folder you want to synchronize and in the following dialog enable all the devices that you want to synchronize with.

    :   This has to be done on the other devices as well, but note that
        the *name* of the folder, not the path, has to be the same
        across all devices. By enabling the *Folder Master* option you
        can synchronize the folder as *read-only*.

-   You have to restart Syncthing after every folder you added, this can
    also be done from the menu bar of the dashboard

Maybe you've asked yourself how the devices can talk to each other? The
answer is the *Global Discovery Server*, that is used to share the
addresses between the devices. If you want you can [run one by
yourself](https://github.com/syncthing/discosrv).

There is a lot of progress in the project on github, so I am hopeful
that this procedure will be way more user friendly in the near future.

Directory structure
-------------------

I have decided to add every subfolder of my base folder (`syncthing`) as
seperate entry in my syncthing configuration. This lets me choose the
optimal synchronization options for each folder. To see directly which
folders are shared with other people I have created the `shared`
subfolder.

```
syncthing
   ├── audio
   ├── docs
   ├── dump
   ├── edu
   ├── gfx
   │   ├── Pixel
   │   └── Vector
   ├── mobile
   ├── shares
   │   ├── LT
   │   └── uni
   └── wallpaper
```

Symbolic links will be copied as is, that means that Syncthing won't
follow those links and synchronize their content as Dropbox does. Maybe
this [behaviour has
changed](https://github.com/syncthing/syncthing/issues/873) in version
[0.10.8](https://github.com/syncthing/syncthing/releases/tag/v0.10.8).
<span
class="strike">Because the Arch package is exceptionally out-of-date I will update this post when the newest version is available.</span>

Update
------

Syncthing 0.10.8 is out and symlinks won't be followed, they will be
copied as is and this should work for Windows as well.

------------------------------------------------------------------------
