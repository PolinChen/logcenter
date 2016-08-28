# setup the env under SunOS, use Korn shell

> need to setup the shell env. in SunOS


- setup the delete key for backspace
- setup the TEM=v100 for vi edit
- set LC_ALL, LANG for ksh and perl

```
stty erase ^?
export TERM=vt100
export LC_ALL=C
export LANG=C
```
