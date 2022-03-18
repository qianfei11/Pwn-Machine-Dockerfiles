# Ubuntu for CTF PWNer

Ubuntu images for CTF PWNer, easy to use.

```bash
$ docker pull assassinq/pwn-machine:$TAGNAME
$ docker run -it --rm --privileged -e DISPLAY="$IP:0" -v "$PWD:/pwn" assassinq/pwn-machine:$TAGNAME /bin/zsh
```

Using `latest` tag to get image with [gef](https://github.com/hugsy/gef) and [pwninit](https://github.com/io12/pwninit) plugin.

