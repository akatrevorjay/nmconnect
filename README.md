nmconnect
=========

A wrapper script around nmcli, as it's interface isn't the easiest to work with from a tty.

* Quickly search for and connect to a `NetworkManager` profile from the CLI without worrying about UUIDs or other assorted carp.

It's a huge time saver for me, and you don't have to be in X to use it.

Requirements
------------

* Bash 4+

Standing on the shoulders of giants
-----------------------------------

I must thank the ABS guide that I always find to be a tremendously useful reference while writing in bash: http://tldp.org/LDP/abs/html/abs-guide.html

Installation
------------

1. Copy nmconnect and symlinks somewhere in your PATH.
```sh
$ cd "$HOME/.bin"
$ cp -av /path/to/this/folder/{nmconnect,vpn,lan,wlan} ./
```

Usage
-----

What the commands do:
* `nmconnect`: works on the full connection list
* `vpn` `lan` `wlan`: automatically adds `type=vpn` to the search, so `vpn AwesomeCo` is the same as running `nmconnect type=vpn AwesomeCo`
 
### Examples

#### List all connections

```sh
$ nmconnect
[*]
0. "Auto eth0" (type="802-3-ethernet", uuid="guid-guid-guid-guid-guid")
1. "Auto ILoveSSIDBeacons" (type="802-11-wireless", uuid="guid-guid-guid-guid-guid")
3. "AwesomeCo" (type="vpn", uuid="guid-guid-guid-guid-guid")
```

#### Search all connections for "auto" (search params are case insensitive)

```sh
$ nmconnect auto
[search=auto]
0. "Auto eth0" (type="802-3-ethernet", uuid="guid-guid-guid-guid-guid")
1. "Auto ILoveSSIDBeacons" (type="802-11-wireless", uuid="guid-guid-guid-guid-guid")
```

#### Shortcuts

The `vpn`/`lan`/`wlan` commands are merely shortcuts, as is shown by the search params (the first line it prints; in the `[brackets]`)

`vpn AwesomeCo` and `nmconnect type=vpn awesomeco` are synonymous.

```sh
$ vpn
[type=vpn] ## <-- SEARCH PARAMS
3. "AwesomeCo" (type="vpn", uuid="guid-guid-guid-guid-guid")
```

```sh
$ nmconnect type=vpn
[type=vpn]
3. "AwesomeCo" (type="vpn", uuid="guid-guid-guid-guid-guid")
```

If you narrow it down to a single result, it asks you if you wish to connect to it.
The default, as shown by the capital `Y`, is to connect, so just hit `enter` and it will connect you.
If you don't wish to connect, either `ctrl+c` or type `n<enter>` and it won't.

```sh
$ vpn awesome
[type=vpn search=awesome]
3. "AwesomeCo" (type="vpn", uuid="guid-guid-guid-guid-guid")
--> Start connection to: "AwesomeCo"? [Y/n]
```

#### Narrowing down to the connection number

```sh
$ vpn 3
[type=vpn search=3]
3. "AWESOMEco" (type="vpn", uuid="guid-guid-guid-guid-guid")
--> Start connection to: "AwesomeCo"? [Y/n]
```
