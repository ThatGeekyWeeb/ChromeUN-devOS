# ChromeUN-devOS
A guide to DEVOS (DEV MODE/dev_install) on Older *Unsupported* Chromebooks!

## Unsupported?
>
With the release of ChromeOS 77.X many devices recived the *End Of Support* Notice!

![EndSupport](https://raw.githubusercontent.com/ssfgames13/ChromeUN-devOS/master/Screenshot%202020-04-29%20at%205.43.26%20PM.png)

### Broken Update?
>
Devices labeled as *unsupported* we're meant to recive ChromeOS 76.X as there final Update, but most didn't!\
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
    * [ctrl + d]
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
