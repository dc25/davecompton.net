---
layout: post
title: "Rust + GKT4 on Windows (v2)"
date: 2026-09-13 13:12:59 -0700
published: true
github_comments_issueid: 31
tags:
---

This post is mostly the same information as on [this previous post]({{ '2022/04/10/rust-gkt4-on-windows-getting-started.html' | relative_url }} ) which, in turn, was taken from [this medium post](https://doko-demo-doa.medium.com/rust-gtk-on-windows-getting-started-14aa2d7c825d), by someone named Doko, which I will refer to in this post as "the Doko page".

The goal here is to put together a portable rust/gtk4 development environment on windows.  As of this writing I'm using windows11.

#  Install Choco
These days I'm using the choco package manager on windows.   Installation instructions [here](https://chocolatey.org/install)

#  Install Rust and msys2 (using Choco)

```
$ choco install rustup.install msys2
Chocolatey v2.7.4
Installing the following packages:
rustup.install;msys2
By installing, you accept licenses for the packages.

```


Update rust with
```
rustup update
```

# Add the GNU toolchain

The Doko page suggests doing this even before installing the gnu tools.  I had my doubts but it worked.

```
rustup target add x86_64-pc-windows-gnu
```

Run ...

```
rustup show
```

... to show the current target:

```
stable-x86_64-pc-windows-gnu
stable-x86_64-pc-windows-msvc (default)
```

Run ...

```
rustup default stable-x86_64-pc-windows-gnu
```

... to switch targets and run ...

```
rustup show
```
... again to see that the current target has changed.

#  Using a MSYS Shell, run the following commands;
```
pacman -S mingw-w64-x86_64-gtk4
pacman -S mingw-w64-x86_64-toolchain
```

The second (toolchain) pacman command gives some options.   The necessary ones for this task are: "binutils", "gcc", and "pkgconf".   Or take the default to install them all which takes longer but seems to work.


# Add mingw64/bin to your PATH

The following is from my shell start script.   I'm running bash in cygwin but something similar should work in the windows cmd.  

```
export PATH=\
/cygdrive/c/tools/msys64/mingw64/bin:\
$PATH
```
It seems that this is necessary to give access to an executable, dlltool.exe, as well as some other things that are found based on the location of dlltool.exe.   If you see an error message related to dlltool not being found, check your PATH.

#   Compile and run gtk4 programs.
Examples are available as part of the [gtk4-rs repository](https://github.com/gtk-rs/gtk4-rs).
They work both on Windows and on Linux without modification.

