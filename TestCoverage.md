# Introduction #

This document addresses to development team which wants to keep track of system specific tests that are written on every platform.

Such tests are written by using system utilities like **ps** (all UNIXes), **sysctl** (FreeBSD and OSX) and **WMI** (Windows) to gather _reliable_ results from the OS to compare against those returned by psutil.

All other generic tests defined in [test\_psutil.py](http://code.google.com/p/psutil/source/browse/trunk/test/test_psutil.py) script are not included in this list.

**Legend**

  * **X** == tested
  * **/** == not tested on purpose (e.g. functionality is not implemented for that specific platform)
  * **empty cell** == test missing


---


## psutil.Process class ##

Refers to psutil.**Process** attributes and methods.

| **Functionnality** | **Windows** | **Linux** | **OSX** | **FreeBSD** |
|:-------------------|:------------|:----------|:--------|:------------|
| ppid               | **X**       | **X**     | **X**   | **X**       |
| name               | **X**       | **X**     | **X**   | **X**       |
| path               | **X**       | **X**     | **X**   | **X**       |
| cmdline            | **X**       | **X**     | **X**   | **X**       |
| create\_time       | **X**       | **X**     | **X**   | **X**       |
| uid                | **/**       | **X**     | **X**   |             |
| gid                | **/**       | **X**     | **X**   | **X**       |
| username           | **X**       | **X**     | **X**   | **X**       |
| groupname          | **/**       | **X**     | **X**   | **X**       |
| getcwd()           |             |           | **/**   | **/**       |
| get\_cpu\_times()  |             |           |         |             |
| get\_cpu\_percent()|             |           |         |             |
| get\_memory\_info()| **X**       | **X**     | **X**   | **X**       |
| get\_cpu\_percent()|             |           |         |             |
| suspend()          |             |           |         |             |
| resume()           |             |           |         |             |

## psutil module ##

Refers to functions and constants defined in the psutil namespace (e.g. psutil.cpu\_percent())

| **Functionnality** | **Windows** | **Linux** | **OSX** | **FreeBSD** |
|:-------------------|:------------|:----------|:--------|:------------|
| `_`UPTIME          | **X**       |           |         |             |
| TOTAL\_PHYMEM      | **X**       |           | **X**   | **X**       |
| NUM\_CPUS          | **X**       |           |         |             |
| get\_pid\_list()   | **X**       | **X**     | **X**   | **X**       |
| cpu\_percent()     |             |           |         |             |
| avail\_phymem()    |             |           |         | **X**       |
| total\_virtmem()   |             |           |         | **X**       |
| avail\_virtmem()   |             |           |         | **X**       |