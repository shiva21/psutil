<font color='red'>Warning: this documentation refers to deprecated 1.2.1 version. New doc is <a href='http://pythonhosted.org/psutil/'>here</a>. </font>



# API Reference #

## Exceptions ##

psutil.**NoSuchProcess**<font size='3'><b><code>(</code></b></font>_pid, name=None, msg=None_<font size='3'><b><code>)</code></b></font>
> Raised when no process with the given PID is found in the current process list or when a process no longer exists (zombie).

psutil.**AccessDenied**<font size='3'><b><code>(</code></b></font>_pid=None, name=None, msg=None_<font size='3'><b><code>)</code></b></font>
> Raised when permission to perform an action is denied.

psutil.**TimeoutExpired**<font size='3'><b><code>(</code></b></font>_pid=None, name=None_<font size='3'><b><code>)</code></b></font>
> Raised on `Process.wait(timeout)` if timeout expires and process is still alive.

> _**New in 0.2.1**_


---


## Classes ##

psutil.**Process**<font size='3'><b><code>(</code></b></font>_pid_<font size='3'><b><code>)</code></b></font>

> A class which represents an OS process.  Raises _NoSuchProcess_ if _pid_ does not exist.

> Note that most of the methods of this class do not make sure the PID of the process being queried has been reused. That means you might end up retrieving an information referring to another process in case the original one this instance refers to is gone in the meantime.  The only exceptions for which process identity is pre-emptively checked are: _parent, get\_children(), set\_nice(), suspend(), resume(), send\_signal(), terminate()_ and _kill()_.

> To prevent this problem for all other methods you can use _is\_running()_ before querying the process or in case you're continuously iterating over a set of Process instances use _process\_iter()_ which pre-emptively checks process identity for every yielded instance

  * **pid**<br>The process pid.</li></ul>

<ul><li><b>ppid</b><br>The process parent pid.</li></ul>

> _**Changed in 0.5.0:** return value is cached._

  * **parent**<br>Return the parent process as a <code>Process</code> object pre-emptively checking whether PID has been reused.. If no ppid is known then return <code>None</code>.</li></ul>

<ul><li><b>name</b><br>The process name.</li></ul>

> _**Changed in 0.5.0:** return value is cached._

  * **exe**<br>The process executable as an absolute path name.</li></ul>

<blockquote>_**Changed in 0.5.0:** return value is cached._

  * **cmdline**<br>The command line process has been called with.</blockquote>

> _**Changed in 0.5.0:** return value is cached._

  * **create\_time**<br> The process creation time as a floating point number expressed in seconds since the epoch, in <a href='http://en.wikipedia.org/wiki/Coordinated_Universal_Time'>UTC</a>.<br>
<pre><code>&gt;&gt;&gt; import os, psutil, datetime<br>
&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; p.create_time<br>
1307289803.47<br>
&gt;&gt;&gt; datetime.datetime.fromtimestamp(p.create_time).strftime("%Y-%m-%d %H:%M:%S")<br>
'2011-03-05 18:03:52'<br>
</code></pre></li></ul>

<blockquote>_**Changed in 0.5.0:** return value is cached._

  * **uids**<br>The <i>real</i>, <i>effective</i> and <i>saved</i> user ids of the current process as a nameduple. This is the same as <a href='http://docs.python.org/library/os.html#os.getresuid'>os.getresuid()</a> but per-process.</blockquote>

> _**New in 0.2.1**_<br><i><b>Availability:</b> UNIX</i></li></ul>

<ul><li><b>gids</b><br>The <i>real</i>, <i>effective</i> and <i>saved</i> group ids of the current process as a nameduple. This is the same as <a href='http://docs.python.org/library/os.html#os.getresgid'>os.getresgid()</a> but per-process.</li></ul>

> _**New in 0.2.1**_<br><i><b>Availability:</b> UNIX</i></li></ul>

<ul><li><b>username</b><br>The name of the user that owns the process. On UNIX this is calculated by using <i>real</i> process uid.</li></ul>

