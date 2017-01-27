---
layout: post
title:  How to kill your VM&#58; limits.d and the Magic of dotfiles
date:   2017-1-27
categories: linux programming
---

I was benchmark testing [StatsD](https://github.com/etsy/statsd) on my VM this week, and was seeing 40% package drop rate right out of the box. So, I increased my UNIX limits by creating a `/usr/security/limits.d/limits.conf` as such:

```
*               soft    nofile           unlimited
*               hard    nofile           unlimited
*               soft    nproc            unlimited
*               hard    nproc            unlimited
```
Which essentially says, “For all users, set soft and hard limits for the number of processes and number of open files to unlimited”.

When you do this, your VM stops accepting all new shell instances, meaning you can't SSH into your machine. It also means that trying to run a remote command, like, say `ssh boguinn 'rm /usr/security/limits.d/limits.conf'` in the vain hope of rescuing your machine is not possible.

Why? Because the Linux kernel has a [hard upper limit](http://four-eyes.net/2012/09/etcsecuritylimits-conf-nofile-absolute-maximum/) of 1048576 (or 1024<sup>2</sup>) open files. And because I optimistically[^opt] applied that to all users and groups by providing `*` in the first column, there were no exempt users that could correct the mistake.

## How to Fix It

In my panic of not being able to access my VM, I ran my laptop over to the IT department. Together, we tried SSHing into my VM through various methods: under different users, under root. We even tried manually rebooting the machine, all to no avail. So we the terrible thing and killed and re-provisioned the machine.

With sunken head I trudged back to my desk, only to discover that *I still had a shell open to the VM running a go task*. At this point it was too late, but because limits.d is only read and applied on the initialization of a shell, I could have used my old session to delete the  offending `limits.conf` .

## An Ounce of Prevention

Later that day I pinged other engineers about if I could have done anything differently. According to them, it might have been possible to salvage the machine by using `virsh`[^virsh] to get into the machine without loading limits.conf. Who knew!

Now that I had a completely fresh VM to re-provision, I realized that I could really use a dotfiles repo. This is a collection of shell scripts and global conf files you keep in a VCS so that whenever you enter a new machine, it will automatically be set up the way you like, with the correct shell interpreter, binaries, and themes.

I found [nicksp's dotfiles](https://github.com/nicksp/dotfiles), which includes [sensible OS X defaults](https://github.com/nicksp/dotfiles/blob/master/osx/set-defaults.sh), (as well as support for oh-my-zsh, npm, and a nifty powershell-based theme) via the [Github dotfiles page](github.dotfies.io). However, it includes several commands and settings that don't work on my CENTOS 7-based VM, and I wanted the opportunity to configure the aesthetics of the theme.

So, I forked it! You can check out my own [dotfiles](https://github.com/broguinn/dotfiles), and run it on most *NIX systems with `./setup-centos.sh`. Fork it, customize away, and worry (less) about borking your machine.

**Disclaimer:** never run any script on your machine, particularly one for provisioning, without thoroughly reading and understanding every line.

## tl;dr

1. Set limits.conf overrides to a specific test user
2. Setting nofile to anything higher than 1048576, including `unlimited`, locks your machine
3. Talk to more people about solutions before making an irrevertable change
4. Check all your iterm windows for forgotten open SSH tunnels :cry:
5. Keep a dotfiles collection in Github

#### footnotes

[^opt]: Foolishly
[^virsh]: Which can be found [here](https://www.centos.org/docs/5/html/5.2/Virtualization/chap-Virtualization-Managing_guests_with_virsh.html)

