# NVIDIA Web Drivers for macOS Mojave and Catalina
Required files and instructions for installing NVIDIA Web Drivers on macOS Mojave and Catalina

All required files taken from [PatcherSupportPkg](https://github.com/dortania/PatcherSupportPkg) repo on 18th December 2024.
You can go there if the patches are updated. Just take files listed on [here](https://github.com/eflanili7881/NVIDIAWebDriversForMacOSMojaveAndCatalina/blob/main/patchlist_for_web_drivers.txt).

## Credits
- [Khronokernel](https://github.com/Khronokernel)
  - Main co-author of OCLP software.
- [DhinakG](https://github.com/DhinakG)
  - Main co-author of OCLP software.
- [ASentientBot](https://github.com/ASentientBot) for Mojave and Catalina Graphics Acceleration Patches.
- [dosdude1](https://github.com/dosdude1) for Mojave and Catalina Graphics Acceleration Patches.
- [EduCovas](https://github.com/covasedu) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for NVIDIA Tesla/Fermi/Maxwell/Pascal.
- [ASentientHedgehog](https://github.com/moosethegoose2213) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for NVIDIA Tesla/Fermi/Maxwell/Pascal.
- [ASentientBot](https://github.com/ASentientBot) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for NVIDIA Tesla/Fermi/Maxwell/Pascal.
- [flagers](https://github.com/flagersgit) for [non-Metal patch set](https://github.com/moraea/non-metal-frameworks) for nVidia Tesla/Fermi/Maxwell/Pascal.
- [Jazzzny](https://github.com/Jazzzny) for NVIDIA OpenCL research and development.
- [acidanthera](https://github.com/acidanthera) for WhateverGreen Lilu plugin and Lilu itself.
- [Apple](https://www.apple.com) for macOS and many of the kexts, frameworks and other binaries.
- [NVIDIA](https://www.nvidia.com) for creating NVIDIA Web Drivers.
- And other contributors that I forgot to mention.
I'm sorry if I forgot to mention other contributors on OCLP and PatcherSupportPkg about NVIDIA Web Drivers.

## Story of creation of this repo
I used NVIDIA GPU's (GT 630 and GTX 1050 Ti) with Sierra and High Sierra during Mojave, Catalina, Big Sur and Monterey eras. NGL, I felt a bit jealous while AMD and Intel iGPU users using most up-to-date OS'. But legends ([Khronokernel](https://github.com/Khronokernel), [DhinakG](https://github.com/DhinakG) and all other legends that has work on these patches) made NVIDIA Web Drivers able to run on newer OS', but only on Big Sur and above with OCLP 0.4.6. And you know that patch support for Mojave and Catalina was removed on OCLP 0.4.4 and last version for patching Mojave and Catalina was 0.4.3 which is not contain patch for NVIDIA Web Drivers. I viewed [this](https://www.reddit.com/r/hackintosh/comments/uxz95u/nvidia_web_drivers_running_on_macos_monterey/) Reddit post and [this](https://x.com/khronokernel/status/1529583832663437312) Twitter/X tweet/post.

But when I checked OCLP pull [#993](https://github.com/dortania/OpenCore-Legacy-Patcher/pull/993), I saw this:

![image](https://github.com/user-attachments/assets/d5f55737-01fc-43dd-9d8f-f88363c20de0)

Do you see what I see? There's 10.14.3 which stands for macOS Mojave build number. A Maxwell-based NVIDIA GPU ran on macOS Mojave 10.14.3 which is tested by legend [Khronokernel](https://github.com/Khronokernel) itself.

After that, I started investigating this. I took necessary files to copy and commands list to run from old version of OCLP and put these files and commands on [patchlist_for_web_drivers.txt](https://github.com/eflanili7881/NVIDIAWebDriversForMacOSMojaveAndCatalina/blob/main/patchlist_for_web_drivers.txt). I tried installing NVIDIA Web Drivers long before on Catalina test system, but due to I unpacked some *.framework files on Windows, I borked that Catalina test install due to borked symbolic links thats caused by extracted these files on Windows. But on date 18th December 2024, I tried this again, but unpacking all files on macOS itself instead, and guess what? I patched OS' finally to run NVIDIA Web Drivers with non-Metal mode just like on Big Sur and above on Mojave and Catalina!

- macOS Mojave 10.14.6 Security Update 2021-005 build 18G9323 

  ![image](https://github.com/user-attachments/assets/226b895f-7319-4bd2-9b8e-8e9c8a6ca89c)

- macOS Catalina 10.15.7 Security Update 2022-005 build 19H2026

  ![image](https://github.com/user-attachments/assets/239bd4c3-cd9f-41a6-97a1-3fd2d7f0b99f)

## Instructions
- Disable Library Validation via:
  - `amfi_get_out_of_my_way=0x1` boot argument or
  - [_cs_require_lv patch](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/67a78ec4a275e0e8f8941a5ff9d64f00c28397dc/payloads/Config/config.plist#L1352-L1381) kernel patch
- Enable Forced Compatibility via WhateverGreen via:
  - `ngfxcompat=1` boot argument or
  - `force-compat` with `Data` type that has value `01000000` on your GPU's device properties.
- Force OpenGL rendering via:
  - `ngfxgl=1` boot argument or
  - `disable-metal` with `Data` type that has value `01000000` on your GPU's device properties.
- Enable NVIDIA Web Drivers via:
  - `nvda_drv_vrl=1` boot argument
- Set your SIP (**S**ystem **I**ntegrity **P**rotection) level to **0xa03**.
  - `CSR_ALLOW_UNTRUSTED_KEXTS` (0x1)
    - Introduced with OS X El Capitan.
    - Allows unsigned kernel drivers to be installed and loaded.
  - `CSR_ALLOW_UNRESTRICTED_FS` (0x2)
    - Introduced with OS X El Capitan.
    - Allows unrestricted file system access.
  - `CSR_ALLOW_UNAPPROVED_KEXTS` (0x200)
    - Introduced with macOS High Sierra.
    - Allows unapproved kernel driver installation/loading.
  - `CSR_ALLOW_UNAUTHENTICATED_ROOT` (0x800)
    - Introduced with macOS Big Sur.
    - Allows custom APFS snapshots to be booted (primarily for modified root volumes).
    - Not necessary on Mojave and Catalina, but patches may somehow require this bit. You can omit it if you want.
      - So, new value for SIP is **0x203** (against **0xa03**) if you omit this bit.
- Install or update Lilu from [here](https://github.com/acidanthera/Lilu).
  - Lilu is requirement for various kernel extensions (*.kext) and WhateverGreen that you're gonna install or update on next step.
- Install or update WhateverGreen from [here](https://github.com/acidanthera/WhateverGreen).
- Download this repo's contents via **Code > Download ZIP**.
- Run commands thats inside on **commands.txt**.
  - `sudo /usr/bin/defaults write /Library/Preferences/.GlobalPreferences.plist InternalDebugUseGPUProcessForCanvasRenderingEnabled -bool false`
    - Disables the use of the GPU process for rendering canvas elements in applications that rely on macOS's rendering infrastructure.
  - `sudo /usr/bin/defaults write /Library/Preferences/.GlobalPreferences.plist WebKitExperimentalUseGPUProcessForCanvasRenderingEnabled -bool false`
    - Configures the WebKit rendering engine used by macOS applications (like Safari or other WebKit-based apps) to disable GPU-based rendering for `<canvas>` elements in web content.
  - `sudo /usr/bin/defaults write /Library/Preferences/com.apple.CoreDisplay useMetal -boolean no`
    - Disables the use of Metal, Apple’s modern graphics and compute API, for CoreDisplay-related rendering tasks on macOS.
  - `sudo /usr/bin/defaults write /Library/Preferences/com.apple.CoreDisplay useIOP -boolean no`
    - Disables the use of IOP (I/O Present), a framework or mechanism related to macOS's CoreDisplay subsystem.
- Remove necessary files thats inside on **files_to_delete.txt**.
  - On Catalina, run `sudo mount -uw /` to mount root filesystem as read-write.
  - I recommend you to delete files via `sudo rm -rf <dropFilesOnDelete.txtFile>`
    - Example
      - `sudo rm -rf /System/Library/Extensions/AMDRadeonX4000.kext /System/Library/Extensions/AMDRadeonX4000HWServices.kext /System/Library/Extensions/AMDRadeonX5000.kext /System/Library/Extensions/AMDRadeonX5000HWServices.kext /System/Library/Extensions/AMDRadeonX6000.kext /System/Library/Extensions/AMDRadeonX6000Framebuffer.kext /System/Library/Extensions/AMDRadeonX6000HWServices.kext /System/Library/Extensions/AppleIntelBDWGraphics.kext /System/Library/Extensions/AppleIntelBDWGraphicsFramebuffer.kext /System/Library/Extensions/AppleIntelCFLGraphicsFramebuffer.kext /System/Library/Extensions/AppleIntelHD4000Graphics.kext /System/Library/Extensions/AppleIntelHD5000Graphics.kext /System/Library/Extensions/AppleIntelICLGraphics.kext /System/Library/Extensions/AppleIntelICLLPGraphicsFramebuffer.kext /System/Library/Extensions/AppleIntelKBLGraphics.kext /System/Library/Extensions/AppleIntelKBLGraphicsFramebuffer.kext /System/Library/Extensions/AppleIntelSKLGraphics.kext /System/Library/Extensions/AppleIntelSKLGraphicsFramebuffer.kext /System/Library/Extensions/AppleIntelFramebufferAzul.kext /System/Library/Extensions/AppleIntelFramebufferCapri.kext /System/Library/Extensions/AppleParavirtGPU.kext /System/Library/Extensions/GeForce.kext /System/Library/Extensions/IOAcceleratorFamily2.kext /System/Library/Extensions/IOGPUFamily.kext /System/Library/Extensions/AppleAfterburner.kext /System/Library/Extensions/AppleCameraInterface.kext /System/Library/Extensions/NVDAStartup.kext`
  - If files missing, don't worry. Just delete files that just exists.
- Copy all files from **copy_mojave** or **copy_catalina** that depends to your currently OS version you want to patch to root filesystem.
  - On Catalina, run `sudo mount -uw /` to mount root filesystem as read-write.
  - Optionally, you can run `sudo chown root:wheel` and `sudo chmod 755` per file after you copied files to appropriate locations. I didn't do that but kexts and frameworks are still loaded. Maybe they're automatically set owner/group and permission octal values by macOS itself or by reducing level of SIP.
  - I recommend you to copy files via `sudo cp -R <pathOfcopy_mojaveOrcopy_catalina>/* /`
    - Example for Catalina
      - `sudo cp -R /Volumes/Untitled/copy_catalina/* /`
    - Example for Mojave
      - `sudo cp -R /Volumes/Untitled/copy_mojave/* /`
- After you copied files, run `sudo kextcache -i /` to rebuild kext cache.
- Reboot your system.

After these instructions, you should get hardware acceleration in non-Metal mode on macOS Mojave and Catalina. But because of the system now runs on non-Metal (OpenGL) mode, bugs on OCLP issues [#108](https://github.com/dortania/OpenCore-Legacy-Patcher/issues/108) may occur.
