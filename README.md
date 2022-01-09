# Ubuntu for CTF PWNer

Ubuntu images for CTF PWNer, easy to use.

```bash
$ docker push assassinq/pwn-machine:$TAGNAME
$ docker run -it --rm --privileged -e DISPLAY="$IP:0" -v "$PWD:/pwn" assassinq/pwn-machine:$TAGNAME /bin/zsh
```

