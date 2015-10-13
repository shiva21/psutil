

# Future milestones #

_Describes features scheduled for future releases_

## Platforms ##

  * **Solaris** support.
  * **Jython** support.

## System ##

  * multiple CPU times and percent usage
  * real time disk IO read writes
  * real time network IO read writes

## Process Object ##

  * IO statistics
  * **on\_process\_terminate(**_callback_, `[*args`, `[**kwargs]]`**)** - call _callback()_ every time a process is terminated.

#### OS specific - UNIX ####

  * **terminal** - terminal used by the process

#### OS specific - Windows ####

  * **comment** - process comment
  * **services** - Windows services registered by this process
  * **bring\_to\_front()** - if process has a GUI associated calls this window to the front, on top of anything else you are running
  * **iconpath** - returns the absolute path of the file used as executable's icon
  * **send\_signal** - make send\_signal() method be able to accept [CTRL\_C\_EVENT](http://docs.python.org/library/signal.html#signal.CTRL_C_EVENT) and [CTRL\_BREAK\_EVENT](http://docs.python.org/library/signal.html#signal.CTRL_BREAK_EVENT) signals. Python 2.7 patch: http://bugs.python.org/issue1220212
  * **title** - the title of the window (GUI): http://msdn.microsoft.com/en-us/library/ms686331(v=VS.85).aspx
  * **handle\_counts()** - ([Windows API](http://msdn2.microsoft.com/en-us/library/ms683214(VS.85).aspx))

## Hooks ##

  * **on\_process\_create(**_pid, callback_, `[*args`, `[**kwargs]]`**)** - call _callback()_ every time a new process is created ([Windows API](http://www.codeproject.com/KB/system/soviet_protector.aspx)).

# Implemented and upcoming milestones #

_Describes features already implemented and those ones scheduled for the next release_


## 0.3.0 Milestone ##

**Process object**
  * terminal

**System**
  * disk usage
  * disk partitions

## 0.2.1 Milestone ##

**Process object**

  * status
  * I/O counters
  * real, effective, saved user ids
  * real, effective, saved groups ids
  * get/set niceness (priority)
  * get/set I/O niceness (priority) - Linux only

**System**

  * boot time

**Other**

  * Popen class extending subprocess stdlib module.

## 0.2.0 Milestone (current) ##

**Process object**

  * open files
  * open connections (TCP, UDP)
  * children
  * executable name
  * send signal
  * terminate
  * number of threads
  * (Linux) system physical cached memory and system physical memory buffers used by the kernel

**Platforms support**

  * Windows 2000
  * Windows 64-bit versions (all)
  * mingw32 compiler

## 0.1.3 Milestone ##

  * process username/groupname
  * process current directory
  * suspend/resume process

## 0.1.2 Milestone ##

**Process Object**

  * CPU user/kernel times
  * create time
  * CPU utilization percentage
  * memory usage (bytes)
  * memory utilization (percent)

**System related objects**

  * system uptime
  * total system virtual memory
  * total system physical memory
  * total system used/free virtual and physical memory

## 0.1.1 Milestone ##

**Process Object**

  * ppid, parent properties

**Module Level Functions**

  * process\_iter()

**Requirements**

  * FreeBSD support


## 0.1.0 Milestone ##

**Requirements**
  * Operating systems: Windows 2000/XP/Vista, Linux, Mac OS X

**Module Level Functions**
  * get\_process\_list() - return a list of Process objects for running processes

**Process Object**
  * Kill a specified process
  * Return the processes' basic information (pid, name, executable, path, command line)

# Rejected Milestones #

  * **set\_name(**_name_**)** - change the displayed executable name the process runs under ([issue 47](https://code.google.com/p/psutil/issues/detail?id=47))