systemd-user-units
==================

Configuring `systemd --user` it's not an easy task, and to be honest I'm by no
means an expert for this matter. Right now my system works and these units get
loaded, but since the support for upstream is inexistent (yet &mdash;I hope&mdash;),
there's not too much you can do about it.

I've learn everything from [http://pbrisbin.com/posts/systemd-user][pbrisbin
excellent article] and of course, the awesome [https://wiki.archlinux.org/index.php/Systemd/User][Arch Linux Wiki].

To use this units, you **need** the [https://github.com/sofar/xorg-launch-helper][xorg-launch-helper].

Structure and how-to (sort of)
------------------------------

The `default.target` file is a symlink that points to `wm.target`, this is
important since the latter makes sure the window manager gets load before the
startup files. While you can make the links by hand, I **strongly** discourage
that since systemd can be a little picky about it.

    $ systemctl --user enable wm.target
    ln -s '/home/pablo/.config/systemd/user/wm.target' '/home/pablo/.config/systemd/user/default.target'
    $ systemctl --user enable xmonad@:0
    ln -s '/home/pablo/.config/systemd/user/xmonad@.service' '/home/pablo/.config/systemd/user/wm.target.wants/xmonad@:0.service'

Notes
-----

While in most places it says that `systemd --user` can work since the startup,
and the DISPLAY variable can be put somewhere else so you don't need the `:0`
thingie, I could never made it work, so I kept the `@` units. To use them,
remove the `*.target.wants` directory, and add them

    $ systemctl --user enable a-service@:0

Where `:0` is the `DISPLAY`, unless you use more than one display (I do to play
with steam or other games), you should never use something different.

If you list the units (`systemctl --user list-unit-files`) you'll realize that
the startup.target is disabled, nevertheless it's working... I don't know if
it's a bug or the command it's supposed to list the units that are explicitily
enabled (`startup.target` it's not because it's *wanted* from the `wm.target`).

Advices?
--------

Honestly, as a lot of (smart) people has already pointed out, don't get
discouraged if something goes wrong. Keep trying, keep the variables at minimum,
make something little work and then go with baby steps adding complexity.

Acknowledgments
---------------

+ https://bitbucket.org/KaiSforza/systemd-user-units
+ https://github.com/lemenkov/systemd-user-units
+ https://github.com/sofar/user-session-units
+ https://github.com/zoqaeski/systemd-user-units

