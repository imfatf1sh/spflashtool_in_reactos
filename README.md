This repository tries to collect information about how to get SP Flash Tool working in ReactOS (free Windows alternative), so one can flash MTK devices inside a VM on Linux without getting annoying errors.

The purpose of using ReactOS instead of a full Windows VM is it's much less bloated.

Quickemu, a handy shell wrapper of QEMU is used. (This needs to be installed)

Q: Why not use MTKClient? A: `mtkclient` doesn't support scatter file as time of writing.

It's probably a better idea to setup an Ubuntu 14.04 VM. (Officially supported by SP Flash Tool, I guess?)


## Current state

USB serial driver is missing in ReactOS.

---

* NetKVM and USB redirection needs the following patches in `quickemu`:

```diff
@@ -1083,8 +1083,10 @@ function configure_os_quirks() {
                  NET_DEVICE="pcnet";;
         *solaris) usb_controller="xhci"
                   sound_card="ac97";;
-        reactos) NET_DEVICE="e1000"
-                 keyboard="ps2";;
+        reactos) NET_DEVICE="virtio-net-pci"
+                 keyboard="ps2"
+                 usb_controller="ehci"
+                 USB_HOST_PASSTHROUGH_CONTROLLER="usb-ehci";;
         macos)
             # Tune QEMU optimisations based on the macOS release, or fallback to lowest
             # common supported options if none is specified.
```

* QXL GPU driver can be installed from virtio-win 0.1.130.

* CJK fonts<sup>rapps</sup> may needs to be installed for Japanese texts to display correctly. ([Credits](https://katahiromz.fc2.page/reactos-mojibake/))

* VC++ runtime 2008<sup>rapps</sup> needs to be installed for SP Flash Tool to launch.

* MSVCRT dlls shipped with SP Flash Tool needs to be deleted to avoid error.

* BTRFS can be used for boot partition, ignore EA (extended attributes) errors when you extract files using 7-zip.

* Snapshot often, just in case you break installation.

* If needed, use MTK-bypass or MTKClient on your host before connecting device to VM.
