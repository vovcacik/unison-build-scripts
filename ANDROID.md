# Notes on Android

###### Android SSH server
I have found [Termux][] to be pretty good. It gives you linux-ish environment (bash, home dir, packages etc.) and actual [OpenSSH][] and not just [Dropbear][].

###### Using unison as non-root
You can run unison on Android as root or non-root user. I would prefer the latter, but you may run into this problem:
```
Fatal error: Warning: the archives are locked.
If no other instance of unison is running, the locks should be removed.
The file /data/data/com.termux/files/home/.unison/lk444065e903864397d2dfd6494cff03d2 on host localhost should be deleted.
Please delete lock files as appropriate and try again.
```

I had to disable SELinux to workaround this problem, but it is rather dirty.
```
# getenforce
Enforcing
# setenforce 0
# getenforce
Permissive
```

###### Permissions
You may not have high enough rights to set permissions, in that case add this to your config file:
```
perms = 0
dontchmod = true
```

###### Syncing main storage
You can symlink other directories to your home dir, which may be located in strange places depending on your ssh server (`/data/data/com.termux/files/home` for [Termux][]).
```
ln -s /storage/emulated/0 ~/storage
```

You may need to:
- manually give permission to read/write main storage to the ssh app in your Android's settings; in my case I granted the rights to [Termux][]
- `unison` will follow the symlink and sync the contents regardless your `follow` instructions. However `unison-fsmonitor` will not descent into the symlinked directory if you won't add `follow = Path ./` in your configuration.

###### Recommended profile file
```
# Ignore permissions
perms = 0
dontchmod = true

# Let unison-fsmonitor to follow symlinked root
follow = Path ./
```

[Termux]: https://termux.com/
[OpenSSH]: http://www.openssh.com/
[Dropbear]: https://en.wikipedia.org/wiki/Dropbear_(software)
