# vt-terminal

The terminal through the lens of variation theory: misconceptions and
critical aspects of the Unix shell — command model, file system, composition
(pipes, redirection, scripts), and SSH.

Companion papers: [vt-prog-misconceptions] (introductory programming, same
method) and [vt-debug] (debugging). Grew out of the [introtools] course
revision.

[vt-prog-misconceptions]: https://github.com/dbosk/vt-prog-misconceptions
[vt-debug]: https://github.com/dbosk/vt-debug
[introtools]: https://github.com/dbosk/introtools

## Build

```sh
git submodule update --init --recursive
make            # article.pdf + slides.pdf
```

Requires TeX with minted (`-shell-escape`), pythontex, and biber.
