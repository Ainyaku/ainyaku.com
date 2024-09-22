# Installing Android APKs on Windows 11

> **Note:**  _[Microsoft has announced that the Windows Subsystem for Android™️ will no longer be supported as of March 5th 2025.](https://www.windowscentral.com/software-apps/windows-11/microsoft-is-killing-support-for-running-android-apps-on-windows-11) According to Microsoft, Android apps installed before this date will still work, but you will not be able to install new apps after WSA is discontinued. Because of this, I recommend installing the [Aurora Store](https://f-droid.org/packages/com.aurora.store/#:~:text=suggested) using this tutorial before March 5th 2025 so you can keep using it after the discontinuation._

With Windows 11 came the introduction of the Windows Subsystem for Android™, which allows you to install Android apps to your PC using the Amazon Appstore. The only issue with this is that the Amazon Appstore is very limited and doesn't offer many apps, but there is a simple solution. Installing APKs lets you install any app you want, including a better appstore like the [Aurora Store](https://f-droid.org/packages/com.aurora.store/#:~:text=suggested), which lets you download any app that is already on the Play Store.

## First Time Setup

1. Install the [Amazon Appstore](https://apps.microsoft.com/store/detail/amazon-appstore/9NJHK44TTKSX) to your PC and follow the instructions.

    <img width="400" src="https://user-images.githubusercontent.com/87048351/217704976-57e6e5eb-8584-46ee-a2ab-eed7177678a7.png">

2. Download and extract [AndroidAutoInstall.zip](https://drive.google.com/u/0/uc?id=1gtdcsUlZiXYtP5kW9PRFijNYmEoEtQqh&export=download) to a folder on your PC.

    _Note: You may see a message saying `AndroidAutoinstall.zip is not commonly downloaded and may be dangerous.` Just click on the down arrow and click `Keep`, and you can even view the contents of the zip file yourself if you really don't believe that it is perfectly safe._

    <img width="400" src="https://user-images.githubusercontent.com/87048351/217703811-f54592be-b014-4567-b941-1da29f7cd1dc.png">

3. Search for `Windows Subsystem for Android™` in the Windows search bar and open it, click on the _Advanced settings_ tab, then turn on Developer mode. 

    ![image](https://github.com/user-attachments/assets/226d9ad4-ad8a-4e4b-a201-e48fa4135fff)

    _Note: When you install an APK for the first time ever, you will see an error that says `adb.exe: device unauthorized.` See [Common Error A](#error-a) to fix this._

## Installing an APK

1. Make sure an existing Android app (like the Amazon Appstore) is currently open or was recently opened.

2. Open the APK you'd like to install with the `Install APK.bat` file from inside the .zip folder (set it as default so you can just double click on the APK next time).

    _Note: You may see a message saying `Windows protected your PC`. Just click `More info` and then `Run anyway`, and you can even view the contents of the zip file yourself if you really don't believe that it is perfectly safe._

    <img width="401" alt="Screenshot 2022-04-17 002707" src="https://user-images.githubusercontent.com/87048351/163703034-68cf4347-07fd-4a6e-b326-1df7435f3a17.png"><img width="401" alt="Screenshot 2022-04-17 002740" src="https://user-images.githubusercontent.com/87048351/163703033-11f4d5a3-8c38-478c-be0b-842ff5d7a34a.png">

3. If the installer says "Success", just search for the name of the app you just installed in the search bar in Windows and open it.

    _Note: You can reinstall an app that's already installed, or even currently open if you want to update it._

## Common Errors

### Error A

`adb.exe: device unauthorized.`

Sometimes (especially if it's the first time installing an APK) you may have to try again with an Android app currently open, then switch to the window with the Android app open and accept a dialogue that says `Allow ADB debugging?` and click `Always allow from this computer`.

![Screenshot 2023-02-08 220924 - Copy](https://user-images.githubusercontent.com/87048351/217709143-783ca586-4f1f-4967-9455-5bc5e78104b6.png)

### Error B

`cannot connect to 127.0.0.1:58526: No connection could be made because the target machine actively refused it. (10061). adb.exe: device offline`

Try opening an existing Android app (like the Amazon Appstore) again as you may not have opened an Android app recently enough.

### Error C

`adb: failed to install` PATH OF APK `: Failure [INSTALL_FAILED_UPDATE_INCOMPATIBLE: Existing package` PACKAGE NAME `signatures do not match newer version; ignoring!]`

Try uninstalling the app you're trying to update, then reinstall again.

### Other

If none of the above solves your issue, try restarting your computer, opening an existing Android app, and trying again.

<p align="center">
<button name="button" onclick="navigator.share({
    title: 'Installing Android APKs on Windows 11',
    text: 'How to install Android APKs on Windows 11 using the Windows Subsystem for Android™ without the Amazon Appstore.',
    url: 'https://🔄️.ainyaku.com/APKs-on-Win-11',
  });">Share</button>
<button name="button" onclick="print();">Print</button>
<button name="button" onclick="window.location.href='https://ainyaku.com';">Back to ainyaku.com</button>
</p>
