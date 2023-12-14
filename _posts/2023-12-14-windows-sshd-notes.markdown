---
layout: post
title: "Windows sshd notes"
date: 2023-12-14 05:14:27 -0800
github_comments_issueid: "24"
published: true
tags:
---

Yesterday I tried (but failed) to get sshd working on my windows machine.   Here are some notes to self in case I ever go this route again.
I've been using [choco](https://chocolatey.org/install) to install windows software when possible.

    $ choco install openssh
    Chocolatey v2.2.2
    3 validations performed. 2 success(es), 1 warning(s), and 0 error(s).

    Validation Warnings:
     - System Cache directory is not locked down to administrators.
       Remove the directory 'C:\ProgramData\ChocolateyHttpCache' to have
       Chocolatey CLI create it with the proper permissions.

    Installing the following packages:
    openssh
    By installing, you accept licenses for the packages.
    Progress: Downloading openssh 8.0.0.1... 100%

    openssh v8.0.0.1 [Approved]
    openssh package files install completed. Performing other installation steps.
    The package openssh wants to run 'chocolateyinstall.ps1'.
    Note: If you don't run this script, the installation will fail.
    Note: To confirm automatically next time, use '-y' or consider:
    choco feature enable -n allowGlobalConfirmation
    Do you want to run the script?([Y]es/[A]ll - yes to all/[N]o/[P]rint): Y

    Running on: Windows 10 Pro, (Professional)
    Windows Version: 10.0.19045

    ************************************************************************************
    ************************************************************************************
    This package is a Universal Installer and can ALSO install Win32-OpenSSH on
    Nano, Server Core, Docker Containers and more WITHOUT using Chocolatey.

    .... (more output)


Now, per [OpenSSH instructions](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH) , open port 22 , install openssh as a service, and start the service

    dc202@DESKTOP-MAR8MAD /cygdrive/c
    $ cd 'c:/Program Files/OpenSSH-Win64'

    dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files/OpenSSH-Win64
    $ netsh advfirewall firewall add rule name=sshd dir=in action=allow protocol=TCP localport=22
    Ok.


    dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files/OpenSSH-Win64
    $ powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps1
    [SC] SetServiceObjectSecurity SUCCESS
    [SC] ChangeServiceConfig2 SUCCESS
    [SC] ChangeServiceConfig2 SUCCESS
    sshd and ssh-agent services successfully installed

    dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files/OpenSSH-Win64
    $ net start sshd
    The OpenSSH SSH Server service is starting.
    The OpenSSH SSH Server service was started successfully.


    dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files/OpenSSH-Win64
    $

That seems to be *almost* enough but when I try to log in remotely, ssh rejects my password:

    dave@vbox:/tmp$ ssh -l dc202 192.168.8.146
    dc202@192.168.8.146's password:
    Permission denied, please try again.
    dc202@192.168.8.146's password:

At this point I searched online a little for a solution but eventually gave up (for now).

I restored the machine to its previous state.   Thanks to [this superuser.com answer from "Vomit IT - Chunky Mess Style"](https://superuser.com/a/1237985) for telling how to undo the firewall changes.

	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files
	$ net stop sshd

	The OpenSSH SSH Server service was stopped successfully.


	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files
	$ netsh advfirewall firewall show rule status=enabled name=all | grep sshd
	Rule Name:                            sshd

	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files
	$ netsh advfirewall firewall delete rule name="sshd"

	Deleted 1 rule(s).
	Ok.


	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files
	$ cd 'c:/Program Files/OpenSSH-Win64'/

	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files/OpenSSH-Win64
	$ powershell.exe -ExecutionPolicy Bypass -File uninstall-sshd.ps1
	sshd successfully uninstalled
	ssh-agent successfully uninstalled

	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files/OpenSSH-Win64
	$ cd ..

	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files
	$ choco uninstall openssh
	Chocolatey v2.2.2
	3 validations performed. 2 success(es), 1 warning(s), and 0 error(s).

	dc202@DESKTOP-MAR8MAD /cygdrive/c/Program Files
	$ rm -rf OpenSSH-Win64/


If anyone reads this and happens to know what the missing link is to get logins working, please let me know.




