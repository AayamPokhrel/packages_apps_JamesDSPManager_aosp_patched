# JamesDSPManager Patched

This is a patched version of the original APK downloaded from the [PBone Deployment Server](https://nightly.timschneeberger.me/jamesdsp-rootfull/).

## Integration in AOSP
It adapts the app for integration into an AOSP environment as a privileged app and includes internal fixes for processing audio from web browsers.

Clone this repo to your ROM Source:
```bash
git clone https://github.com/AayamPokhrel/packages_apps_JamesDSPManager_aosp_patched.git packages/apps/JamesDSPManager
```

Add the configuration to your `device.mk`:
```makefile
$(call inherit-product, packages/apps/JamesDSPManager/config.mk)
```

Register the audio libs under `<libraries>` and `<effects>` in the `audio_effects.xml` of your device tree. *(Note: This can vary according to the device, so add them accordingly.)*
```xml
<library name="jdsp" path="libjamesdsp.so"/>
<effect name="jamesdsp" library="jdsp" uuid="f27317f4-c984-4de6-9a90-545759495bf2"/>
```

Define SELinux properties in your device tree (`hal_audio_default.te`):
```te
allow hal_audio_default priv_app:fifo_file { write };
allow hal_audio_default platform_app:fifo_file { write };
```

## Changes in this Patch:
- **Permissions:** Added required privileged audio permissions (`MODIFY_AUDIO_ROUTING`, `QUERY_AUDIO_STATE`, `MODIFY_AUDIO_SETTINGS_PRIVILEGED`) to AndroidManifest to clear SecurityExceptions.
- **Dynamic Regex:** Patched the internal log parser to successfully map and apply audio effects to AAudio streams and web browsers.

## Credits:
- **James Fung (james34602)** for the original JamesDSP audio engine.
- **Tim Schneeberger (ThePBone)** for the RootlessJamesDSP app and dumpsys logic.
- **TogoFire** for the AOSP packages/apps wrapper reference. [Referenced repo](https://github.com/TogoFire/packages_apps_JamesDSPManager)
- All credit for the original JamesDSPManager project and its upstream work goes to their respective contributors.
 
## License
Please refer to the original project for licensing information and preserve all applicable copyright and license notices.