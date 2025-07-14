---
title: Westview TitanTurtles Cybersecurity Club
layout: default
---

## [Root](./index.html) | [Join](./apply.html) | [Camp](./cybercamp.html) | [Blog](./blog.html) | [Touch](./contacts.html) | **Grind** | [Caregiver](./techcg.html) | [Events](./events.html) | [Legend](./legend.html)

# Virtualized Intel VT-x/EPT with VMware

## Problem

Usually, with either Intel or AMD computer, you can enable the Virtualization Technology or something similar in BIOS so that in VMware you can use "Virtualized Intel VT-x/EPT or AMD-V/RVI" settings for Processors

<img width="768" height="745" alt="Screenshot 2025-07-14 164220" src="https://github.com/user-attachments/assets/910134aa-29aa-425f-9b0c-ad9c6bccbb50" />

However, you may still encounter error when boot up the virtual machine.

## Solution

The root cause is the "Virtualization-based security" is enabled. You can double check it by running "msinfo32" in the Windows search area. And, you will see this:

<img width="640" height="381" alt="image" src="https://github.com/user-attachments/assets/b84a22d2-f5c7-4315-8abc-2830309b1d1b" />

To turn it off, you can use the scripts provided in this website: [https://consumer.huawei.com/cn/support/content/zh-cn16012808/](https://consumer.huawei.com/cn/support/content/zh-cn16012808/). 

Or, you can create your own batch file and type in the following command and run it:

```
@echo off

dism /Online /Disable-Feature:microsoft-hyper-v-all /NoRestart
dism /Online /Disable-Feature:IsolatedUserMode /NoRestart
dism /Online /Disable-Feature:Microsoft-Hyper-V-Hypervisor /NoRestart
dism /Online /Disable-Feature:Microsoft-Hyper-V-Online /NoRestart
dism /Online /Disable-Feature:HypervisorPlatform /NoRestart

REM ===========================================

mountvol X: /s
copy %WINDIR%\System32\SecConfig.efi X:\EFI\Microsoft\Boot\SecConfig.efi /Y
bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=X:
mountvol X: /d
bcdedit /set hypervisorlaunchtype off

echo.
echo.
echo.
echo.
echo =======================================================
echo Finished. Please reboot your computer.
pause > nul
echo.
echo.
```

After running the batch commands, manually reboot the machine. And, type F3 several times when you see this screen during rebooting:

<img width="833" height="538" alt="image" src="https://github.com/user-attachments/assets/86b123f7-060a-4e05-974d-934c6aff261e" />

Afterwards, the "Virtualization-based security" will be disabled. And, you should be able to run VMware virtual machines with "Virtualized Intel VT-x/EPT or AMD-V/RVI" turned ON.

<img width="494" height="119" alt="image" src="https://github.com/user-attachments/assets/91d735d1-13c5-4ca6-87f9-4df24984a624" />

<img width="759" height="277" alt="image" src="https://github.com/user-attachments/assets/439893d7-9603-4a8a-98d8-6af0d9aa530d" />


