~# Introduction
~# General
~Secure Shell "encrypted telnet or rsh (remote shell)"
~it's a protocol, not product or application
~encrypted data transfer
authetntication, encryption, message integrity
~client-server
~various clients (scp, sftp, ...) replace others like telnet, rcp, ftp, ...
~ssh is not a true shell\*, it is the channel to the remote shell [try this: create user with non-bash, login from bash, check which shell it is]
~it affects data transfer, not data content (eg. viruses, trojans)
~RTFM!

~2 protocol versions, SSH-1 (not secure anymore) developed in 1995, SSH Communication Security
~SSH-2 developed in 1996 (incompatible with SSH-1), released by SSH C S in 1998
~IETF formed SECSH group to standardize protocol (2006)

~OpenSSH first free implementation by OpenBSD project
~2006: IETF SECSH released SSH-2 standard (RFC 4251)
~bild OSI-layer

~SSH connection establishing: Crypto handshake; encrypted connection with symmetric cipher; client authenticates
~SSH protects against: eavesdropping, data manipulation, IP spoofing (MITM)
~bad: password auth doesn't verify RSA identity of client

# Config
system-wide vs user-specific
system: (/etc/ssh) ssh\_config, sshd\_config
user: (~/.ssh) authorized\_keys, known\_hosts, \*/\*.pub, config
user config must be owned by user and unreadable by everyone else, otherwise it's ignored

## sshd\_config
PasswordAuthentication
PubkeyAuthentication
HostBased, ChallengeResponse, KeyboardInteractive
AllowGroups, AllowUsers (intersection)
DenyGroups, DenyUsers (union)
AllowAgentForwarding (?)
X11Forwarding
PermitTunnel
PermintUserEnvironment

TCPKeepAlive [yes/no]  (?)
ServerAliveInterval [sec]  (?)

Match user <username>
ForceCommand <command>

## ~/.ssh
Host dev
  User johndoe
  Hostname foobar.com
  Port 2200
  ForwardX11 yes
  Localforward 8000 10.10.10.10:80

## speedup
multiple channels per connection
multiplexing:
- ControlMaster auto
- ControlPath ~/.ssh/master/%r@%h:%p
- ControlPersist yes
TODO: test!

~# Usages
~remote sysadmin
~file transfer
~- scp -3 host1:/file1 host2:/file2 copy between hosts that don't know each other!
~run remote apps
~secure other communication
~little bandwidth
~prevalent (industry standard) - all platforms, mobile, browser

# Basic usage
## ssh [user@]host
(sample with fingerprint)
fingerprint - get from sysadmin, connect in trustyed network or just hope it's ok :)
works over IPv6

~## ssh user@host COMMAND
~(sample)

~## ssh -X user@host [COMMAND]
~(sample)

~## scp user@host:/path/to/file .
~(or sftp)

## tunnelling
encrypted, authenticated, through firewall
ssh -L <localport>:<remotehost>:<remoteport> user@host
reverse: ssh -R <remoteport>:<localhost>:<localport> user@host
options: -g to allow others to use the tunnel, -N only forwarding, no commands

~# SSH bruteforce
~(images from http://dragonresearchgroup.org/insight/sshpwauth-cloud.html)

~# Security Measures
~alternate port (less noise) - security through obscurity
~use SSH keys
~disable password or at least use a strong password
~fail2ban `http://www.fail2ban.org/wiki/index.php/Main_Page`

# Keys
pairs of public-private key
private: kept by user to authenticate
public: placed on server to identify user
ssh-copy-id
tools: ssh-keygen, ssh-agent (keeps private key in memory), ssh-add (adds key to the key agent)

~# Environment variables
~SSH\_CONNECTION
~SSH\_AUTH\_SOCK
~SSH\_CLIENT
~SSH\_TTY

# Demos
port forwarding

~# Sources
~http://www.slideshare.net/shahhe/introduction-to-ssh
~`https://systemoverlord.com/projects/ssh_presentation`
~http://www.ssh.com
~http://www.openssh.org
~`https://en.wikipedia.org/wiki/Secure_Shell`
~man ssh
~man sshd