> _**Changed in 2.0:** Windows implementation has been rewritten in C for performace and [pywin32](http://sourceforge.net/projects/pywin32/) extension is no longer required._

  * **terminal**<br>The terminal associated with this process, if any, else None. This is similar to "tty" command but per-process.</li></ul>

<blockquote>_**New in 0.3.0**_<br><i><b>Availability:</b> UNIX</i></blockquote>

  * **status**<br>The current process status. The return value is one of the <code>STATUS_*</code> constants (a lower cased string).  Example:<br>
<pre><code>&gt;&gt;&gt; import psutil, os<br>
&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; p.status<br>
'running'<br>
&gt;&gt;&gt;<br>
</code></pre></li></ul>

<blockquote>_**New in 0.2.1**_<br><i><b>Changed in 0.5.0</b>: the string representation can be used for comparison</i></blockquote>

  * **nice**<br>A property getter/setter to get or set process <a href='http://blogs.techrepublic.com.com/opensource/?p=140'>niceness</a> (priority). Deprecated in 0.5.0; use <code>get_nice()</code> and <code>set_nice()</code> instead.</li></ul>

<blockquote>_**New in 0.2.1**_<br><b><font color='red'>Deprecated in 0.5.0</b></font>_</blockquote>_

  * **get\_nice**<font size='3'><b><code>(</code></b></font><font size='3'><b><code>)</code></b></font><br>
<ul><li><b>set_nice</b><font size='3'><b><code>(</code></b></font><i>value</i><font size='3'><b><code>)</code></b></font><br><br>Get or set process <a href='http://blogs.techrepublic.com.com/opensource/?p=140'>niceness</a> (priority). On UNIX this is a number which usually goes from -20 to 20. The higher the nice value, the lower the priority of the process.<br>
<pre><code>&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; p.get_nice()<br>
0<br>
&gt;&gt;&gt; p.set_nice(10)<br>
&gt;&gt;&gt;<br>
</code></pre></li></ul>

<blockquote>On Windows this is available as well by using <a href='http://msdn.microsoft.com/en-us/library/ms683211(v=vs.85).aspx'>GetPriorityClass</a> and <a href='http://msdn.microsoft.com/en-us/library/ms686219(v=vs.85).aspx'>SetPriorityClass</a>. <code>psutil.*_PRIORITY_CLASS</code> constants (explained <a href='http://msdn.microsoft.com/en-us/library/ms686219(v=vs.85).aspx'>here</a>) can be used in conjunction. Example which increases process priority:</blockquote>

<blockquote><pre><code>&gt;&gt; p.set_nice(psutil.HIGH_PRIORITY_CLASS)</code></pre></blockquote>

<blockquote><i><b>New in 0.5.0</b></i></blockquote>

<ul><li><b>as_dict</b><font size='3'><b><code>(</code></b></font><i>attrs=<code>[]</code>, ad_value=None</i><font size='3'><b><code>)</code></b></font><br>Utility method returning process information as a hashable dictionary. If <code>attrs</code> is specified it must be a list of strings reflecting available Process class's attribute names (e.g. <code>['get_cpu_times', 'name']</code>) else all public (read only) attributes are assumed. <code>ad_value</code> is the value which gets assigned to a dict key in case <code>AccessDenied</code> exception is raised when retrieving that particular process information.</li></ul>

<blockquote><i><b>New in 0.5.0</b></i></blockquote>

<ul><li><b>getcwd</b><font size='3'><b><code>()</code></b></font><br>Return a string representing the process current working directory.</li></ul>

<blockquote><i><b>Changed in 0.4.0:</b> added FreeBSD support</i><br>
<i><b>Changed in 0.6.0:</b> added OSX support</i></blockquote>

<ul><li><b>get_io_counters</b><font size='3'><b><code>()</code></b></font><br>Return process I/O statistics as a namedtuple including the number of read and write operations performed by the process and the amount of bytes read and written. For linux refer to <a href='http://www.mjmwired.net/kernel/Documentation/filesystems/proc.txt#1304'>/proc filesysem documentation</a>. On BSD there's apparently no way to retrieve bytes counters, hence <code>-1</code> is returned for <code>read_bytes</code> and <code>write_bytes</code> fields. OSX is not supported.<br>
<pre><code>&gt;&gt;&gt; p.get_io_counters()<br>
io(read_count=454556, write_count=3456, read_bytes=110592, write_bytes=0)<br>
</code></pre>
</li></ul><blockquote><i><b>New in 0.2.1</b></i><br><i><b>Availability:</b> Linux, Windows, FreeBSD</i></blockquote>

<ul><li><b>get_ionice</b><font size='3'><b><code>()</code></b></font><br>Return <a href='http://friedcpu.wordpress.com/2007/07/17/why-arent-you-using-ionice-yet/'>process I/O niceness</a> (priority) as a namedtuple including priority class and priority data. See <code>set_ionice</code> below for more information.<br>
</li></ul><blockquote><i><b>New in 0.2.1</b></i><br><i><b>Availability:</b> Linux, Windows</i><br><b>Changed in 0.7.0</b>: added Windows support.<i></blockquote></i>

<ul><li><b>set_ionice</b><font size='3'><b><code>(</code></b></font><i>ioclass, value=None</i><font size='3'><b><code>)</code></b></font><br>Change <a href='http://friedcpu.wordpress.com/2007/07/17/why-arent-you-using-ionice-yet/'>process I/O niceness</a> (priority). <i>ioclass</i> is one of the <b><code>IOPRIO_CLASS_*</code></b> constants. <i>value</i> is a number which goes from 0 to 7. The higher <i>value</i> value, the lower the I/O priority of the process. For further information refer to <a href='http://linux.die.net/man/1/ionice'>ionice</a> command line utility or <a href='http://linux.die.net/man/2/ioprio_set'>ioprio_set</a> system call. The example below sets IDLE priority class for the current process, meaning it will only get I/O time when no other process needs the disk:<br>
<pre><code>&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; p.set_ionice(psutil.IOPRIO_CLASS_IDLE)<br>
&gt;&gt;&gt;<br>
</code></pre></li></ul>

<blockquote><i><b>New in 0.2.1</b></i><br><i><b>Availability:</b> Linux, Windows</i></blockquote>

<ul><li><b>get_num_fds</b><font size='3'><b><code>()</code></b></font><br>Return the number of file descriptors used by this process.</li></ul>

<blockquote><i><b>New in 0.5.0</b></i><br><i><b>Availability:</b> UNIX</i></blockquote>

<ul><li><b>get_num_handles</b><font size='3'><b><code>()</code></b></font><br>Return the number of handles used by this process.</li></ul>

<blockquote><i><b>New in 0.5.0</b></i><br><i><b>Availability:</b> Windows</i></blockquote>

<ul><li><b>get_num_threads</b><font size='3'><b><code>()</code></b></font><br>Return the number of threads used by this process.</li></ul>

<ul><li><b>get_threads</b><font size='3'><b><code>()</code></b></font><br>Return threads opened by process as a list of namedtuples including thread id and thread CPU times (user/system).</li></ul>

<blockquote><i><b>New in 0.2.1</b></i></blockquote>

<ul><li><b>get_num_ctx_switches</b><font size='3'><b><code>()</code></b></font><br>Return the number voluntary and involuntary context switches performed by this process.</li></ul>

<blockquote><i><b>New in 0.6.0</b></i></blockquote>

<ul><li><b>get_cpu_times</b><font size='3'><b><code>()</code></b></font><br>Return a tuple whose values are process CPU user and system times which means the amount of time expressed in seconds that a process has spent in <a href='http://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1'>user/system mode</a>.</li></ul>

<ul><li><b>get_cpu_percent</b><font size='3'><b><code>(</code></b></font><i>interval=0.1</i><font size='3'><b><code>)</code></b></font><br>Return a float representing the process CPU utilization as a percentage. When interval is > 0.0 compares process times to system CPU times elapsed before and after the interval (blocking). When interval is 0.0 or None compares process times to system CPU times elapsed since last call, returning immediately. In this case is recommended for accuracy that this function be called with at least 0.1 seconds between calls. Example:<br>
<pre><code>&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; # blocking<br>
&gt;&gt;&gt; p.get_cpu_percent(interval=1)<br>
2.0<br>
&gt;&gt;&gt; # non-blocking (percentage since last call)<br>
&gt;&gt;&gt; p.get_cpu_percent(interval=0)<br>
2.9<br>
&gt;&gt;&gt;<br>
</code></pre></li></ul>

<blockquote><i><b>Changed in 0.2.0:</b> interval parameter was added</i></blockquote>

<ul><li><b>get_cpu_affinity</b><font size='3'><b><code>(</code></b></font><font size='3'><b><code>)</code></b></font>
</li><li><b>set_cpu_affinity</b><font size='3'><b><code>(</code></b></font><i>cpus</i><font size='3'><b><code>)</code></b></font><br>Get and set process current <a href='http://www.linuxjournal.com/article/6799?page=0,0'>CPU affinity</a>.  CPU affinity consists in telling the OS to run a certain process on a limited set of CPUs only. The number of eligible CPUs can be obtained with <code>list(range(psutil.NUM_CPUS))</code>.<br>
<pre><code>&gt;&gt;&gt; psutil.NUM_CPUS<br>
4<br>
&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; p.get_cpu_affinity()<br>
[0, 1, 2, 3]<br>
&gt;&gt;&gt; p.set_cpu_affinity([0])  # from now on, process will run on CPU #0 only<br>
&gt;&gt;&gt;<br>
</code></pre></li></ul>

<blockquote><i><b>New in 0.5.0</b></i><br><i><b>Availability:</b> Linux, Windows</i></blockquote>

<ul><li><b>get_rlimit</b><font size='3'><b><code>(</code></b></font><i>resource</i><font size='3'><b><code>)</code></b></font>
</li><li><b>set_rlimit</b><font size='3'><b><code>(</code></b></font><i>resource, limits</i><font size='3'><b><code>)</code></b></font><br>Get and set process resource limits (see <a href='http://linux.die.net/man/2/prlimit'>man prlimit</a>. <code>resource</code> is one of the <code>psutil.RLIMIT_*</code> constants. <code>limits</code> is a (soft, hard) tuple. Examples:<br>
<pre><code>&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; # process may open no more than 128 file descriptors<br>
&gt;&gt;&gt; p.set_rlimit(psutil.RLIMIT_NOFILE, (128, 128))<br>
&gt;&gt;&gt;<br>
&gt;&gt;&gt; # process may create files no bigger than 1024 bytes<br>
&gt;&gt;&gt; p.set_rlimit(psutil.RLIMIT_FSIZE, (1024, 1024))<br>
&gt;&gt;&gt; <br>
</code></pre></li></ul>

<blockquote><i><b>New in 1.1.0</b></i><br><i><b>Availability:</b> Linux</i></blockquote>

<ul><li><b>get_memory_info</b><font size='3'><b><code>()</code></b></font><br>Return a tuple representing <a href='http://en.wikipedia.org/wiki/Resident_Set_Size'>RSS</a> (Resident Set Size) and VMS (Virtual Memory Size) in bytes.<br>On UNIX RSS and VMS are the same values shown by ps. On Windows RSS and VMS refer to "Mem Usage" and "VM Size" columns of taskmgr.exe.</li></ul>

<ul><li><b>get_ext_memory_info</b><font size='3'><b><code>()</code></b></font><br>Return a namedtuple with variable fields depending on the platform representing extended memory information about the process. All numbers are expressed in bytes.</li></ul>

<blockquote><i><b>New in 0.6.0</b></i></blockquote>

<ul><li><b>get_memory_percent</b><font size='3'><b><code>()</code></b></font><br>Compare physical system memory to process resident memory (RSS) and calculate process memory utilization as a percentage.</li></ul>

<ul><li><b>get_memory_maps</b><font size='3'><b><code>(</code></b></font><i>grouped=True</i><font size='3'><b><code>)</code></b></font><br>Return process's mapped memory regions as a list of nameduples whose fields are variable depending on the platform. As such, portable applications should rely on namedtuple's <code>path</code> and <code>rss</code> fields only. This method is useful to obtain a detailed representation of process memory usage as explained <a href='http://bmaurer.blogspot.it/2006/03/memory-usage-with-smaps.html'>here</a>.  If <code>grouped</code> is True the mapped regions with the same <code>path</code> are grouped together and the different memory fields are summed.  If <code>grouped</code> is False every mapped region is shown as a single entity and the namedtuple will also include the mapped region's address space (<code>addr</code>) and permission set (<code>perms</code>).<br>
<pre><code>&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; p.get_memory_maps()<br>
[mmap(path='/lib/x86_64-linux-gnu/libutil-2.15.so', rss=16384, anonymous=8192, swap=0),<br>
 mmap(path='/lib/x86_64-linux-gnu/libc-2.15.so', rss=6384, anonymous=15, swap=0),<br>
 mmap(path='/lib/x86_64-linux-gnu/libcrypto.so.0.1', rss=34124, anonymous=1245, swap=0),<br>
 mmap(path='[heap]', rss=54653, anonymous=8192, swap=0),<br>
 mmap(path='[stack]', rss=1542, anonymous=166, swap=0),<br>
 ...]<br>
&gt;&gt;&gt; <br>
</code></pre>
</li></ul><blockquote><i><b>New in 0.5.0</b></i></blockquote>

<ul><li><b>get_children</b><font size='3'><b><code>(</code></b></font><i>recursive=False</i><font size='3'><code>)</code><b></font></b><br>Return the children of this process as a list of <code>Process</code> objects pre-emptively checking whether PID has been reused. If recursive is <code>True</code> return all the parent descendants.  Example (A == this process):<br>
<pre><code>A ─┐<br>
   │<br>
   ├─ B (child) ─┐<br>
   │             └─ X (grandchild) ─┐<br>
   │                                └─ Y (great grandchild)<br>
   ├─ C (child)<br>
   └─ D (child)<br>
<br>
&gt;&gt;&gt; p.get_children()<br>
B, C, D<br>
&gt;&gt;&gt; p.get_children(recursive=True)<br>
B, X, Y, C, D<br>
</code></pre></li></ul>

<blockquote>Note that in the example above if process X disappears process Y won't be returned either as the reference to process A is lost.</blockquote>

<blockquote><i><b>Changed in 0.5.0:</b> recursive parameter was added.</i><br></blockquote>

<ul><li><b>get_open_files</b><font size='3'><b><code>()</code></b></font><br>Return regular files opened by process as a list of <a href='http://docs.python.org/library/collections.html?highlight=namedtuple#collections.namedtuple'>namedtuples</a> including file absolute path name and file descriptor. Example:<br>
<pre><code>&gt;&gt;&gt; f = open('file.ext', 'w')<br>
&gt;&gt;&gt; p = psutil.Process(os.getpid())<br>
&gt;&gt;&gt; p.get_open_files()<br>
[openfile(path='/home/giampaolo/svn/psutil/file.ext', fd=3)]<br>
</code></pre></li></ul>

<blockquote><i><b>Changed in 0.2.1:</b> OSX implementation rewritten in C; no longer requiring lsof.</i><br>
<i><b>Changed in 0.4.1:</b> FreeBSD implementation rewritten in C; no longer requiring lsof.</i></blockquote>

<ul><li><b>get_connections</b><font size='3'><b><code>(</code></b></font><i>kind='inet'</i><font size='3'><b><code>)</code></b></font><br>Return all TCP and UDP connections opened by process as a list of <a href='http://docs.python.org/library/collections.html?highlight=namedtuple#collections.namedtuple'>namedtuples</a>. Every namedtuple provides 6 attributes:<br />
<ul><li><b>fd</b>: the socket file descriptor. This can be passed to <a href='http://docs.python.org/library/socket.html#socket.fromfd'>socket.fromfd</a> to obtain a usable socket object. This is only available on UNIX; on Windows <code>-1</code> is always returned.<br>
</li><li><b>family</b>: the address family, either <a href='http://docs.python.org/library/socket.html#socket.AF_INET'>AF_INET</a> or <a href='http://docs.python.org/library/socket.html#socket.AF_INET6'>AF_INET6</a> <br>
</li><li><b>type</b>: the address type, either <a href='http://docs.python.org/library/socket.html#socket.SOCK_STREAM'>SOCK_STREAM</a> or <a href='http://docs.python.org/library/socket.html#socket.SOCK_DGRAM'>SOCK_DGRAM</a> <br>
</li><li><b>laddr</b>: the local address as a <code>(ip, port)</code> tuple. <br>
</li><li><b>raddr</b>: the remote address as a <code>(ip, port)</code> tuple. When the remote endpoint is not connected the tuple is empty. <br>
</li><li><b>status</b>: represents the status of a TCP connection. The return value is one of the <code>psutil.CONN_*</code> constants. For UDP and UNIX sockets this is always going to be <code>psutil.CONN_NONE</code>.<br></li></ul></li></ul>

<blockquote>The kind parameter is a string which filters for connections that fit the following criteria:</blockquote>

<table><thead><th> <b>Kind value</b> </th><th>      <b>Connections using</b> </th></thead><tbody>
<tr><td> inet              </td><td>      IPv4 and IPv6            </td></tr>
<tr><td> inet4             </td><td>      IPv4                     </td></tr>
<tr><td> inet6             </td><td>      IPv6                     </td></tr>
<tr><td> tcp               </td><td>      TCP                      </td></tr>
<tr><td> tcp4              </td><td>      TCP over IPv4            </td></tr>
<tr><td> tcp6              </td><td>      TCP over IPv6            </td></tr>
<tr><td> udp               </td><td>      UDP                      </td></tr>
<tr><td> udp4              </td><td>      UDP over IPv4            </td></tr>
<tr><td> udp6              </td><td>      UDP over IPv6            </td></tr>
<tr><td> unix              </td><td>      UNIX socket (both UDP and TCP protocols) </td></tr>
<tr><td> all               </td><td>      the sum of all the possible families and protocols </td></tr></tbody></table>

<blockquote>Example:<br>
<pre><code>&gt;&gt;&gt; p = psutil.Process(1694)<br>
&gt;&gt;&gt; p.name<br>
'firefox'<br>
&gt;&gt;&gt; p.get_connections()<br>
[connection(fd=115, family=2, type=1, laddr=('10.0.0.1', 48776), raddr=('93.186.135.91', 80), status='ESTABLISHED'),<br>
 connection(fd=117, family=2, type=1, laddr=('10.0.0.1', 43761), raddr=('72.14.234.100', 80), status='CLOSING'),<br>
 connection(fd=119, family=2, type=1, laddr=('10.0.0.1', 60759), raddr=('72.14.234.104', 80), status='ESTABLISHED'),<br>
 connection(fd=123, family=2, type=1, laddr=('10.0.0.1', 51314), raddr=('72.14.234.83', 443), status='SYN_SENT')]<br>
</code></pre>
On <b>FreeBSD</b> this is implemented by parsing <a href='http://en.wikipedia.org/wiki/Lsof'>lsof</a> command output. If lsof is not installed on the system <a href='http://docs.python.org/library/exceptions.html?highlight=notimplementederror#exceptions.NotImplementedError'>NotImplementedError</a> exception is raised and for third party processes (different than <code>os.getpid()</code>) results can differ depending on user privileges.<br></blockquote>

<blockquote><i><b>Changed in 0.2.1:</b> OSX implementation rewritten in C; no longer requiring lsof.</i><br>
<i><b>Changed in 0.4.0:</b> added 'kind' parameter.</i><br>
<i><b>Changed in 0.6.0:</b> added 'unix' socket support.</i><br>
<i><b>Changed in 0.6.0:</b> BSD implementation rewritten in C; no longer requiring lsof.</i><br>
<i><b>Changed in 1.0.0:</b> local_address and remote_address fields are deprecated.</i><br>
<i><b>Changed in 1.0.0:</b> 'status' field is no longer a string but a constant object (<code>psutil.CONN_*</code>).</i></blockquote>

<ul><li><b>is_running</b><font size='3'><b><code>()</code></b></font><br>Return whether the current process is running in the current process list. This is reliable also in case the process is gone and its PID reused by another process, therefore it must be preferred over doing <code>psutil.pid_exists(p.pid)</code>.</li></ul>

<ul><li><b>send_signal</b><font size='3'><b><code>(</code></b></font><i>signal</i><font size='3'><b><code>)</code></b></font><br>Send a signal to process (see <a href='http://docs.python.org/library/signal.html'>signal module</a> constants) pre-emptively checking whether PID has been reused. On Windows only <code>SIGTERM</code> is valid and is treated as an alias for <code>kill()</code>.</li></ul>

<ul><li><b>suspend</b><font size='3'><b><code>()</code></b></font><br>Suspend process execution with <code>SIGSTOP</code> signal pre-emptively checking whether PID has been reused. On Windows this is done by suspending all process threads execution.</li></ul>

<ul><li><b>resume</b><font size='3'><b><code>()</code></b></font><br>Resume process execution with <code>SIGCONT</code> signal pre-emptively checking whether PID has been reused. On Windows this is done by resuming all process threads execution.</li></ul>

<ul><li><b>terminate</b><font size='3'><b><code>()</code></b></font><br>Terminate the process with <code>SIGTERM</code> signal pre-emptively checking whether PID has been reused. On Windows this is an alias for <code>kill()</code>.</li></ul>

<ul><li><b>kill</b><font size='3'><b><code>()</code></b></font><br>Kill the current process by using <code>SIGKILL</code> signal pre-emptively checking whether PID has been reused.</li></ul>

<blockquote><i><b>Changed in 0.2.0:</b> no longer accepts <code>sig</code> keyword argument - use <code>send_signal()</code> instead.</i></blockquote>

<ul><li><b>wait</b><font size='3'><b><code>(</code></b></font><i>timeout=None</i><font size='3'><b><code>)</code></b></font><br>Wait for process termination and if the process is a children of the current one also return the exit code, else <code>None</code>. On Windows there's no such limitation (exit code is always returned). If the process is already terminated does not raise <code>NoSuchProcess</code> exception but just return <code>None</code> immediately. If <i>timeout</i> is specified and process is still alive raises <code>TimeoutExpired</code> exception.</li></ul>

<blockquote><i><b>New in 0.2.1</b></i></br>
<i><b>Changed in 0.4.0</b>: timeout=0 can now be used to make wait() return immediately (non blocking)</i></blockquote>

<br />
psutil.<b>Popen</b><font size='3'><b><code>(</code></b></font><code>*args, **kwargs</code><font size='3'><b><code>)</code></b></font>

<blockquote>A more convenient interface to stdlib <a href='http://docs.python.org/library/subprocess.html#subprocess.Popen'>subprocess.Popen</a>. It starts a sub process and deals with it exactly as when using subprocess.Popen class but in addition also provides all the properties and methods of psutil.Process class in a single interface. For method names common to both classes such as <i>kill()</i> and <i>terminate()</i>, psutil.Process implementation takes precedence. For a complete documentation refers to <a href='http://docs.python.org/library/subprocess.html'>subprocess module documentation</a>.<br>
<pre><code>&gt;&gt;&gt; import psutil<br>
&gt;&gt;&gt; from subprocess import PIPE<br>
&gt;&gt;&gt; p = psutil.Popen(["/usr/bin/python", "-c", "print 'hi'"], stdout=PIPE)<br>
&gt;&gt;&gt; p.name<br>
'python'<br>
&gt;&gt;&gt; p.uids<br>
user(real=1000, effective=1000, saved=1000)<br>
&gt;&gt;&gt; p.username<br>
'giampaolo'<br>
&gt;&gt;&gt; p.communicate()<br>
('hi\n', None)<br>
&gt;&gt;&gt; p.wait(timeout=2)<br>
0<br>
&gt;&gt;&gt;<br>
</code></pre></blockquote>

<blockquote><i><b>New in 0.2.1</b></i></blockquote>

<hr />

<h2>System related functions</h2>

<h3>Processes</h3>

psutil.<b>get_pid_list</b><font size='3'><b><code>()</code></b></font><br>
<blockquote>Return a list of current running PIDs.</blockquote>

psutil.<b>pid_exists</b><font size='3'><b><code>(</code></b></font><i>pid</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Check whether the given PID exists in the current process list. This is faster than doing <code>pid in psutil.get_pid_list()</code> and should be preferred.</blockquote>

psutil.<b>process_iter</b><font size='3'><b><code>()</code></b></font><br>
<blockquote>Return an iterator yielding a Process class instances for all running processes on the local machine.  Every new Process instance is only created once and then cached into an internal table which is updated every time this is used. As such it must be preferred over doing <code>for pid in psutil.get_pid_list(): psutil.Process(pid)</code> as it's optimized and avoids to maintain a separate process table in your code. Cached Process instances are checked for identity so that you're safe in case a PID has been reused by another process, in which case the cached instance is updated. Sorting order in which processes are returned is based on their PID.</blockquote>


<blockquote><i><b>Changed in 0.5.0:</b> added internal cache</i><br>
<i><b>Changed in 0.6.0:</b> yields processes sorted by their PIDs</i></blockquote>

psutil.<b>wait_procs</b><font size='3'><b><code>(</code></b></font><i>procs, timeout, callback=None</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Convenience function which waits for a list of processes to terminate. Return a <code>(gone, alive)</code> tuple indicating which processes are gone and which ones are still alive. The gone ones will have a new 'retcode' attribute indicating process exit status (may be <code>None</code>). <code>callback</code> is a callable function which gets called every time a process terminates (a <code>Process</code> instance is passed as callback argument). Function will return as soon as all  processes terminate or when timeout occurs. Tipical use case is:<br>
</blockquote><ul><li>send SIGTERM to a list of processes<br>
</li><li>give them some time to terminate<br>
</li><li>send SIGKILL to those ones which are still alive<br>
</li></ul><blockquote>Example:</blockquote>

<pre><code>def on_terminate(proc):<br>
    print("process {} terminated".format(proc))<br>
<br>
for p in procs:<br>
   p.terminate()<br>
gone, alive = wait_procs(procs, 3, callback=on_terminate)<br>
for p in alive:<br>
    p.kill()<br>
</code></pre>

<blockquote><i><b>New in 1.2.0</b></i></blockquote>

psutil.<b>get_process_list</b><font size='3'><b><code>()</code></b></font><br>
<blockquote>Same as <code>process_iter()</code> but return a list instead.</blockquote>

<blockquote><b><font color='red'>Deprecated in 0.5.0</font></b><i></blockquote></i>


<hr />

<h3>CPU</h3>

psutil.<b>cpu_percent</b><font size='3'><b><code>(</code></b></font><i>interval=0.1, percpu=False</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return a float representing the current system-wide CPU utilization as a percentage. When interval is > 0.0 compares system CPU times elapsed before and after the interval (blocking). When interval is 0.0 or None compares system CPU times elapsed since last call or module import, returning immediately. In this case is recommended for accuracy that this function be called with at least 0.1 seconds between calls.<br>When <i>percpu</i> is True returns a list of floats representing the utilization as a percentage for each CPU.  First element of the list refers to first CPU, second element to second CPU and so on. The order of the list is consistent across calls.</blockquote>


<pre><code>&gt;&gt;&gt; # blocking, system-wide<br>
&gt;&gt;&gt; psutil.cpu_percent(interval=1)<br>
2.0<br>
&gt;&gt;&gt;<br>
&gt;&gt;&gt; # blocking, per-cpu<br>
&gt;&gt;&gt; psutil.cpu_percent(interval=1, percpu=True)<br>
[2.0, 1.0]<br>
&gt;&gt;&gt;<br>
&gt;&gt;&gt; # non-blocking (percentage since last call)<br>
&gt;&gt;&gt; psutil.cpu_percent(interval=0)<br>
2.9<br>
&gt;&gt;&gt;<br>
</code></pre>

<blockquote><i><b>Changed in 0.2.0:</b> interval parameter was added</i><br><i><b>Changed in 0.3.0:</b> percpu parameter was added</i></blockquote>

psutil.<b>cpu_times</b><font size='3'><b><code>(</code></b></font><i>percpu=False</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return system CPU times as a namedtuple. Every attribute represents the time CPU has spent in the given mode.<br>The attributes availability varies depending on the platform.  Here follows a list of all available attributes:<br>
<ul><li><b>user</b>
</li><li><b>system</b>
</li><li><b>idle</b>
</li><li><b>nice</b> <i>(UNIX)</i>
</li><li><b>iowait</b> <i>(Linux)</i>
</li><li><b>irq</b> <i>(Linux, FreeBSD)</i>
</li><li><b>softirq</b> <i>(Linux)</i>
</li><li><b>steal</b> <i>(Linux >= 2.6.11)</i>
</li><li><b>guest</b> <i>(Linux >= 2.6.24)</i>
</li><li><b>guest_nice</b> <i>(Linux >= 3.2.0)</i></li></ul></blockquote>

<blockquote>When <i>percpu</i> is True return a list of nameduples for each CPU. First element of the list refers to first CPU, second element to second CPU and so on. The order of the list is consistent across calls.</blockquote>

<blockquote><i><b>Changed in 0.3.0:</b> percpu parameter was added</i><br><b>Changed in 0.7.0:</b> added 'steal', 'guest', and 'guest_nice'<i></blockquote></i>

psutil.<b>cpu_times_percent</b><font size='3'><b><code>(</code></b></font><i>interval=0.1, percpu=False</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Same as <i>cpu_percent()</i> but provides utilization percentages for each specific CPU time as is returned by <i>cpu_times()</i>. <i>interval</i> and <i>percpu</i> arguments have the same meaning as in <i>cpu_percent()</i>.</blockquote>

<blockquote><i><b>Added in 0.7.0.</b></i></blockquote>

<hr />

<h3>Memory</h3>

psutil.<b>virtual_memory</b><font size='3'><b><code>()</code></b></font><br>
<blockquote>Return statistics about system memory usage as a namedtuple including the following fields, expressed in bytes:<br>
<ul><li><b>total:</b> total physical memory available<br>
</li><li><b>available:</b> the actual amount of available memory that can be given instantly to processes that request more memory in bytes; this is calculated by summing different memory values depending on the platform (e.g. free + buffers + cached on Linux) and it is supposed to be used to monitor actual memory usage in a cross platform fashion.<br>
</li><li><b>percent:</b> the percentage usage calculated as <code>(total - available) / total * 100</code>
</li><li><b>used:</b> memory used, calculated differently depending on the platform and designed for informational purposes only.<br>
</li><li><b>free:</b> memory not being used at all (zeroed) that is readily available; note that this doesn't reflect the actual memory available (use 'available' instead).</li></ul></blockquote>

<blockquote>Platform-specific fields:<br>
<ul><li><b>active</b> <i>(UNIX)</i>: memory currently in use or very recently used, and so it is in RAM.<br>
</li><li><b>inactive</b> <i>(UNIX)</i>: memory that is marked as not used.<br>
</li><li><b>buffers</b> <i>(Linux, BSD)</i>: cache for things like file system metadata.<br>
</li><li><b>cached</b> <i>(Linux, BSD)</i>: cache for various things.<br>
</li><li><b>wired</b> <i>(BSD, OSX)</i>: memory that is marked to always stay in RAM. It is never moved to disk.<br>
</li><li><b>shared</b> <i>(BSD)</i>: memory that may be simultaneously accessed by multiple processes.</li></ul></blockquote>

<blockquote>The sum of 'used' and 'available' does not necessarily equal total. On Windows 'available' and 'free' are the same.</blockquote>

<pre><code>&gt;&gt;&gt; psutil.virtual_memory()<br>
vmem(total=8374149120L, available=1247768576L, percent=85.1, used=8246628352L, free=127520768L, active=3208777728, inactive=1133408256, buffers=342413312L, cached=777834496)<br>
</code></pre>

<blockquote><i><b>New in 0.6.0</b></i></blockquote>

psutil.<b>swap_memory</b><font size='3'><b><code>()</code></b></font><br>
<blockquote>Return system swap memory statistics as a namedtuple including the following attributes:<br>
<ul><li><b>total:</b> total swap memory in bytes<br>
</li><li><b>used:</b> used swap memory in bytes<br>
</li><li><b>free:</b> free swap memory in bytes<br>
</li><li><b>percent:</b> the percentage usage<br>
</li><li><b>sin:</b> no. of bytes the system has swapped in from disk (cumulative)<br>
</li><li><b>sout:</b> no. of bytes the system has swapped out from disk (cumulative)</li></ul></blockquote>

<blockquote>'sin' and 'sout' on Windows are meaningless and always set to 0.</blockquote>

<pre><code>&gt;&gt;&gt; psutil.swap_memory()<br>
swap(total=2097147904L, used=886620160L, free=1210527744L, percent=42.3, sin=1050411008, sout=1906720768)<br>
</code></pre>

<blockquote><i><b>New in 0.6.0</b></i></blockquote>

psutil.<b>phymem_usage</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>virtmem_usage</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>cached_phymem</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>phymem_buffers</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>avail_phymem</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>used_phymem</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>total_virtmem</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>avail_virtmem</b><font size='3'><b><code>()</code></b></font><br>
psutil.<b>used_virtmem</b><font size='3'><b><code>()</code></b></font><br>
<blockquote>These functions are deprecated by <i>psutil.virtual_memory()</i> and <i>psutil.swap_memory()</i>. Use them instead.</blockquote>

<blockquote><b><font color='red'>Deprecated in 0.3.0 and 0.6.0</font></b><i></blockquote></i>

<hr />

<h3>Disks</h3>

psutil.<b>disk_partitions</b><font size='3'><b><code>(</code></b></font><i>all=False</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return all mounted disk partitions as a list of namedtuples including device, mount point and filesystem type, similarly to "df" command on posix. <br>If <i>all</i> parameter is False return physical devices only (e.g. hard disks, cd-rom drives, USB keys) and ignore all others (e.g. memory partitions such as <a href='http://www.cyberciti.biz/tips/what-is-devshm-and-its-practical-usage.html'>/dev/shm</a>).<br> Namedtuple's 'fstype' field is a string which varies depending on the platform. <br>On Linux it can be one of the values found in /proc/filesystems (e.g. 'ext3' for an ext3 hard drive o 'iso9660' for the CD-ROM drive). <br>On Windows it is determined via <a href='http://msdn.microsoft.com/en-us/library/aa364939(v=vs.85).aspx'>GetDriveType</a> and can be either "removable", "fixed", "remote", "cdrom", "unmounted" or "ramdisk". <br>On OSX and FreeBSD it is retrieved via <a href='http://www.manpagez.com/man/2/getfsstat/'>getfsstat(2)</a>. <br>See <a href='https://code.google.com/p/psutil/source/browse/examples/disk_usage.py'>examples/disk_usage.py</a> script providing an example usage.<br>
<pre><code>&gt;&gt;&gt; psutil.disk_partitions()<br>
[partition(device='/dev/sda3', mountpoint='/', fstype='ext4', opts='rw,errors=remount-ro'),<br>
 partition(device='/dev/sda7', mountpoint='/home', fstype='ext4', opts='rw')]<br>
&gt;&gt;&gt;<br>
</code></pre></blockquote>

<blockquote><i><b>New in 0.3.0</b></i><br><i><b>Changed in 0.5.0</b>: namedtuple now includes a new 'opts' field</i></blockquote>

psutil.<b>disk_usage</b><font size='3'><b><code>(</code></b></font><i>path</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return disk usage statistics about the given <i>path</i> as a namedtuple including total, used and free space expressed in bytes plus the percentage usage. OSError is raised if path does not exist. See <a href='https://code.google.com/p/psutil/source/browse/examples/disk_usage.py'>examples/disk_usage.py</a> script providing an example usage.<br>
<pre><code>&gt;&gt;&gt; psutil.disk_usage('/')<br>
usage(total=21378641920, used=4809781248, free=15482871808, percent=22.5)<br>
</code></pre></blockquote>

<blockquote><i><b>New in 0.3.0</b></i></blockquote>

psutil.<b>disk_io_counters</b><font size='3'><b><code>(</code></b></font><i>perdisk=False</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return system disk I/O statistics as a namedtuple including the following attributes:<br>
<ul><li><b>read_count</b>: number of reads<br>
</li><li><b>write_count</b>: number of writes<br>
</li><li><b>read_bytes</b>: number of bytes read<br>
</li><li><b>write_bytes</b>: number of bytes written<br>
</li><li><b>read_time</b>: time spent reading from disk (in milliseconds)<br>
</li><li><b>write_time</b>: time spent writing to disk (in milliseconds)</li></ul></blockquote>

<blockquote>If perdisk is True return the same information for every physical disk installed on the system as a dictionary with partition ames as the keys and the namedutuple described above as the values.</blockquote>

<pre><code>&gt;&gt;&gt; psutil.disk_io_counters()<br>
iostat(read_count=8141, write_count=2431, read_bytes=290203, write_bytes=537676, read_time=5868, write_time=94922)<br>
&gt;&gt;&gt;<br>
&gt;&gt;&gt; psutil.disk_io_counters(perdisk=True)<br>
{'sda1': iostat(read_count=920, write_count=1, read_bytes=2933248, write_bytes=512, read_time=6016, write_time=4),<br>
 'sda2': iostat(read_count=18707, write_count=8830, read_bytes=6060, write_bytes=3443, read_time=24585, write_time=1572),<br>
 'sdb1': iostat(read_count=161, write_count=0, read_bytes=786432, write_bytes=0, read_time=44, write_time=0)}<br>
<br>
</code></pre>

<blockquote><i><b>New in 0.4.0</b></i></blockquote>

<hr />

<h3>Network</h3>

psutil.<b>net_io_counters</b><font size='3'><b><code>(</code></b></font><i>pernic=False</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return network I/O statistics as a namedtuple including the following attributes:<br>
<ul><li><b>bytes_sent</b>: number of bytes sent<br>
</li><li><b>bytes_recv</b>: number of bytes received<br>
</li><li><b>packets_sent</b>: number of packets sent<br>
</li><li><b>packets_recv</b>: number of packets received<br>
</li><li><b>errin</b>: total number of errors while receiving<br>
</li><li><b>errout</b>: total number of errors while sending<br>
</li><li><b>dropin</b>: total number of incoming packets which were dropped<br>
</li><li><b>dropout</b>: total number of outgoing packets which were dropped (always 0 on OSX and BSD)</li></ul></blockquote>

<blockquote>If pernic is True return the same information for every network interface installed on the system as a dictionary with network interface names as the keys and the namedtuple described above as the values.</blockquote>

<pre><code>&gt;&gt;&gt; psutil.network_io_counters()<br>
iostat(bytes_sent=14508483, bytes_recv=62749361, packets_sent=84311, packets_recv=94888, errin=0, errout=0, dropin=0, dropout=0)<br>
&gt;&gt;&gt;<br>
&gt;&gt;&gt; psutil.network_io_counters(pernic=True)<br>
{'lo': iostat(bytes_sent=547971, bytes_recv=547971, packets_sent=5075, packets_recv=5075, errin=0, errout=0, dropin=0, dropout=0),<br>
 'wlan0': iostat(bytes_sent=13921765, bytes_recv=62162574, packets_sent=79097, packets_recv=89648, errin=0, errout=0, dropin=0, dropout=0)}<br>
</code></pre>

<blockquote><i><b>New in 0.4.0</b></i><br><i><b>Changed in 0.6.0:</b> added errin, errout, dropin, dropout fields</i></blockquote>

psutil.<b>network_io_counters</b><font size='3'><b><code>(</code></b></font><i>pernic=False</i><font size='3'><b><code>)</code></b></font><br>
<blockquote>Old name for net_io_counters().</blockquote>

<blockquote><b><font color='red'>Deprecated in 1.0.0</font></b><i></blockquote></i>

<hr />

<h3>Other system info</h3>

psutil.<b>get_users</b><font size='3'><b><code>(</code></b></font><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return users currently connected on the system as a list of namedtuples including the following attributes:<br>
<ul><li><b>user</b>: the name of the user<br>
</li><li><b>terminal</b>: the tty or pseudo-tty associated with the user, if any.<br>
</li><li><b>host</b>: the host name associated with the entry, if any.<br>
</li><li><b>started</b>: the creation time as a floating point number expressed in seconds since the epoch.</li></ul></blockquote>

<pre><code>&gt;&gt;&gt; psutil.get_users()<br>
[user(name='giampaolo', terminal='pts/2', host='localhost', started=1340737536.0),<br>
 user(name='giampaolo', terminal='pts/3', host='localhost', started=1340737792.0)]<br>
</code></pre>

<blockquote><i><b>New in 0.5.0</b></i></blockquote>

psutil.<b>get_boot_time()</b><font size='3'><b><code>(</code></b></font><font size='3'><b><code>)</code></b></font><br>
<blockquote>Return the system boot time expressed in seconds since the epoch. This is also available as <code>psutil.BOOT_TIME</code> but reflects system clock updates.</blockquote>

<hr />

<h2>Constants</h2>

psutil.<b>TOTAL_PHYMEM</b><br>
<blockquote>The amount of total physical memory on the system, in bytes. This is the same as <code>psutil.virtual_memory().total</code>.</blockquote>

psutil.<b>NUM_CPUS</b><br>
<blockquote>The number of CPUs on the system. This is preferable than using <code>os.environ['NUMBER_OF_PROCESSORS']</code> as it is more accurate and always available.</blockquote>

psutil.<b>BOOT_TIME</b><br>
<blockquote>A number indicating the system boot time expressed in seconds since the epoch.</blockquote>

<blockquote><b><b>New in 0.2.1</b></br><font color='red'>Deprecated in 0.7.0: use psutil.get_boot_time()</font></b><i></blockquote></i>

psutil.<b>ABOVE_NORMAL_PRIORITY_CLASS</b><br>
psutil.<b>BELOW_NORMAL_PRIORITY_CLASS</b><br>
psutil.<b>HIGH_PRIORITY_CLASS</b><br>
psutil.<b>IDLE_PRIORITY_CLASS</b><br>
psutil.<b>NORMAL_PRIORITY_CLASS</b><br>
psutil.<b>REALTIME_PRIORITY_CLASS</b><br>
<blockquote>A set of integers representing the priority of a process on Windows (see <a href='http://msdn.microsoft.com/en-us/library/ms686219(v=vs.85).aspx'>MSDN documentation</a>). They can be used in conjunction with <code>psutil.Process.nice</code> to get or set process priority.</blockquote>

<blockquote><i><b>New in 0.2.1</b></i><br><i><b>Availability:</b> Windows</i></blockquote>

psutil.<b>IOPRIO_CLASS_NONE</b><br>
psutil.<b>IOPRIO_CLASS_RT</b><br>
psutil.<b>IOPRIO_CLASS_BE</b><br>
psutil.<b>IOPRIO_CLASS_IDLE</b><br>
<blockquote>A set of integers representing the I/O priority of a process on Linux. They can be used in conjunction with <code>psutil.Process.get_ionice()</code> and <code>psutil.Process.set_ionice()</code> to get or set process I/O priority. For further information refer to <a href='http://linux.die.net/man/1/ionice'>ionice</a> command line utility or <a href='http://linux.die.net/man/2/ioprio_get'>ioprio_get</a> system call.</blockquote>

<blockquote><i><b>New in 0.2.1</b></i><br><i><b>Availability:</b> Linux</i></blockquote>

psutil.<b>RLIMIT_INFINITY</b><br>
psutil.<b>RLIMIT_AS</b><br>
psutil.<b>RLIMIT_CORE</b><br>
psutil.<b>RLIMIT_CPU</b><br>
psutil.<b>RLIMIT_DATA</b><br>
psutil.<b>RLIMIT_FSIZE</b><br>
psutil.<b>RLIMIT_LOCKS</b><br>
psutil.<b>RLIMIT_MEMLOCK</b><br>
psutil.<b>RLIMIT_MSGQUEUE</b><br>
psutil.<b>RLIMIT_NICE</b><br>
psutil.<b>RLIMIT_NOFILE</b><br>
psutil.<b>RLIMIT_NPROC</b><br>
psutil.<b>RLIMIT_RSS</b><br>
psutil.<b>RLIMIT_RTPRIO</b><br>
psutil.<b>RLIMIT_RTTIME</b><br>
psutil.<b>RLIMIT_RTPRIO</b><br>
psutil.<b>RLIMIT_SIGPENDING</b><br>
psutil.<b>RLIMIT_STACK</b><br>
<blockquote>Constants used for getting and setting process resource limits to be used in conjunction with <code>psutil.Process.get_rlimit()</code> and <code>psutil.Process.set_rlimit()</code>. See <a href='http://linux.die.net/man/2/prlimit'>man prlimit</a> for futher information.</blockquote>

<blockquote><i><b>New in 1.1.0</b></i><br><i><b>Availability:</b> Linux</i></blockquote>

psutil.<b>STATUS_RUNNING</b><br>
psutil.<b>STATUS_SLEEPING</b><br>
psutil.<b>STATUS_DISK_SLEEP</b><br>
psutil.<b>STATUS_STOPPED</b><br>
psutil.<b>STATUS_TRACING_STOP</b><br>
psutil.<b>STATUS_ZOMBIE</b><br>
psutil.<b>STATUS_DEAD</b><br>
psutil.<b>STATUS_WAKE_KILL</b><br>
psutil.<b>STATUS_WAKING</b><br>
psutil.<b>STATUS_IDLE</b><br>
psutil.<b>STATUS_LOCKED</b><br>
psutil.<b>STATUS_WAITING</b><br>
<blockquote>A set of integers representing the status of a process. To be used in conjunction with <code>psutil.Process.status</code> property.</blockquote>

<blockquote><i><b>New in 0.2.1</b></i><br><b>Changed in 1.1.0:</b> turned into python strings (instead of int-like objects with a nice str() representation)<br><i></blockquote></i>

psutil.<b>CONN_ESTABLISHED</b><br>
psutil.<b>CONN_SYN_SENT</b><br>
psutil.<b>CONN_SYN_RECV</b><br>
psutil.<b>CONN_FIN_WAIT1</b><br>
psutil.<b>CONN_FIN_WAIT2</b><br>
psutil.<b>CONN_TIME_WAIT</b><br>
psutil.<b>CONN_CLOSE</b><br>
psutil.<b>CONN_CLOSE_WAIT</b><br>
psutil.<b>CONN_LAST_ACK</b><br>
psutil.<b>CONN_LISTEN</b><br>
psutil.<b>CONN_CLOSING</b><br>
psutil.<b>CONN_NONE</b><br>
psutil.<b>CONN_DELETE_TCB</b>  <i>(Windows only)</i><br>
psutil.<b>CONN_IDLE</b>  <i>(Solaris only)</i><br>
psutil.<b>CONN_BOUND</b>  <i>(Solaris only)</i><br>
<blockquote>TCP connection status as returned by <code>Process.get_connections()</code>'s <code>status</code> field.  If used with str() return a human-readable status string. The str() representation can also be used for comparison.</blockquote>

<blockquote><i><b>New in 1.0.0</b></i><br><b>Changed in 1.1.0:</b> turned into python strings (instead of int-like objects with a nice str() representation)