# Controlling GTKWave with gdb commands

This python module adds gtkwave control support using gdb commands.

## GDB setup

### Loading the module

To load this module into gdb and add the support, just do
`source gtkwave-gdb.py`.

This can be done in the command-line, in the console or in a .gdbinit file.

### Defining the socket

GTKWave is controlled using a socket. It needs to be configured in gdb so that
the new commands work.

The module adds a new gdb parameter named _gtkwave-socket_. It can be displayed
and changed using the `show gtkwave-socket` and `set gtkwave-socket host:port`
commands.

This parameter is initialized with the content of `GTKWAVE_CONTROL_SOCKET`
environment variable.

## GDB commands

This module adds 2 commands.

### gtkwave-set-time

It sets the current time displayed in gtkwave view. It takes one argument
specifying the timestamp. The timestamp can be the keywords _start_ or _end_
or a number with a time unit.

Examples:

```
gtkwave-set-time start
gtkwave-set-time end
gtkwave-set-time 32s
gtkwave-set-time 42ms
gtkwave-set-time 18
```

### gtkwave-toggle-dyn-time

It toggles the dynamic displaying in gtkwave view. It take no argument.
When it is enabled the view show the last generated trace while generation.

Example:
```
gtkwave-toggle-dyn-time
```
