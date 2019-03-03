# CTF checklist

Following is a list of things to try when you're brain-dead in a CTF.  For now, they're in no particular order.  This list will change over time as additional content is added.

## arp - check for hidden IPs

Sometimes a CTF organizer will hide a machine behind another (causing you to miss the machine in a network scan).  Once you have access to a machine, one of the things you should do is list the arp table via:
```c
arp -an
```
It's always a good idea to keep track of detected IPs in a target range.  If you're able, you should also take a look at the output of "ip addr", "ip route", iptables and netstat commands, as well as the contents of /etc/resolv.conf, and /etc/hosts.

## google is your friend

If the rules allow, always perform a Google search first (esp. for password cracking).  If someone else has used an online password cracker in the past, for the same hash(es), it may show up in a search.  For other challenges, there may be a write-up for the presented puzzle.  Search for the obvious keywords, along with one or more of the following: write-up, writeup, crackme, ctf and/or challenge.

Be sure to ***CHECK THE CTF RULES*** before doing this sort of thing.

## sudo - listing capabilities

If you have an actual shell on a machine, run the following:
```c
sudo -l
```
The above will list any defined sudo capabilities for the your account.  Example (assuming you're logged in as bob):
```c
User bob may run the following commands on target1.space.local:
    (root) NOPASSWD: /usr/bin/vim /home/bob/*/*/EditMe.txt
```
means that if you run:
```c
sudo vim /home/ctf/*/*/EditMe.txt
```
You'll be editing that file with root permissions.  Then, while in vim's command mode, try running:
```c
:shell
```
to temporarily exit to a shell. From there, you might be able to do things like:
```c
cat /root/flag.txt
```
to read the contents of /root/flag.txt.

## sudo - sudo plus su

For awhile, a certain vendor's serial controllers were susceptable to an issue with their non-root accounts.  While the following would not work:
```c
su - root
sudo -s
```
the following did work:
```c
sudo su - root
```
Depending on how admins dole out permissions, the above may be possible.  I've also seen it used on RHEL boxes.

