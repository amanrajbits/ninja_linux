Check the Memory used by a process:
$ free -h 
$ top (used to know the memory and also gives the cpu usage)
$ htop
$ vmstat (used to know the virtual memory used by a process)

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
What init process is responsible for?
The program init is the process with process ID 1. It is responsible for initializing the system in the required way.
init is centrally configured in the /etc/inittab file where the runlevels are defined.
The file also specifies which services and daemons are available in each of the runlevels.
Depending on the entries in /etc/inittab, several scripts are run by init called init scripts, all reside in the directory /etc/init.d.
The entire process of starting the system and shutting it down is maintained by init.
init is started directly by the kernel and resists signal 9, which normally kills processes. All other programs are either started directly by init or by one of its child processes.

What are Running, Waiting, Stopped and Zombie processes? 
Running:  running is the current process in the system or it’s ready to run (it’s waiting to be assigned to one of the CPUs). 
Waiting : In this state, a process is waiting for an event to occur or for a system resource. Additionally, the kernel also differentiates between two types of waiting processes; interruptible waiting processes – can be interrupted by signals and uninterruptible waiting processes – are waiting directly on hardware conditions and cannot be interrupted by any event/signal.
Stopped: In this state, a process has been stopped, usually by receiving a signal. For instance, a process that is being debugged.
Zombie : Here, a process is dead, it has been halted but it’s still has an entry in the process table.

What are stdin, stdout, and stderr and how do we use them? 
Linux is built being able to run instructions from the command line using switches to create the output. The question of course is how do we make use of that? One of the ways to make use of this is by using the three special file descriptors - stdin, stdout and stderr. 
Standard input - this is the file handle that process reads to get information from user. 
Standard output - Process writes normal information to this file handle.
Standard error - process writes error information to this file handle.

What filter and nat table responsible for?
Filter Table: It is default table for iptables. Iptables’s filter table has the following built-in chains. 
INPUT chain – Incoming to firewall. For packets coming to the local server.
OUTPUT chain – Outgoing from firewall. For packets generated locally and going out of the local server.
FORWARD chain – Packet for another NIC on the local server. For packets routed through the local server.
NAT table: Iptable’s NAT table has the following built-in chains.
PREROUTING chain – Alters packets before routing. i.e Packet translation happens immediately after the packet comes to the system (and before routing). This helps to translate the destination ip address of the packets to something that matches the routing on the local server. This is used for DNAT (destination NAT).
POSTROUTING chain – Alters packets after routing. i.e Packet translation happens when the packets are leaving the system. This helps to translate the source ip address of the packets to something that might match the routing on the desintation server. This is used for SNAT (source NAT).
OUTPUT chain – NAT for locally generated packets on the firewall.

What is the difference between -I and -A while applying a rule in iptables?
Insert one or more rules in the selected chain as the given rule number. So, if the rule number is 1,  the  rule  or  rules  are inserted  at the head of the chain.
The -A option appends one or more rules to the end of the selected chain. When the source and/or destination names resolve  to  more  than  one address, a rule will be added for each possible address combination.

How do you check all the running process in the system?
$ top (by using top command we can check the running process in the system, CPU usage and also the load average. 
