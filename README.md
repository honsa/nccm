
NCurses ssh Connection Manager (nccm)
=====================================

![](images/program_screenshot.png)

Copyright (C) 2020 Kenneth Aaron.

flyingrhino AT orcon DOT net DOT nz

Freedom makes a better world: released under GNU GPLv3.

https://www.gnu.org/licenses/gpl-3.0.en.html

This software can be used by anyone at no cost, however
if you like using my software and can support - please
donate money to a children's hospital of your choice.

This program is free software: you can redistribute it
and/or modify it under the terms of the GNU General Public
License as published by the Free Software Foundation:
GNU GPLv3. You must include this entire text with your
distribution.

This program is distributed in the hope that it will be
useful, but WITHOUT ANY WARRANTY; without even the implied
warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR
PURPOSE.
See the GNU General Public License for more details.


Manual install instructions
---------------------------

This is one of the ways to install nccm, you can of course
install and use nccm in any way you wish.

You may need to manually install the Python3 `PyYAML`
module. You'll know this when you run nccm and get a
python exception saying it can't find this module.
It can be installed as follows:
`pip3 install --user PyYAML`

You may even need to install the Python3 pip3 program
(which is needed for the command above) if your distro
doesn't have it. On debian based distros use:
`sudo apt-get install python3-pip`

After satisfying the dependencies (or following these next
steps first and running nccm to figure out whether you
have any unmet dependencies) you can do the following:
- copy the `nccm` script to: `/usr/local/bin/`
- `cd /usr/local/bin/`
- `chmod 755 nccm`
- `chown root:root nccm`
- copy the nccm.yml config file to one of the supported
  paths as described below

The script can then be run by simply typing `nccm`
from anywhere.

The ssh connections/config file nccm.yml is loaded from
one of the following paths, whichever is found first:
- `~/.config/nccm/nccm.yml`
- `~/.nccm.yml`
- `~/nccm.yml`
- `/etc/nccm.yml`

This program comes with an example yml file. Edit it
to suit your needs.


nccm.yml settings
-----------------

This file is mostly used for ssh connection details, but
also supports program settings as follows:

`nccm_config_controlmode`:
Controls the cursor movement mode. Two modes are supported:
- std:    Cursor keys move the marker as expected.
- focus:  Marker and list move independently.
          The list is designed to move while the marker
          remains fixed unless it is moved manually.

`nccm_config_loglevel`:
Controls log level of messages sent by nccm to syslog.
Use this for debugging. Default level is warning.
Supported levels: debug, info, warning, error, critical


Controls
--------

In nccm_config_controlmode == std mode:

- Up/Down arrows:     Move the marker the traditional way
- Home/End:           Jump marker to list first/last entry
- PgUp/PgDn:          Page up/down in the list

In nccm_config_controlmode == focus mode:

- Up/Down arrows:     Scroll the list up/down
- Home/End:           Jump to list first/last entry
- PgUp/PgDn:          Page up/down in the list
- Shift Up/Down:      Move the marker up/down
- Shift Left/Right:   Move the marker to display top/bottom

In both modes:

- Left/Right arrows:  Scroll the list horizontally
- Tab:                Switch between text boxes
- Enter or Ctrl-m:    Connect to the selected entry
- Ctrl-h:             Display this help menu
- Ctrl-k              Toggle cursor mode std <--> focus
- Ctrl-q or Ctrl-c:   Quit the program
- F1-F5 or !@#$% :    Sort by respective column (1-5)


Usage
-----

`Conn` textbox:
Accepts integer values only (and !@#$% for sorting).
Pressing Enter here will connect to this connection ID,
as corresponding to a valid value in the full
unfiltered list (even if that particular connection
is hidden by unmatching text in the `Filter` textbox),
ignoring everything else (`Filter` textbox, highlighted
line) - even if they don't match.
If this textbox is empty, it will connect to the
connection marked by the highlighted line.

`Filter` textbox:
Type any filter text here.
Accepts any printable character and space.
Text is forced to lowercase, and the resulting filtering
is case insensitive.
Pressing Enter will connect to the connection highlighed
in red. This also works if you're in the `Conn` textbox
and it's empty.

Textboxes accept backspace to delete one char backwards,
inline editing not supported.

Displayed connection list is filtered by the combined
contents of all the fields as you type in real time.
Spaces delimit filters if typed into `Filter` textbox
and all filter entries are AND'ed.

nccm_config_controlmode == focus was inspired by vim
where you can move your display up/down around your work
while keeping your current line selected.


Sorting
-------

F1-F5 keys sort by the respective fields 1-5.
The display shows 4 visible columms but we treat
username and server address as separate columns for
sorting purposes.
The Fn keys may be captured by certain GUIs so we have
an alternative - when focused on `Conn` window, press
Shift-1 through 5 (!@#$%) to toggle sorting by the
respective field number. If you type these special
characters in the `Filter` textbox they become standard
filters just like any printable character.
Pressing the same key again reverses the sort order.

```
Column #  Column name       Sort    Alternate sort
--------  -----------       ----    --------------
1         List serial #     F1      !
2         Friendly name     F2      @
3         User name         F3      #
4         Server address    F4      $
5         Description       F5      %
```


Limitations
-----------

Will not store passwords. Please don't request this
feature because it won't be added.
Either use ssh passwordless login (by placing your
public key on the server in `.ssh/authorized_keys` - tip:
look up `ssh-copy-id`) or store your password in a
password manager and paste it when prompted.

Does not like window resizing and exits gracefully
displaying an error message.
It's safe to resize the window once connection
establishment is in progress or after connected to
your server.

Does not support highlighting filter keywords in search
results because this results in a messy and confusing
display once more than a couple keywords are used.

Text entry is limited to the length of the textboxes
which in turn are dictated by the width of your window.
This should be enough for most use cases though.


Troubleshooting
---------------

Look at your syslog file for nccm entries. Depending upon
the verbosity level set in the config file you may not see
much if at all anything.
By default the production level of the script logs WARNING
and above which results in syslog silence until something
bad happens.
To increase logging verbosity change this line in the
nccm.yml config file to debug:
`nccm_config_loglevel: debug`
Also - more debugging calls exist but are commented out in
the code due to too much logging. Enable them as required.


Hacking nccm
------------

Take something good and make it better!
The code is heavily commented, with the hope that it will
make life easier for modders and forkers.

The config file is simple yaml. If you already have a
collection of logins elsewhere in an accessible format -
writing a script to convert and append fields to nccm.yml
is easy.


Misc
----

This program aims to do one thing well - lets you make SSH
connections from an ncurses based manager with minimum
distraction. Feature requests that keep nccm on focus will
be considered.

