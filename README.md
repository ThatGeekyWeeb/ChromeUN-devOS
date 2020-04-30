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
  
3. RW rootfs
      * [ctrl + alt + t]
      * `shell`
      * `sudo su -`
        * `cd /usr/share/vboot/bin/`
        * `./make_dev_ssd.sh --remove_rootfs_verification --partitions 4`
        * `./make_dev_ssd.sh --remove_rootfs_verification --partitions 2`
        * `sudo reboot`
      * [ctrl + d]

4. Remount
    * [ctrl + alt + t]
    * `shell`
    * `sudo su -`
        * `mount -o remount,rw /`
        * `mount -o remount,exec /mnt/stateful_partition`

### dev_install 

1. dev_install
      * [ctrl + alt + t]
      * `shell`
      * `sudo su -`
        * `cd /etc/portage/make.profile`
        * `curl -LO https://github.com/ssfgames13/ChromeUN-devOS/blob/master/make.default`
        * <dev_install>
        * `dev_install`
        * `rm /home/.shadow/c63d9ab87aa339209c9ed060de104e55ade6705c/vault/portage/packages/* -r`
        * `rm /usr/lib/python-exec/ -rf`
        * `cp /home/.shadow/c63d9ab87aa339209c9ed060de104e55ade6705c/vault/lib/python-exec/ /usr/lib/python-exec/ -r`
        * `ln -s /home/.shadow/<encryptkey>/vault/Downloads/bin/python2.7 /usr/bin/python2.7`
        * `ln -s /home/.shadow/<encryptkey>/vault/Downloads/bin/python2.7 /usr/bin/python2`
        * `ln -s /home/.shadow/<encryptkey>/vault/Downloads/bin/python2.7 /usr/bin/python`
        * `dev_install --reinstall --yes`
        * `emerge nano`
        * `emerge wget`
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
        
# Issues

###### [Bug 1X2](https://github.com/ssfgames13/ChromeUN-devOS/issues/20):
***`rm /usr/lib/*`*** ***`May break sudo, Issue results in broken system!`***

# Testing
### Custom make.defaults

Patching the orginal make.defaults (/etc/portage/make.profile/make.defaults)\
With my custom built one, should make portage use *drivefs* (GoogleDrive mount)\
This should fix [Bug 1x1](https://github.com/ssfgames13/ChromeUN-devOS/issues/1)
> Additionally, if the users device has more than 15GB, make.defaults can be edited to use the *homedir* (/home/user/'*') *: /home/user/'encryptitionkey'
    
1. Patch
    * curl -LO https://raw.githubusercontent.com/ssfgames13/ChromeUN-devOS/master/make.defaults
    * cp ./make.defaults /etc/portage/make.profile/
    > ***Patching must be done BEFORE [`dev_install`](https://github.com/ssfgames13/ChromeUN-devOS/blob/master/README.md#dev_install)is ran***

### Python

**[Python-exec bug](https://bugs.chromium.org/p/chromium/issues/detail?id=842039)**

- [x] Test Custom make.defaults > [Test Results](https://github.com/ssfgames13/ChromeUN-devOS/issues/1)
- [ ] Create /usr/lib/ tarball for easy fixes!
- [x] Add python-exec link
