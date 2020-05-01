# ChromeUN-devOS
A guide to DEVOS (DEV MODE/dev_install) on Older *Unsupported* Chromebooks!

## Unsupported?
>
With the release of ChromeOS 77.X many devices recived the *End Of Support* Notice!

![EndSupport](https://raw.githubusercontent.com/ssfgames13/ChromeUN-devOS/master/Screenshot%202020-04-29%20at%205.43.26%20PM.png)

### Broken Update?
>
Devices labeled as *unsupported* we're meant to recive ChromeOS 77.X as there final Update, but most didn't!\
While devices would be *prompted*  to Update, the *Check For Update* button was removed, when devices we're labeled as *Unsupported*

>![Updatepromt](https://raw.githubusercontent.com/ssfgames13/ChromeUN-devOS/master/Screenshot%202020-04-29%20at%205.55.12%20PM.png)

>> **Fixing?**

This update bug was never patched, but a work around is simple!

>> **DevMode!**

By default the update process, runs with the lowest priority on the system, ***Unless*** the *Check For Update* button is pressed!\
\
Using DevMode, we can override the update process and force it to become the operation with the highest priorty!

## DevMode

1. DevMode
     * [esc + F3 (refresh) + power]
     
    ![DevMode](https://github.com/ssfgames13/ChromeUN-devOS/blob/master/68747470733a2f2f626565626f6d2e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031392f31322f5475726e2d4f6e2d4368726f6d65626f6f6.jpeg?raw=true)
     * [ctrl + d]
     
     ![DevModCom](https://github.com/ssfgames13/ChromeUN-devOS/blob/master/68747470733a2f2f7777772e7365727665746865686f6d652e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f476f6f676c652d4.jpeg?raw=true)
     * [enter]
     * [ctrl + d]
> 1a. Transition
   >  * The Transition to developer mode can somtimes freeze! (Spin Wheel isn't animated = Frozen)
   >  * Simpy reboot the device ([F3/Refesh + Power])
   >  * Start Again!

2. VT-2
    * [ctrl + alt + F2/→]
    * `root` (no password required)
        * `initctl stop update-engine`
        * `update_engine -foreground -logtostderr & while true; do update_engine_client -check_for_update; update_engine_client -update; pause 5; done`
  > *update can take 5-30mins (Differs based on Internet Speed)*
  > *Wait for update to finsh downloading & installing*
  
  * `sudo reboot` 
  
  > *Press "[ctrl + d]" to enter Developer Mode!*
  
### rw_rootfs
  
3. RW rootfs
      * [ctrl + alt + t]
      * `shell`
      * `sudo su -`
        * `cd /usr/share/vboot/bin/`
        * `./make_dev_ssd.sh --remove_rootfs_verification --partitions 4`
        * `./make_dev_ssd.sh --remove_rootfs_verification --partitions 2`
        * `sudo reboot`
      * [ctrl + d]
      > Must be done each reset!

### remount

4. Remount
    * [ctrl + alt + t]
    * `shell`
    * `sudo su -`
        * `mount -o remount,rw /`
        * `mount -o remount,exec /mnt/stateful_partition`
    > Must be done each reboot!

### dev_install 

1. dev_install
      * [ctrl + alt + t]
      * `shell`
      * `sudo su -`
        * `curl -LO https://raw.githubusercontent.com/ssfgames13/ChromeUN-devOS/master/make.defaults`
        * `curl -LO https://raw.githubusercontent.com/ssfgames13/ChromeUN-devOS/master/dev_install`
        * `ln -s /usr/local /home/.shadow/<encryptkey>/vault/`
        * `curl -LO https://raw.githubusercontent.com/ssfgames13/ChromeUN-devOS/master/python-exec2-c`
        * `mkdir /usr/lib/python-exec`
        * `cp ./python-exec2-c /usr/lib/python-exec`
        * `cp ./make.defaults /etc/portage/make.profile/`
        * `cp ./dev_install /usr/bin/dev_install`
        * `dev_install`
        * `rm /home/.shadow/<encryptkey>/vault/local/portage/packages/* -r`
        * `rm /usr/lib/python-exec/ -rf`
        * `cp /home/.shadow/<encryptkey>/vault/local/lib/python-exec/ /usr/lib/python-exec/ -r`
        * `ln -s /home/.shadow/<encryptkey>/vault/local/bin/python2.7 /usr/bin/python2.7`
        * `ln -s /home/.shadow/<encryptkey>/vault/local/bmkdir /usr/local/share2in/python2.7 /usr/bin/python2`
        * `ln -s /home/.shadow/<encryptkey>/vault/local/bin/python2.7 /usr/bin/python`
        * `dev_install --reinstall --yes`
        * `emerge nano`
        * `emerge wget`
        * `cp /usr/share/* -rf /usr/local/share/`
        * `mkdir /usr/local/share2`
        * `mv /usr/local/share/* /usr/local/share2`
        * `rm /usr/local/share -rf`
        * `ln -s /usr/share/ /usr/local/`
        * `emerge @world`
        * `emerge -a virtual/target-os` [yes]
        * `emerge --root="$ROOT_FS_DIR" --root-deps=rdeps --usepkgonly virtual/target-os`
        * `emerge busybox`
        * `rm -rf /usr/local/portage/packages/*`
        * `rm -rf /usr/local/var/tmp/portage/*`
        * `rm /tmp/* -rf`
        > emerge is now installed!\
        > Haven't used Gentoo/Emerge before?\
        > Visit [Gentoo Wiki](https://wiki.gentoo.org/wiki/Portage#emerge)
        
### Python

**[Python-exec bug](https://bugs.chromium.org/p/chromium/issues/detail?id=842039)**

## To_Do:

- [x] Test Custom make.defaults > [Test Results](https://github.com/ssfgames13/ChromeUN-devOS/issues/1)
- [ ] Create /usr/lib/* tarball for easy fixes! [Bug 1x2](https://github.com/ssfgames13/ChromeUN-devOS/issues/2)
- [x] Add python-exec link
- [x] Create /usr/share/* tarball > [Bug 2x1](https://github.com/ssfgames13/ChromeUN-devOS/issues/3)
