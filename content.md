class: center, middle

# SSH - Secure SHell

---

## Agenda

--

1. What is it?

--

2. Why bother?

--

3. How does it work?

--

3. Configuration

--

4. Usage examples

---

## Preface

--

- my main takeaway from BSE so far:
  - we are different people... with different areas of knowledge (duh!)

--
- that was the main factor when choosing a topic
  - hopefully there's something interesting for everyone
  - and maybe even something useful as well!

--
- with that in mind:
  - don't hesitate to interrupt and ask if something is unclear

---
layout: true
.left-column[
  ## What is SSH?
]
---

--
.right-column[
### Well, RTFM! 😄
![:scale 90%](images/man-ssh.png)
]

---
layout: true
.left-column[
  ## What is SSH?
  ### Overview
]
---

--
.right-column[
- "**S**ecure **SH**ell"
]

---
count: false
.right-column[
- "**S**ecure **SH**ell"
- network protocol
]

---
count: false
.right-column[
- "**S**ecure **SH**ell"
- network protocol
  ![:scale 85%](images/osi-layer.png)
]

---
count: false
.right-column[
- "**S**ecure **SH**ell"
- network protocol
  ![:scale 85%](images/osi-layer.png)
- _but, it's no application (and no shell!)_
]

???
**Demo**: Log in from bash-user to non-bash user on known host

`echo $0`: current shell

`getent passwd $LOGNAME`: user's login shell


---
count: false
.right-column[
- "**S**ecure **SH**ell"
- network protocol
  ![:scale 85%](images/osi-layer.png)
- _but, it's no application (and no shell!)_
- client-server architecture
]

---
count: false
.right-column[
- "**S**ecure **SH**ell"
- network protocol
  ![:scale 85%](images/osi-layer.png)
- _but, it's no application (and no shell!)_
- client-server architecture
- secures communication, not the messages
]

???
no replacement for anti-virus

---
layout: true
.left-column[
  ## What is SSH?
  ### Overview
  ### History
]
---

--
.right-column[
- SSH-1: initial version designed by Tatu Ylönen (1995)
  - Ylönen founded the SSH Communication Security

.center[![:scale 30%](images/sshcs.png)]
]

???

Goal of SSH Communication Security was to market and develop SSH

---
count: false
.right-column[
- SSH-1: initial version designed by Tatu Ylönen (1995)
  - Ylönen founded the SSH Communication Security

.center[![:scale 30%](images/sshcs.png)]
  - _not secure anymore!_
]

---
count: false
.right-column[
- SSH-1: initial version designed by Tatu Ylönen (1995)
  - Ylönen founded the SSH Communication Security

.center[![:scale 30%](images/sshcs.png)]
  - _not secure anymore!_

- SSH-2: various improvements over SSH-1
  - released by SCS (1998)
  - incompatible to SSH-1
  - standardized by IETF: RFC 4251 (2006)
]

---

.right-column[
- intended to replace unsecured remote login protocols (`telnet`/`rlogin`/`rsh`/...)
]

--
.right-column[
- first free SSH implementation: `OpenSSH` (OpenBSD)
]

--
.right-column[
- new secure clients such as `scp`, `sftp`, ... replace unsecured ones like `rcp` or `ftp`
]

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
]
---

--
.right-column[
- secure, encrypted communication channel
]

--
.right-column[
- guarantees message integrity
]

--
.right-column[
- uses little bandwidth
]

--
.right-column[
- user authentication
]

--
.right-column[
- ... and server authentication!
]
To be demonstrated later

--
.right-column[
- industry standard
]
???
Available on pretty much all platforms, mobile, browser

**Demo**: Show FireSSH plugin

---

.right-column[
- run commands on remote systems
]
???
eg. set-ntp on FPUs

--
.right-column[
- remote system administration
]

--
.right-column[
- remote file transfer
]

--
.right-column[
- even transfer files between hosts that have no connection to each other!
  - `scp -3 host1:/file host2:/file2`
]
???
Other remote file transfer applications can't do that!

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ### Connection
]
---

---
.right-column[
- server sends its public host key
]

--
.right-column[
- establish encryption: session key negotiation (Diffie-Hellman)
]
???
Diffie-Hellman:
- both agree on large prime number and encryption generator (like AES)
- both generate secret prime number (private key) and generate public key with the private key, encryption generator and the shared prime number
- the public keys are exchanged
- both calculate the shared session key from the own private key, the other's public key, and the original shared prime number

--
.right-column[
- authenticate user
  - based on GSSAPI, host, **public key**, challenge-response, or _password_
]
???
Host-based: grant access if host is listed in `/etc/hosts.equiv` (or one of a few other files), the
username matches, and the key in `known_hosts` matches. This is inherently insecure.

Challenge-response: server sends Qq and expects response (eg. Bx or PAM).

