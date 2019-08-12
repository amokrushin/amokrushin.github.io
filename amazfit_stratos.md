## Apps

* https://4pda.ru/forum/index.php?showtopic=768365&st=20260#entry83216921

## Development

* https://github.com/KieronQuinn/AmazfitStepNotify
* https://github.com/manuel-alvarez-alvarez/malvarez-watchface
* https://github.com/drbourbon/drbourbon-watchfaces

```
adb shell getprop ro.build.version.release
5.1
adb shell getprop ro.build.version.sdk
22
```

### ADB over WiFi

```
# discover IP
adb shell ip -f inet addr show wlan0

# restart adbd listening on TCP on PORT
adb tcpip 5555
adb connect 192.168.1.5:5555
adb disconnect 192.168.1.5:5555

# restart adbd listening on USB
adb usb
```
