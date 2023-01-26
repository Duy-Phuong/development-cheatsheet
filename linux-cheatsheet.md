

## Curl command

[How do I post request body with Curl?](https://reqbin.com/req/c-d2nzjn3z/curl-post-body)

[Curl Command In Linux Explained + Examples How To Use It](https://phoenixnap.com/kb/curl-command?utm_source=coda&utm_medium=iframely)

### HEAD

In Curl, we can call a HEAD request in various ways, including:

- with the-Ior--headflag
- with the-X HEADor--request HEADflag

### POST

[How to perform a POST request using Curl](https://www.educative.io/edpresso/how-to-perform-a-post-request-using-curl)

```bash
curl -d '{json}' -H 'Content-Type: application/json' https://example.com/login

curl -X POST -H "Content-Type: application/json" \

   -d '{"name": "linuxize", "email": "linuxize@example.com"}' \

   https://example/contact
```

[How to make a POST request with cURL | Linuxize](https://linuxize.com/post/curl-post-request/)

## Netstat

https://www.addictivetips.com/ubuntu-linux-tips/netstat-linux/#:~:text=To%20open%20up%20a%20terminal,This%20package%20contains%20Netstat.

## ack

```bash
$ sudo apt-get install ack-grep      [On Debian, Ubuntu and Mint]
```



[How to Install and Use Ack Command in Linux with Examples](https://www.linuxshelltips.com/ack-command-in-linux/)

## tig

[LinuxOPsys: Linux How-to guide, Tutorials &amp; Tips](https://linoxide.com/how-to-install-tig-on-ubuntu-20-04/)

```bash
sudo apt install tig
```

## chown

[Learn Usage of chown (Change Ownership) Command in Linux](https://www.linuxshelltips.com/chown-command-examples/)

## Find IP

[Unix command to find IP address from hostname - Linux example | Java67](https://www.java67.com/2012/12/unix-command-to-find-ip-address-from-hostname.html?utm_source=dlvr.it&utm_medium=facebook)

Here is a list of [UNIX commands](http://javarevisited.blogspot.com/2011/04/unix-commands-tutorial-and-tips-for.html) which can be used to find the IP address :

- ifconfig
- nslookup
- hostname

```bash
# /usr/sbin/ifconfig -a
inet 192.52.32.15 netmask ffffff00 broadcast 192.52.32.255

# grep `hostname` /etc/hosts
192.52.32.15     nyk4035        nyk4035.unix.com

# nslookup `hostname`
nyk4035.unix.com       canonical name = nyk4035.unix.com
Name:   nyk4035.unix.com
Address: 192.52.32.15

Read more: https://www.java67.com/2012/12/unix-command-to-find-ip-address-from-hostname.html#ixzz7S7x47CD7
```

Read more: [Unix command to find IP address from hostname - Linux example | Java67](https://www.java67.com/2012/12/unix-command-to-find-ip-address-from-hostname.html#ixzz7S7wfsvix)

## alias

```bash
git config --list | grep alias.back

alias.back=checkout -

alias.back2=checkout @{-1}
```

## copy

```bash
cp pom.xml{,.backup}
```

cp pom.xml{,.backup}

## p10k configure

Reconfigure

```bash
p10k configure
```
