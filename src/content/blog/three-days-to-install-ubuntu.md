---
title: "It Took Me Three Days to Install Ubuntu. Ubuntu Wasn't the Problem."
description: "I thought this would take half an hour. Then the USB stick died, the download crawled, and Windows locked the replacement drive."
pubDate: 2026-09-21
canonical: "https://rohanroots.substack.com"
image: "/blog/ubuntu-cover.png"
---

![Ubuntu running on the Lenovo Yoga 720](/blog/ubuntu-cover.png)

For months, my old Lenovo Yoga 720 had been doing what old laptops do best: lying around. Its battery indicator was always stuck at 0 percent, so I did not think much about it. I had quietly accepted that its main job was now collecting dust.

Then my X timeline filled up with people talking about Linux desktops. I remembered the laptop, looked at its aging Windows-era specs, and thought: this could still make a perfectly useful Linux machine.

So, on Saturday afternoon, I gave myself half an hour to install Ubuntu. A few hours later, I caught myself asking whether this was really the best use of my Saturday.

The answer was probably no. But by then I wanted to know what was actually wrong. I had several possible causes and still had not reached the Ubuntu installer.

Across three calendar days, with two days of actual troubleshooting, I ran into three separate problems: a dying USB drive, a slow mirror, and a Windows lock on the replacement drive. All three left me at the same place: still no installer.

I expected the install to be the easy part. It was. Getting trustworthy boot media onto a USB drive consumed nearly all the time.

The problems looked related because they all stopped me at the same place. They were not. I had to check the USB stick, the download, and Windows separately instead of blaming the whole Ubuntu setup.

## The failure chain, in order

## Eight failed attempts turned the first USB stick into the suspect

The original 64 GB USB stick did not fail cleanly. It sent me through eight different attempts first. A standard Rufus flash ended with a vmlinuz corruption error. DD mode made the drive disappear from the F12 boot menu, while Windows Advanced Startup simply looped back to the desktop.

Then I tried Ventoy. Running it from inside the compressed ZIP failed, so I extracted it and tried again with GPT selected. Ventoy answered with the impressively unhelpful message: 'An error occurred during the installation.'

I used DiskPart to clean the old drive, created a FAT32 partition, and tried Ventoy again. Same error. Windows' normal format tool then said it could not complete the format. On the next force-wipe attempt, DiskPart cleaned the drive but failed during formatting with 'A device which does not exist was specified.' The USB stick had temporarily disconnected from Windows.

I reinserted it and gave Ventoy one last chance. The drive mounted as an empty volume, but Ventoy failed again. It was never successfully installed on that stick.

That final disconnect mattered more than the earlier error messages. A partitioning problem can make a drive look unusable, but hardware that vanishes while it is being written is a different kind of warning. By attempt eight, I was no longer troubleshooting. I was negotiating with a tiny piece of plastic.

I stopped trying to rescue it and ordered a 64 GB SanDisk Ultra Flair from Amazon. It arrived the next day. At that point, Amazon's delivery pipeline was moving faster than my Ubuntu installation. Replacing the stick was the right decision, but it did not immediately solve the installation. It only removed the first fault.

## The 35-minute download was not my Wi-Fi

The next delay looked like a network problem. The official Ubuntu download estimated roughly 35 minutes for the desktop image. That did not fit the connection I was using.

A speed test on the same Wi-Fi showed about 264 Mbps. The laptop's connection was not the bottleneck. The server path was.

I switched to the Massachusetts Institute of Technology mirror and downloaded the Ubuntu 26.04.1 desktop ISO in about three minutes. Same laptop. Same network. Different source. Thirty-two minutes disappeared without a single router reboot, which is usually a good clue that the router was innocent.

## The new USB drive failed for a completely different reason

With a healthy replacement drive and a freshly downloaded ISO, Rufus should have been routine. I selected the image, used GPT for a Unified Extensible Firmware Interface (UEFI) system, and started the write.

Rufus reported that it could not access the drive because another process was using it. Closing File Explorer, unplugging and reconnecting the stick, and running Rufus as administrator did not clear the error.

This was the moment when it would have been easy to assume the new USB stick was also bad and start pricing a third one. The symptom looked familiar, but the evidence did not. The drive stayed connected and Rufus could identify it. This was a lock or partition-state problem inside Windows, not failing flash memory.

## A normal Windows format unlocked Rufus

I did not run DiskPart clean on the new USB drive. I opened This PC, right-clicked the drive, and formatted it through the normal Windows menu.

After that manual format, Rufus started writing normally. Windows briefly reported that the USB drive was not accessible while Rufus repartitioned it. In this context, that popup was expected. Rufus was actively replacing the filesystem Windows had just mounted.

The progress bar reached the green READY state. The replacement drive had never been broken. It had only needed a regular format before Rufus could use it.

## Once the boot media worked, Ubuntu installed without drama

I selected the USB drive from the Lenovo Yoga 720 boot menu. Ubuntu loaded into the live environment, and the installer started.

This laptop was being repurposed, not set up for dual boot, so I erased the internal disk and gave Ubuntu the whole machine. The installation completed, the laptop restarted, and Ubuntu 26.04.1 LTS booted from the internal drive.

![Ubuntu 26.04.1 LTS welcome screen on the Lenovo Yoga 720](/blog/ubuntu-inline.png)

Ubuntu 26.04.1 LTS, finally running on the Lenovo Yoga 720. The first welcome came with 37 updates waiting.

After all that, the first notification said '37 Updates Available.' Three days to install the operating system, and it welcomed me by giving me more work. Fair enough.

## The useful lesson was to troubleshoot the layer, not the symptom

The repeated outcome was "Ubuntu is still not installed," but the causes belonged to different layers:

Hardware: The old USB stick disconnected during writes.

Network path: The selected mirror was slow even though the local connection was healthy.

Operating-system state: Windows held the replacement drive in a state Rufus could not use.

Boot configuration: The finished USB needed to be selected from the UEFI boot menu.

What helped was checking the evidence again after every change. The old USB stick was bad. The new one was not. It had run into a separate Windows problem.

I started on Saturday afternoon thinking Ubuntu would be installed in half an hour. Instead, I spent two days finding three unrelated problems before the installer even started. The actual installation was the shortest and least interesting part.

Ubuntu had barely done anything wrong, which was slightly inconvenient after I had spent hours blaming the Ubuntu installation. The forgotten laptop now has a fresh Linux desktop, Windows is gone, and that questionable Saturday decision finally has something to show for it.

A 30-second hold of the laptop's power button brought the battery indicator, which had been stuck showing 0 percent, back to life. The percentage moves again, but the battery still drains faster than it should. Apparently the Yoga has already chosen my next project: replace the battery.
