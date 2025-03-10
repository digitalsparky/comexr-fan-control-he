Clevo Fan Control Indicator for Ubuntu
======================================

This program is an Ubuntu driver to control the fan of Clevo laptops, using reversed-engineering port information from ECView.

For command-line, use *-h* to display help, or a number representing percentage of fan duty to control the fan (from 20% to 100%).


Build and Install
-----------------

```shell
sudo apt-get install build-essential
git clone https://github.com/comexr/comexr-fan-control-he.git
cd comexr-fan-control-he
make install
```


Notes
-----

The executable has setuid flag on, but must be run by the current desktop user,
because only the desktop user is allowed to display a desktop indicator in
Ubuntu, while a non-root user is not allowed to control Clevo EC by low-level
IO ports. The setuid=root creates a special situation in which this program can
fork itself and run under two users (one for desktop/indicator and the other
for EC control), so you could see two processes in ps, and killing either one
of them would immediately terminate the other.

Be careful not to use any other program accessing the EC by low-level IO
syscalls (inb/outb) at the same time - I don't know what might happen, since
every EC actions require multiple commands to be issued in correct sequence and
there is no kernel-level protection to ensure each action must be completed
before other actions can be performed... The program also attempts to prevent
abortion while issuing commands by catching all termination signals except
SIGKILL - don't kill the indicator by "kill -9" unless absolutely necessary.

