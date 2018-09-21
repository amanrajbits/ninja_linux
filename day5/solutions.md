Check the Memory used by a process:
$ free -h 
$ top
$ htop

Top command also gives the load average details in three frequencies(1min, 5min and 15min)
High load averages imply that a system is overloaded; many processes are waiting for CPU time.
The load averages shown by these tools is read /proc/loadavg file
$ cat /proc/loadavg

Total number of open files by a process:
lsof meaning ‘LiSt Open Files’ is used to find out which files are open by which process.
One of the reason to use lsof command is when a disk cannot be unmounted as it says the files are being used. 
$ lsof
FD: when we open a file, the operating system creates an entry to represent that file and store the information about that opened file. So if there are 100 files opened in your OS then there will be 100 entries in OS (somewhere in kernel). This entry number is the file descriptor. 
FD: column indicates file descriptor
cwd current working directory 
rtd root directory lsof
txt program text (code and data) 
mem memory-mapped file 
FD column numbers like 1u is actual file descriptor and followed by u,r,w of it’s mode as:
1. r for read access. 
2. w for write access. 
3. u for read and write access. 
TYPE – of files and it’s identification.
1. DIR – Directory 
2. REG – Regular file 
3. CHR – Character special file. 
4. FIFO – First In First Out 

List User Specific Opened Files
$ lsof -u username

Find Out who’s Looking What Files and Commands
$ lsof -i -u username

Find Processes running on Specific Port
$ lsof -i TCP:22
$ lsof -i TCP:22-1024

Search by PID
$ lsof -p 1

List Only IPv4 files 
$ lsof -i 4

Exclude User with ‘^’ Character
$ lsof -i -u^ root

Kill all Activity of Particular User
$ kill -9 `lsof -t -u tecmint`

Kill:
kill command to terminates stalled or unwanted processes without having to log out or restart the server.
The kill command sends the specified signal such as kill process to the specified process or process groups. 
If no signal is sent then SIGTRERM will taken (signal 15)

List of common term signals:
SIGHUP (1) – Hangup detected on controlling terminal or death of controlling process. Use SIGHUP to reload configuration files and open/close log files.
SIGKILL (9) – Kill signal. Use SIGKILL as a last resort to kill process. This will not save data or cleaning kill the process.
SIGTERM (15) – Termination signal. This is the default and safest way to kill process.

$ kill <signal> PID
$ kill -15 PID
$ kill -9 PID
$ kill PID1 PID2
$ killall -15 <process name>

Permissions to kill process
user can kill his own process
Only root user can kill system level process.
Only root user can kill process started by other users.

PID:
A Linux or Unix process is running instance of a program. Each time you start any process, the system is automatically assigned a unique process identification number (PID).

In Linux, an executable stored on disk is called a program, and a program loaded into memory and running is called a process. When a process is started, it is given a unique number called process ID (PID) that identifies that process to the system.

$ pidof <process name>   		(pidof apache2)
 
$ ps -ef 
$ ps -aux |grep apache2 

PPID:
each process is assigned a parent process ID (PPID) that tells which process started it. The PPID is the PID of the process’s parent.
Init is grandfather of all processors
all processes running in linux can trace their relationship back to init

Importance of PPID:
Occasionally, processes go bad. You might try to quit a program only to find that it has other intentions. The process might continue to run or use up resources even though its interface closed. Sometimes, this leads to what is called a zombie process, a process that is still running, but dead.
One effective way to kill a zombie process is to kill its parent process.
Print PID of current shell
$ echo $$
$ echo $BASHPID

How to clear a log file of running process
sudo cat /dev/null > /var/log/httpd/access_log

