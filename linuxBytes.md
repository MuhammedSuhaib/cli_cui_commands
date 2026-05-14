# Basic File System Commands: Linux vs CMD vs PowerShell

## listing

Linux: `ls`  <br>

- `ls -al` shows all files in a directory with full details.

      Use cases:
      
      See hidden files (those starting with .)
      
      Check permissions, owners, sizes, timestamps
      
      Inspect directories in depth for debugging, config, deployment

CMD: `dir`  <br>
- dir /a /s > sizes.txt    
      This creates sizes.txt in the same folder with the full output (including hidden dirs).
PowerShell: `ls` / `dir` / `gci`  <br>

## show file content

Linux: `cat`  <br>
CMD: `type`  <br>
PowerShell: `cat` / `gc`  <br>

## create folder

Linux: `mkdir`  <br>
CMD: `md` / `mkdir`  <br>
PowerShell: `md` / `mkdir`  <br>

## delete file

Linux: `rm`  <br>
CMD: `del`  <br>
PowerShell: `rm`  <br>

## move/rename

Linux: `mv`  <br>
CMD: `move`  <br>
PowerShell: `mv` / `move`  <br>

## copy

Linux: `cp`  <br>
CMD: `copy`  <br>
PowerShell: `cp` / `copy` <br>

## remove folder

Linux: `rmdir`  <br>
CMD: `rd` / `rmdir`  <br>
PowerShell: `rmdir` / `rm -r`  <br>

## current directory

Linux: `pwd`   <br>
CMD: none (use `cd` alone)  <br>
PowerShell: `pwd`  <br>

## change directory

Linux: `cd` <br>
CMD: `cd`  <br>
PowerShell: `cd` <br>

## create empty file

Linux: `touch`   <br>
CMD: none (use `type nul > file`) <br>
PowerShell: `ni file`   <br>

<br>
<br>
<br>
<br>
<br>

# Checking Manually Installed Packages on Debian Bookworm (ChromeOS Linux Container)

Command to check manually installed packages on Debian Bookworm (ChromeOS Linux container):

```
comm -23 <(apt-mark showmanual | sort) <(gzip -dc /var/log/installer/manifest.gz | awk '{print $1}' | sort)
```

> Nothing useless on my system. Everything in that list is core Debian stuff except the few tools I added myself on purpose.   

1. **Neofetch:**    
To quickly log system info, run: `neofetch`

2. **Bettercap:**   
Installed this while following a tutorial. I’ll test it in the future. 
It’s for network/security testing (MITM, Wi-Fi, packets, etc).

# Quick APK Sideload Guide

1. **Move the APK** to the **Linux files** folder.
2. **Open Terminal** and connect:
`adb connect arc`
3. **Install the app**:
`adb -s arc install 'filename.apk'`

---

### Pro Tips

* **Update Apps:** To update an existing app without uninstalling, use:
`adb -s arc install -r 'filename.apk'`
* **Clean Up:** If you get a "device offline" error, run `adb kill-server` then start over from step 2.
