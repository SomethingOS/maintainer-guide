This guide and rules has been edited for SomethingOS from [CrDroid ones](https://github.com/crdroidandroid/rules-and-guidelines?tab=readme-ov-file#general-rules).
## General rules

- All maintainers must have knowledge about source control tools such as _git_ and _repo_.
- All maintainers must release device sources **publicly** on Github, including common device tree (if present), device tree and kernel
- Device tree and device specific sources need to be cloned via aospa.dependencies and be present on vendor_something
- Vendor is not mandatory due to possible proprietary code that can result in DMCA, but highly recommended also
- All sources must be fully synced (pushed to GitHub) **prior to** every official build release
- Device trees can be co-maintained
- All maintainers must test every build before release this including with testers if possible in order to avoid issues
- If some quality requirements (_more on that below_) can’t be passed, the maintainer must provide the reason for the exception while applying for maintainer status
- Maintainers must respect each other, any act of hate or abuse will be severely punished
- A forum thread (usually XDA) must be made using official template and contain all the device documentation such as installation steps, download links, sources

## Applying for Official

- Meet all the quality requirements (described above)
- Having a working build, fully working and public (On XDA for example).
- Plan to keep and maintain the device

Once you meet these conditions, contact @DylanAkp on telegram

## Git

- Git trees should be maintained in a tidy and organized manner
- Release branches (ones that are used for official releases) must be named after current Android version AOSPA codename, e.g. Android 14 branch name -> uvite, this naming is mandatory
- Maintainers are free to create additional backup/testing branches
- Original commit authorship must be maintained. If you modified it by some degree (forward port, conflict fix) you can leave a signoff or add your note at the end of a commit message, e.g. _'xNombre: fixed for R'_ Commits must preserve proper and informative naming, e.g. _'sm6150-common: Add missing camera prop'_ no _'fix cam'_ or _'aldksjflkajsd'_. For kernels shortened path or affected source file must be used for name, e.g. _'block: cfq: Fix uninitialized variable'_ or _'adsprpcd: Fix memory leak'_
- Commits must describe the change, especially reverts. Commits without proper messages are meaningless, showing that you have no actual idea what you’re doing. Reverts without a message doesn’t let others know what problem it was causing and it is generally bad for community
- Rebasing and force-pushing is allowed as long as it doesn’t affect other users badly, e.g. don’t force-push main branch of a common dt repository until consulted with all maintainers using it
- When force-pushing a branch it is advised to create a copy of it just in case, named _'mybranch-old'_ or _'mybranch-today's date'_

## Builds quality requirements

### Hardware:
- **Audio**
    - All audio outputs that are available in stock rom must be operational
- **RIL**
    - Must be operational if it is available in stock rom, it must not crash in any case
- **Camera**
    - Default’s system app - Aperture must be working, however can be overridden. Prebuilt camera apps extracted from stock (like _AnxCamera_) can be included only if it is fully functional and doesn’t crash randomly
- **USB**
    - All usb features such as MTP, PTP, ADB, tethering must be working
- **Bluetooth**
    - Must be fully operational
    - Maintainers can switch to A2DP stack if their hardware supports it
- **Wifi**
    - 5GHz must be working if it’s available in stock rom
    - Wifi hotspot must be working and should support 5GHz
- **Fingerprint Sensor**
    - Must be operational if included
- **Sensors**
    - All basic sensors must be working
- **GPS**
    - Must be operational and support same range of satellite types as stock
- **Storage**
    - Every storage memory must be working, internal and external (like sd cards)
    - Devices must support encryption. FDE devices should switch to FBE, FBE encryption must be enabled by default
    - Devices should be able to use latest encryption method following Google recommendations, metadata partition should be used when available

### Kernel:

- Builtin kernel must not modify default hardware configuration, which is altering cpu/gpu/mem/etc frequency/voltage because of stability reasons
- Must be built with stack protector strong
- Must be built with selinux and seccomp enabled, audit can be disabled
- You must not use any commit that masks disabled selinux or any other security-related feature
### Vendor

Maintainers are free to use stock-extracted blobs, blobs from other devices or leaked BSPs. However, maintainers must test them after any alteration and make sure new blobs are fully operational. They must check logcat and dmesg for any unusual output (like dlsym errors).

### Software:

- Only production builds can be shipped (user/userdebug)
- Official builds can be released as weekly, monthly or nightlies when needed but at least they must be released on every SomethingOS minor version update
- Selinux can be permissive during initial bringup days, only in beta builds. It must be enforced in every official release after non-dt related source is deemed stable
- Sepolicy must meet security standards, it must cover every system and vendor component (no random denials), however it must not contain any rules for 3rd party apps
- Neverallows MUST NOT be masked using _SELINUX_IGNORE_NEVERALLOWS_
- Official builds must not contain any additional apps included by the maintainer, except from apps extracted from stock rom and _GCam_ (exception to devices like Nothing Phones or some other that needs apps for some features to work, like Glyph)
