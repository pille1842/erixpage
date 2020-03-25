---
title: Get the driver to work for a Radeon HD 7600G on Ubuntu 16.04
location: Rastatt
---
Yesterday I put an SSD drive into my 2013 laptop, an Acer Aspire V5-551 which originally came with Windows 8. I decided on Ubuntu Mate 16.04 for the new operating system (before that, I had been running Windows 10 with quite some issues -- e.g. the internal wifi chip did not work, I had to use an external dongle).

Expecting the best, after I finished the installation, I sat on the couch with the laptop detached from power. Then the issues started. When logged into the graphical environment, the machine would randomly hang and freeze after a few seconds. Not even changing to a console worked after such a freeze, I had to do a hard reset each time.

Initially I thought this was a problem with TLP, the power-saving tool for laptops, since I have had problems with TLP on this machine before. Disabling TLP did not improve the situation, however. After enabling persistent logging, I was able to hunt down the bad guy: the open-source Radeon GPU driver. It would spit out the following message again and again after a freeze:

```
radeon SOMENUMBER: ring 0 stalled for more than SOMENUMBER seconds
```

"Fine", I thought, "let's go with the proprietary driver then." As it turns out, there is no proprietary driver from AMD that works with a Radeon HD 7600G chipset on Ubuntu 16.04. The ```fglrx``` driver has been discontinued and in the list of supported cards for its successor, AMDGPU, the 7600G doesn't appear.

So I looked for another solution to get the Radeon driver to work. Some people suggested manually enabling Dynamic Power Management by adding ```radeon.dpm=1``` to the kernel boot options. I tried that and it didn't work. So I thought: "Maybe the other way around will do" and added ```radeon.dpm=0``` to the options instead.

**And it worked.** Now this might cost me some battery lifetime, but it's better for the laptop to be usable a few hours than only a few seconds when it's not connected to a power outlet.

If you encounter a similar problem on a similar machine (after all, this might apply to other Radeon HD chips and other GNU/Linux distributions), try the following:

1. Open ```/etc/default/grub``` in an editor by typing ```sudoedit /etc/default/grub``` in your terminal.
2. In the line that says ```GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"``` or something similar, add ```radeon.dpm=0``` after the "quiet splash" part (separated by a space).
3. Update GRUB's configuration by running ```sudo update-grub``` from your terminal.
4. Reboot and see if the problem is fixed.

I really hope I could help some of you out there stuck with this graphics card.
