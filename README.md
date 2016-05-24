**WARNING: This is a fork from [arcanericky/ga-cmd](https://github.com/arcanericky/ga-cmd "arcanericky/ga-cmd"). It's a bit cleaner and works only using file (no hardcoded-compiled key). Also, I removed unused files and forked only the necessary google-authenticator files direct to libpam-google-authenticator dir.**

**Likewise, it's limited to 16-char keys so far. I'll try to include longer keys in the future and change a few minor things.**

-------------
###Google Authenticator - Command Line

It's like Google Authenticator, but on a command line using your Linux/OSX box.
The extremely small program (~40k) is built by leveraging the Google Authenticator source itself - specifically, the PAM code. 

####Background
The Google Authentication codes are standardized message authentication codes called HMACs. Read more about them starting at the [Wikipedia page for HMAC](https://www.google.com "Wikipedia: HMAC"). You can find more source code for generating HMACS by searching here on [GitHub for HMAC](https://github.com/search?q=hmac "GitHub: HMAC"). This project was done for the fun of bending Google's PAM code to generate an HMAC.

####Requisites
#####Tested on Mac OSX 10.11, Ubuntu 15.04 64-bit and Openwrt Chaos Calmer 15.05:
- On Ubuntu
```
libpam0g-dev
```

- On OSX
```
(None that I'm aware. Just Homebrew/Macports with default dev-tools)
```

- OpenWRT package building:
```
(...)
DEPENDS:=libpam
(...)
```

####Building
```
$ git clone https://github.com/fallc0nn/gacli.git
```
Once you've done this, switch into the src directory and make:
```
$ cd gacli
$ make all
```
The source will compile, then the gacli is built and deposited at bin/gacli.

####Using
Create $HOME/.gacli with your 16-digit seed and assign the correct permission. Example:
```
$ echo FFFFGGGGHHHHJJJJ > $HOME/.gacli
$ chmod 0600 $HOME/.gacli
```

**Warning: Make sure permission is 0600.**
```
$ bin/gacli
123456
```

If needed, just change the SEED in $HOME/.gacli and re-run:
```
$ bin/gacli
654321
```

You can also use a keyfile as a parameter (Again, use 0600 as permission):
```
$ bin/gacli /etc/gacli.d/google
112233
```

If the executable doesn't emit a correct verification code (check it against your official Google Authenticator app) then you probably entered the wrong key in your keyfile or the time on your computer is off.

####Know issues:
- (#01) A few random seeds generated by google-authenticator doesn't work. Try a simpler key instead.
