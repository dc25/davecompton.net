---
layout: post
title: "Rust + GKT4 on Windows: Getting Started"
date: 2022-04-10 06:23:40 -0700
published: true
github_comments_issueid: 16
tags:
---

This post is mostly the same information as on [this medium post](https://doko-demo-doa.medium.com/rust-gtk-on-windows-getting-started-14aa2d7c825d), by someone named Doko, which I will refer to in this post as "the Doko page".

Doko, if you read this, thank you for the instructions, they were very helpful.  

I'm creating *this* post to preserve what I learned from the Doko page and to offer a few comments on my experience with the process outlined there and to attest that it worked.

#  Install Rust

I did this a while back on windows and I don't remember doing anything special.  Instructions [here](https://www.rust-lang.org/tools/install).

Check rust version with:
```
rustc --version
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

# Install MSYS2 
Use the [provided installer](https://www.msys2.org/)

# Using a cmd shell, set the following environment variables:

```
SET GTK_LIB_DIR=C:\msys64\mingw64\lib
SET PATH=%PATH%;C:\msys64\mingw64\bin
SETX GTK_LIB_DIR %GTK_LIB_DIR%
SETX PATH %PATH%
```

I was following the Doko page as closely as possible so I did this but I'm not sure if it was really necessary.  If I go through this process again, I'll try setting them in a shell startup script.


#  Using a MSYS Shell, run the following commands;
```
pacman -S mingw-w64-x86_64-gtk4
pacman -S mingw-w64-x86_64-toolchain
```

The Doko page specifies gtk3 which I tried first. It worked but I wanted to use gtk4 and that worked as well.

#   Compile and run gtk4 programs.
Examples are available as part of the [gtk4-rs repository](https://github.com/gtk-rs/gtk4-rs).
They work both on Windows and on Linux without modification.

