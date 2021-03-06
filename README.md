# GTKWave with remote control

This software is a fork of GTKWave with an additional remote control feature. It
allows GTKWave to display a running live trace and control it using tcl commands.

## Addition

The addition contains a new command line option `-p PORT` (or `--tcl_port=PORT`).
This opens a tcp server socket on the given port. The server accepts connections
and handles incoming lines as tcl commands and tries to interpret them.

The server does not send results or outputs back to the client. Errors are printed
on the standard error of gtkwave. If you want to do complex things you'll need to
write a script file and execute the command `source my_script.tcl`.

See the TCL commands section below for additional details on supported commands.

## Building

To build gtkwave with this option, several dependencies are required.

For ubuntu:
```
apt-get install build-essential tcl-dev tk-dev liblzma-dev gperf
```

To build, go in the directory where you cloned the repository, then do:
```
mkdir build
cd build
../configure --enable-tcl --disable-mime-update --prefix=/absolute/path/to/install/dir
make
make install
```

To be able to use this gtkwave without specifying the full path, you should add
the `bin` install directory to your path:
```
export PATH=/absolute/path/to/install/dir/bin:$PATH
```

## Testing

1. Start gtkwave on an existing trace file:
   
    ```
    gtkwave -p 1234 my_trace_file.vcd
    ```

    Or on a live trace:

    ```
    gtkwave -p 1234 -I SHMEMID
    ```

2. Open a connection and type tcl commands:
   
    ```
    nc locahost 1234
    ```

## TCL commands

This software supports any gtkwave tcl integrated commands, including the following:

### gtkwave::setWindowStartTime

This command takes a single argument which is a string representing a
timestamp. It must be formatted as a number followed by a time
unit. Supported time unit are `s`, `ms`, `us`, `ns`, `ps`, and smaller ones.
The number can be formatted as an integer or a floating point.
In the case it is an integer, the time unit can be omitted and default to the
trace's default.

Examples:
```
gtkwave::setWindowStartTime "32 us"
gtkwave::setWindowStartTime 47ms
gtkwave::setWindowStartTime 3.5ps
gtkwave::setWindowStartTime "12.3e2 us"
```

### gtkwave::/Time/Zoom/Zoom_To_Start

This set the current view to the begining of the trace.

### gtkwave::/Time/Zoom/Zoom_To_End

This set the current view to the end of the trace.

### gtkwave::/View/Partial_VCD_Dynamic_Zoom_To_End

This command toggles the dynamic signal display. When it is on, the view
follows the trace being generated. When it is off, the current displayed
is static and be changed (for example by `gtkwave::setWindowStartTime`).

### gtkwave::addSignalsFromList

This command takes a list of signals name as an argument.
Signals are added to view. Duplicates are removed from the list but a signal can be added several times (and will be duplicated in the view in that case).

Examples:
```
gtkwave::setWindowStartTime { sig1 }
gtkwave::setWindowStartTime { "sig1" }
gtkwave::setWindowStartTime { sig1 sig2 }
gtkwave::setWindowStartTime { "sig3[31:0]" }
gtkwave::setWindowStartTime { sig4\[31:0\] }
```

## GDB integration

GDB integration of the gtkwave control is in [gdb subdirectory](gdb).

## Licensing

This software is distributed under the terms of the GNU General Public License
version 2 (or higher). Please see LICENSE.TXT for more details about the
licensing and AUTHORS for the copyright holders.

## External resources

- [Original GTKWave website](http://gtkwave.sourceforge.net/)
- [GTKWave documentation](http://gtkwave.sourceforge.net/gtkwave.pdf)
