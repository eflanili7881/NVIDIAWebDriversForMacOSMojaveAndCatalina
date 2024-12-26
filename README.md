# NVIDIA Web Drivers for macOS Mojave and Catalina
Required files and instructions for installing NVIDIA Web Drivers on macOS Mojave and Catalina

## Credits
- [Khronokernel](https://github.com/Khronokernel)
  - Main co-author of OCLP software.
- [DhinakG](https://github.com/DhinakG)
  - Main co-author of OCLP software.
- [ASentientBot](https://github.com/ASentientBot) for Mojave, Catalina Graphics Acceleration Patches.
- [dosdude1](https://github.com/dosdude1) for Mojave and Catalina Graphics Acceleration Patches.
- [EduCovas](https://github.com/covasedu) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for NVIDIA Tesla/Fermi/Maxwell/Pascal.
- [ASentientHedgehog](https://github.com/moosethegoose2213) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for NVIDIA Tesla/Fermi/Maxwell/Pascal.
- [ASentientBot](https://github.com/ASentientBot) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for NVIDIA Tesla/Fermi/Maxwell/Pascal.
- [flagers](https://github.com/flagersgit) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for nVidia Tesla/Fermi/Maxwell/Pascal.
- [Jazzzny](https://github.com/Jazzzny) for NVIDIA OpenCL research and development.
- [acidanthera](https://github.com/acidanthera) for WhateverGreen Lilu plugin and Lilu itself.
- [Apple](https://www.apple.com) for macOS and many of the kexts, frameworks and other binaries.
I'm sorry if I forgot to mention other contributors on OCLP and PatcherSupportPkg about NVIDIA Web Drivers.
- [NVIDIA](https://www.nvidia.com) for creating NVIDIA Web Drivers.

## Story of creation of this repo
I used NVIDIA GPU's (GT 630 and GTX 1050 Ti) with Sierra and High Sierra during Mojave, Catalina, Big Sur and Monterey eras. NGL, I felt a bit jealous while AMD and Intel iGPU users using most up-to-date OS'. But legends ([Khronokernel](https://github.com/Khronokernel), [DhinakG](https://github.com/DhinakG) and all other legends) made NVIDIA Web Drivers able to run on newer OS' but only on Big Sur and above with OCLP 0.4.6. I viewed [this](https://www.reddit.com/r/hackintosh/comments/uxz95u/nvidia_web_drivers_running_on_macos_monterey/) Reddit post and [this](https://x.com/khronokernel/status/1529583832663437312) Twitter/X tweet/post

But when I checked OCLP pull [#993](https://github.com/dortania/OpenCore-Legacy-Patcher/pull/993), I saw this:

![image](https://github.com/user-attachments/assets/d5f55737-01fc-43dd-9d8f-f88363c20de0)

Do you see what I see? There's 10.14.3 which stands for macOS Mojave build number. A Maxwell-based NVIDIA GPU ran on macOS Mojave 10.14.3 which is tested by legend [Khronokernel](https://github.com/Khronokernel) itself.

After that, I started investigating this. I tried installing NVIDIA Web Drivers before on Catalina test system but due to I unpacked some *.framework files on Windows, I borked that Catalina test install. But on December 18, I tried this again, but unpacking all files on macOS itself instead, and guess what? I patched OS' finally to run NVIDIA Web Drivers with non-Metal mode just like on Big Sur and above on Mojave and Catalina!

macOS Mojave 10.14.6 Security Update 2021-005 build 18G9323 
![IMG_20241218_224255](https://github.com/user-attachments/assets/226b895f-7319-4bd2-9b8e-8e9c8a6ca89c)

macOS Catalina 10.15.7 Security Update 2022-005 build 19H2026
![IMG_20241218_224618](https://github.com/user-attachments/assets/239bd4c3-cd9f-41a6-97a1-3fd2d7f0b99f)

## Instructions
- Disable Library Validation via:
  - amfi_get_out_of_my_way=0x1 boot argument or
  - [_cs_require_lv patch](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/67a78ec4a275e0e8f8941a5ff9d64f00c28397dc/payloads/Config/config.plist#L1352-L1381) kernel patch
- Enable Forced Compatibility via WhateverGreen via:
  - ngfxcompat=1 boot argument or
  - force-compat with Data type that has value 01000000 on your GPU's device properties.
- Force OpenGL rendering via:
  - ngfxgl=1 boot argument or
  - disable-metal with Data type that has value 01000000 on your GPU's device properties.
- Enable NVIDIA Web Drivers via:
  - nvda_drv_vrl=1 boot argument
- Set your SIP to 0xa03.
  - CSR_ALLOW_UNTRUSTED_KEXTS
  - CSR_ALLOW_UNRESTRICTED_FS
  - CSR_ALLOW_UNAPPROVED_KEXTS
  - CSR_ALLOW_UNAUTHENTICATED_ROOT
- Install Lilu from [here](https://github.com/acidanthera/Lilu).
- Install WhateverGreen from [here](https://github.com/acidanthera/WhateverGreen).
- Download this repo contents via **Code>Download ZIP**.
- Run commands thats inside on **commands.txt**.
- Remove necessary files thats inside on **files_to_delete.txt**.
  - If files missing, don't worry. Just delete files that just exists.
  - On Catalina, run **sudo mount -uw /** to mount root filesystem as read-write.
- Copy all files from **copy_mojave** or **copy_catalina** to root filesystem.
  - On Catalina, run **sudo mount -uw /** to mount root filesystem as read-write.
- After you copied files, run **sudo kextcache -i /** to rebuild kext cache.
- Reboot your system.
