# curl-tap-sh

So, the internet seems to have a lot of software with the installation method
being in the infamous `curl .. | sh` format. People don't like this because
what `curl` downloads might have been messed with by someone in between
depending on the specifics. But people still use this method because it is
convenient.

Awal is here to present a solution. Included in this repo is a script,
which you can put in your `$PATH` by the name `tap`. And now whenever you
are about to run:

```sh
curl foo/bar | sh
```

Simply run the following instead:

```sh
curl foo/bar | tap | sh
```

`tap` will first collect all the data from curl, save it to a temp file,
open that file in your `$EDITOR` (or `vim` if not specified), and you can
review it. You can make changes to it if you want. If you write the file
and close the editor successfully (i.e., the editor returns exit code 0),
then `tap` sends the saved output (including your edits, if any) along the
pipe. Else it doesn't (so you can exit with `:cq` in vim if you don't want
to run the script after reviewing). This also shields against a timing
attack which [detects `curl | sh` server-side][1].

Ofcourse, `tap` deletes the temporary file after this :)

## Other Stuff

There is also `vipe` from the excellent [moreutils][2] toolkit, written as
a perl script. It does pretty much the same thing.

There is [hashpipe][3], written in Go, which verifies stdin based on a
checksum passed to it. This is a pretty good idea too, but it requires the
distributor of the script to provide an up-to-date checksum at all times,
and you need to be sure that the medium through which you are obtaining the
checksum has not been meddled with.

## Author

Awal Garg <awalgarg@gmail.com>, [@awalGarg](https://twitter.com/awalGarg)

This repo is released under WTFPL.

[1]: https://www.idontplaydarts.com/2016/04/detecting-curl-pipe-bash-server-side/
[2]: https://joeyh.name/code/moreutils/
[3]: https://github.com/jbenet/hashpipe
