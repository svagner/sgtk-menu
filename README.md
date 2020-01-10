# sway-gtk-menu
This project is an attempt to create a simple menu, that behaves decently on sway and i3 window manager. 
It uses `pygobject` to create a themeable, searchable gtk3-based system menu w/ some optional features.

## Features

- `.desktop` entries-based system menu;
- search box to find what you need quickly;
- favourites (most frequently used entries) menu above (optional argument);
- user-defined menu below (optional argument).

As the script uses `.desktop` files only, it may be used as a replacement to wofi/rofi `--drun`, but not for `--dmenu` mode.

![screenshot](http://nwg.pl/Lychee/uploads/big/fbe90b3f5c8d192657a7e9ad98310d84.png)
*The menu in Clearlooks GTK theme w/ Aqatix icons*

## Background

Well, I didn't even think that sway needed a menu, being happy with [wofi](https://hg.sr.ht/~scoopta/wofi) and 
[dmenu-wayland](https://github.com/nyyManni/dmenu-wayland). I started coding just to find out what the freedesktop 
[Desktop Menu Specification](https://specifications.freedesktop.org/menu-spec/latest) looks like, and also to learn some 
more [pygobject](https://pygobject.readthedocs.io/en/latest). [The best menu I know](https://github.com/johanmalm/jgmenu), 
however, does not (yet?) behave well on sway. So, I thought to share the code, which has already taken me more time 
that I had ever expected.

[This code](https://github.com/johanmalm/jgmenu/blob/master/contrib/pmenu/jgmenu-pmenu.py) by 
[Johan Malm](https://github.com/johanmalm) helped me understand how to make use of `.desktop` entries. Many thanks!

## Usage

```text
$ /path/to/the/script/menu.py -h
usage: menu.py [-h] [-b] [-f | -fn FN] [-a | -af AF] [-l L] [-s S] [-w W] [-d D] [-o O] [-t T]

GTK menu for sway and i3

optional arguments:
  -h, --help        show this help message and exit
  -b, --bottom      display menu at the bottom
  -f, --favourites  prepend 5 most used items
  -fn FN            prepend <FN> most used items
  -a, --append      append custom menu from /home/piotr/.config/sgtk-menu/appendix
  -af AF            append custom menu from /home/piotr/.config/sgtk-menu/<AF>
  -l L              force language (e.g. "de" for German)
  -s S              menu icon size (min: 16, max: 48, default: 20)
  -w W              menu width in px (integer, default: screen width / 8)
  -d D              menu delay in milliseconds (default: 100)
  -o O              overlay opacity (min: 0.0, max: 1.0, default: 0.3)
  -t T              sway submenu lines limit (default: 30)
```

## Dependencies
- gtk3
- python (python3)
- python-gobject
- python-i3ipc (optionally, to avoid deprecation warning)

## How it works?

The problem to resolve was, that the Gtk.Menu class behaves differently / unexpectedly when open over Wayland and X11 windows. 
To avoid it, the script first creates a (semi-)transparent, floating window, that covers all the screen.

## Troubleshooting

In case __the menu does not position properly in the screen corner__, try increasing the delay length. The default value
equals 100 milliseconds, and on my laptop it works well down to 30. However slower machines may require higher values.
E.g. try using `-d 200` argument.

## TODO
- On next sway / GTK release, check if the overflowed menus issue on sway is fixed; remove 50 SLOC long workaround if so;
- testing;
- more testing.
