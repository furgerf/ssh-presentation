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
Demo: Log in from bash-user to non-bash user

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
  - adopted as standard by IETF (2006), RFC 4251
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
???
Demo: MITM

--
.right-column[
- industry standard
]
???
Available on pretty much all platforms, mobile, browser
Demo: FireSSH plugin

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
  ### Overview
]
---

--
.right-column[
- encryption is based on public-key cryptography
]

--
.right-column[
- authentication can be ensure in one of two ways
  - either use automatically-generated key pair for encryption and password for authentication
  - or use manually-generated key pair for _both_ encryption and authentication
]

--
.right-column[
- TODO: crypto hs, conn encrypted with symmetric cipher, client authenticates
]

--
.right-column[
- authentication methods:
  - GSSAPI-based, host-based, **public key**, challenge-response, password
  - public key: grant access when public key in `~/.ssh/authorized_hosts` belongs to user/host TODO there's something missing here
  - password transmission is safe because communication is encrypted
]
???
Host-based: grant access if host is listed in `/etc/hosts.equiv` (or one of a few other files), the
username matches, and the key in `known_hosts` matches. This is inherently insecure.
challenge-response: server sends Qq and expects response (eg. Bx or PAM).

--
.right-column[
- `ssh` maintains `~/.ssh/known_hosts` file to avoid MITM attacks
]
???
MITM demo

---
layout: true
.left-column[
  ## What is SSH?
  ## Why SSH?
  ## How to SSH?
  ### Overview
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
  ### Overview
  ### Risks
  ### Configuration
]
---

--
.right-column[
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
#### Manual

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
Demo

---
count: false
.centered[
## `ssh -X user@host COMMAND`
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

--
.right-column[
- `-X`: enable X11 forwarding
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
  ## Outlook
]
---

--
.right-column[
- load balancing

- multiplexing

- VPN

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
http://dragonresearchgroup.org/insight/sshpwauth-cloud.html
http://www.ssh.com
http://www.openssh.org
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

