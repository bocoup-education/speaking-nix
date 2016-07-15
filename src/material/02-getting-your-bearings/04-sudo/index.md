# All about `sudo`

???

Hopefully as we finish chapter 2, you are feeling more comfortable following
instructions in a Unix-like environment.

There's one more topic that is important to understand: the `sudo` utility.

---

# Background: Users and permissions

```
$ whoami
sally
$
```

???

Unix was designed as a time-sharing system. Many people were expected to be
logged in and working at the same time. The system recognizes "users" as a way
to promote privacy while still allowing people to share files and devices.

---

:continued:

```
$ echo $HOME
/home/sally
$ ls /home
larry
nancy
sally
xavier
$
```

???

We've already seen some evidence of this: our `$HOME` directory isn't simply
`/home`; it's a sub-directory of `/home` named `sally`. This suggests that
there might be more directories for other people.

---

:continued:

```
$ ls /home/larry
ls: cannot open directory /home/larry: Permission denied
$
```

???

We don't generally have access to the `$HOME` directories that belong to other
users.

---

:continued:

```
$ ls -o /home
total 102
drwx------ 72 larry  24576 Jul 01 10:55 larry
drwx------ 72 nancy  8374  Jul 09 10:55 nancy
drwx------ 72 sally  39827 Jul 14 10:55 sally
drwx------ 72 xavier 33432 Jul 11 10:55 xavier
$
```

???

All files and folders have a set of "permissions" which describe exactly which
users may interact with them, and even *how* they may interact.

We'll cover this in greater detail in later sections.

---

# Background: the `root` user

???

A Unix-like system can have any number of user accounts like this. However,
there is one special user that is present on *all* Unix-like systems: the
"root" user (also known as the "superuser").

This is the administrative user account. It has no access control restrictions,
meaning it has the permissions to do everything with every file and every
directory. The root user's `HOME` directory is in a special location on the
file system: `/root`.

---

:continued:

```
$ rm -rf /bin
rm: cannot remove ‘/bin/zdiff’: Permission denied
rm: cannot remove ‘/bin/findmnt’: Permission denied
rm: cannot remove ‘/bin/bzegrep’: Permission denied
rm: cannot remove ‘/bin/gzexe’: Permission denied
rm: cannot remove ‘/bin/sh.distrib’: Permission denied
rm: cannot remove ‘/bin/chgrp’: Permission denied
rm: cannot remove ‘/bin/login’: Permission denied
```

???

This is good news for when you are learning because it means you are unable to
perform many actions that would break things. Even the person who is "in
charge" of the system (that's you in these exercises and on your personal
computer) usually has a normal user account. This limits the damage that a
malicious program can do when it runs on your behalf.

---

# `sudo`

```
$ whoami
sally
$ sudo whoami
[sudo] password for sally:
root
$
```

???

- The `sudo` utility allows us to execute single commands as the `root` user.
- There is some disagreement about where its name comes from. Some say it is
  short for "**s**uper **u**ser **do**." Because you may use the `-u` option to
  execute a command as *any* user, some argue that it is short for "**s**witch
  **u**ser and **do**."

---

# Using `sudo` safely

```
$ apt-get install cowsay
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?
$ sudo apt-get install cowsay
[sudo] password for sally: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
cowsay is already the newest version.
0 upgraded, 0 newly installed, 0 to remove and 23 not upgraded.
$
```

???

You will need to use `sudo` when performing system administration tasks like
installing software. (In this example, we're using the APT package manager from
Ubuntu.) It's generally safe to use `sudo` with system utilities and when you
understand what the command is going to do.

---

# Avoiding "unsafe" uses of `sudo`

```
$ sudo ./script-i-found-on-the-web.sh
[sudo] password for sally: 
Installing a key logger... Done
Deleting all your applications... Done
$
```

???

However, many tutorials online contain instructions that use `sudo` unsafely.
Remember that the root user can do *anything*, so using `sudo` with an
untrusted program from the Internet may cause all sorts of problems.

---

:continued:

```
$ sudo apt-get install the-dependency-i-need
[sudo] password for sally: 
$ ./script-i-found-on-the-web.sh
Performing actions as the current (de-privileged) user
$
```

???

It's much better to perform the administrative actions manually and rely on the
untrusted program to do the rest. Even this is dangerous, though--the script
could still access your personal files. There is no substitute for trusting the
code you run, whether through reading the source or through having confidence
in its origins!

---

# In Review

- The "root" user (or "superuser") is capable of performing any task
- The `sudo` utility allows you to execute single commands as the "root" user
- You should trust any program you run using `sudo`!