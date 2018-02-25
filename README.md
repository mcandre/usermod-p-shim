# usurp: a replacement part for missing -p functionality in usermod

# ABOUT

usurp emits an augmented copy of a UNIX shadow file, with the configured username's entry modified to feature the desired (hashed) password. This shim is useful for automatically updating passwords in environments where a system's `usermod` command omits the typical `-p` flag to perform this operation.

For manual password changes, `passwd` can safely edit the `/etc/shadow` file. However, this typically requires the password to be entered via an interactive tty session, requiring either a human operator or surgical application of the `expect` utility. This shim provides a more automated workflow, intended for system administrators, Continuous Integration servers, virtual machine configuration, and so on.

# EXAMPLE

```console
$ sudo cp /etc/shadow /etc/shadow.bak

$ USERNAME=root PASSWORD='$1$zNXmpWZT$OjY/uoVZo1bsnld1/k5za1' sudo usurp < /etc/shadow > /etc/shadow.new

$ diff /etc/shadow /etc/shadow.new
1c1
< root:$1$TDQFedzX$.kv51AjM.FInu0lrH1dY30:15045:0:99999:7:::
---
> root:$1$zNXmpWZT$OjY/uoVZo1bsnld1/k5za1:15045:0:99999:7:::

$ sudo cp /etc/shadow.new /etc/shadow
```

# REQUIREMENTS

* awk (either GNU gawk or legacy nawk)

## Recommended

* `openssl passwd`/`mkpasswd`/`cryptpass`/etc.
* diff

# INSTALL

1. Clone https://github.com/mcandre/usurp.git
2. Add the .../lib directory to your shell's `$PATH`.