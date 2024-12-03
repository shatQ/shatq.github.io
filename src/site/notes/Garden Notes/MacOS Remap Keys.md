---
{"dg-publish":true,"permalink":"/garden-notes/mac-os-remap-keys/","tags":["note","seedling"],"created":"2023-01-30T09:48:00","updated":"2024-11-29T14:51"}
---

# MacOS Remap Keys

Re-map right hand `Command` and `Option` keys:

```bash
hidutil property --set '{"UserKeyMapping": [{"HIDKeyboardModifierMappingSrc":0x7000000e7, "HIDKeyboardModifierMappingDst":0x7000000e6}, {"HIDKeyboardModifierMappingSrc":0x7000000e6, "HIDKeyboardModifierMappingDst":0x7000000e7}] }'
```

To make remap persistent create launch agent configuration file `~/Library/LaunchAgents/com.example.KeyRemapping.plist` with the following content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.example.KeyRemapping</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/hidutil</string>
        <string>property</string>
        <string>--set</string>
        <string>{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x7000000e7, "HIDKeyboardModifierMappingDst":0x7000000e6}, {"HIDKeyboardModifierMappingSrc":0x7000000e6, "HIDKeyboardModifierMappingDst":0x7000000e7}]}</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

---
- https://itectec.com/askdifferent/macos-changing-right-hand-command-alt-key-order-to-be-like-a-windows-keyboard/
- https://www.nanoant.com/mac/macos-function-key-remapping-with-hidutil
- https://developer.apple.com/library/archive/technotes/tn2450/_index.html