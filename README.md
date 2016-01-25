# SSHF (SSH Find)

`sshf` is a really simple shell script that will search your `~/.ssh/config` by available keywords (usually vhosts) and display the according ssh server to connect to.

This script should only be useful if you have hundreds of ssh hosts and cannot remember what domain is on what host.

## Usage

For example I want to check on which server [www.everythingcli.org](http://www.everythingcli.org) is hosted.

```shell
$ sshf everything
------------------------------------------------------------
 c1
------------------------------------------------------------
 @vhosts: www.everythingcli.org another-domain.com
```

> The search string will be highlighted in the @vhosts section.

Now I know that [www.everythingcli.org](http://www.everythingcli.org) is on server `c1` and I can simply connect to it:
```shell
ssh c1
```


## Setup

For the whole thing to work, the `~/.ssh/config` must be in the following format:

```shell
# @vhosts: www.example.com sub1.example.com www.another-example.com www.google.com
Host example
	HostName	example.com
	Port		10222
	User		example
	PubkeyAuthentication yes
	IdentityFile ~/.ssh/id_ed25519__example


# @vhosts: www.everythingcli.org another-domain.com
Host c1
	HostName	everythingcli.org
	Port		10222
	User		user1
	PubkeyAuthentication yes
	IdentityFile ~/.ssh/id_ed25519__ecli
```

So in words: above every `Host` there must be a comment starting with `# @vhosts: ....`.

## Examples

### Get VirtualHosts

#### Apache
To get a list of all VirtualHosts apache is serving, use the following command:
```shell
httpd -S 2>&1 | grep namevhost | awk '{print $4}' | sort -u
```

#### Nginx
```shell
find /etc/nginx/ -type f -name "*.conf" -print0 | xargs -0 egrep '^(\s|\t)*server_name' | awk '{print $3}' | sort -u
```

## More info

Read up more about it in this article at the bottom [SSH tunnelling for fun and profit: SSH Config](http://www.everythingcli.org/ssh-tunnelling-for-fun-and-profit-ssh-config/)