--
.right-column[
- `ssh` maintains `~/.ssh/known_hosts` file to avoid MITM attacks
- TODO: Move to other slide
]
???
TODO: Demo: Man-in-the-Middle attack

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ### Connection
  ### Risks
]
---

--
.right-column[
- any connection increases the attack surface
]

--
.right-column[
- brute-force attacks
  - popular usernames (May 2015)

![:scale 90%](images/bruteforce-usernames.png)
]

---

.right-column[
- any connection increases the attack surface
]

.right-column[
- brute-force attacks
  - popular usernames (May 2015)
  - popular passwords (May 2015)

![:scale 80%](images/bruteforce-passwords.png)
]

---

.right-column[
#### Measures
]

--
.right-column[
- use non-standard port
  - no real security, but less noise
]

--
.right-column[
- disable password authentication
  - use SSH keys instead
  - (or at least use strong passwords)
]

--
.right-column[
- block hosts with repeated failed logins
  - eg. [fail2ban](http://www.fail2ban.org/wiki/index.php/Main_Page)
]

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ### Connection
  ### Risks
  ### Configuration
]
---

--
.right-column[
TODO
#### System-wide configuration
- `/etc/ssh/ssh_config`, `/etc/ssh/sshd_config`
]

--
.right-column[
- 
]

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ## SSH usage
]
---

--
.right-column[
#### From the manual:

`ssh  [-1246AaCfGgKkMNnqsTtVvXxYy]  [-b  bind_address]  [-c  cipher_spec]  [-D[bind_address  :]port] [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file] [-J[user@]host[:port]] [-L address] [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address] [-S ctl_path] [-W host :port] [-w local_tun[:remote_tun]] [user@]hostname [command]`
]

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ## SSH usage
  ### Basic usage
]
---
.right-column[.centered[
## `ssh host`
]]

---
count: false
.centered[
## `ssh user@host`
]

---
count: false
.centered[
## `ssh user@host COMMAND`
]

???
**Demo**:

`ls -alh`

---
count: false
.centered[
## `ssh -X user@host COMMAND`

`-X`: enable X11 forwarding
]

???
Demo

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ## SSH usage
  ### Basic usage
  ### Options
]
---

---
.right-column[
- `-C`: enable compression
]
???
uses `gzip` - recommended for slow networks only

--
.right-column[
- `-c`: specify cipher
]
???
has security and performance implications

--
.right-column[
- `-D [bind_address:]port`: dynamic port forwarding
]
???
listens on the local address and forwards all connections to the address
unix sockets can be forwarded as well

--
.right-column[
- `-f`: fork to background, recommended when using X11-forwarding
]
???
implies `-n` (stdin > /dev/null)

--
.right-column[
- `-g`: allow remote hosts to connect to local forwarded ports
]

---
.right-column[
- `-i identity_file`: specify public key to use for authentication
]

--
.right-column[
- `-J [user@]host[:port]`: use a jump host and establish forwarding to destination from there
]
???
Demo
TODO: TEST THIS!

--
.right-column[
- `-L [bind_address:]port:host:hostport`: forward connections to the local host to the remote host/port
]
???
unix sockets can be forwarded as well

--
.right-column[
- `-N`: no remote command execution
]
???
useful when just wanting to forward ports

---
.right-column[
- `-p`: port on remote host
]

--
.right-column[
- `-R [bind_address:] port:host:hostport`: reverse port forwarding
]

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ## SSH usage
  ### Basic usage
  ### Options
  ### Environment
]
---

--
.right-column[
- some of the interesting environment variables are
  - `DISPLAY`: location of forwarded X11 server
  - `SSH_CONNECTION`: client and server of the connection (client IP, client port, server IP, server port)
  - TODO: Test with different usernames to show more variables
]

--
.right-column[
- escape characters
  - `~.`: disconnect
  - `~^Z`: background session
  - `~#`: list forwarded connections
]

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ## SSH usage
  ### Basic usage
  ### Options
  ### Environment
  ### Tunneling
]
---

--
.right-column[
- secure insecure protocol
]

--
.right-column[
- make remote service appear local
]

--
.right-column[
- make remote service appear local
]

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ## SSH usage
  ## Outlook
]
---

--
.right-column[
- load balancing

- multiplexing

- VPN

- SSH agent

- other clients
  - MoSH!

- ...
]
---
layout: true
---
### Sources
#### Information
```
https://medium.com/swlh/ssh-how-does-it-even-9e43586e4ffc
https://systemoverlord.com/projects/ssh_presentation
http://www.slideshare.net/shahhe/introduction-to-ssh
https://en.wikipedia.org/wiki/Secure_Shell
https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys
http://dragonresearchgroup.org/insight/sshpwauth-cloud.html
http://www.ssh.com
http://www.openssh.org
https://en.wikibooks.org/wiki/OpenSSH
man ssh
man sshd
```

#### Tools
```
https://github.com/apenwarr/sshuttle
https://github.com/shazow/ssh-chat
https://github.com/tombh/texttop <---?

https://github.com/gnab/remark
```

