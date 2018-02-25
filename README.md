# usermod-p-shim: a replacement part for missing -p functionality in usermod

# ABOUT

usermod-p.awk emits an augmented copy of a UNIX shadow file, with the configured username's entry modified to feature the desired (hashed) password. This shim is useful for automatically updating passwords in environments where a system's `usermod` command omits the typical `-p` flag to perform this operation.

# EXAMPLE

```console
$ sudo cp /etc/shadow /etc/shadow.bak

$ USERNAME=root PASSWORD='$1$zNXmpWZT$OjY/uoVZo1bsnld1/k5za1' sudo awk -f usermod-p.awk < /etc/shadow > /etc/shadow.new

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