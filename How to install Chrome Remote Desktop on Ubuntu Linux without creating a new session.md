# How to install Chrome Remote Desktop on Ubuntu Linux without creating a new session

> **Important:**  _This method of getting Chrome Remote Desktop to work on Ubuntu seems to no longer work from my experience. Instead, I recommend using [DWService](https://www.dwservice.net/), a different browser based remote desktop software, that has many more features, works fine on Linux and Windows, and is completely free._

## Introduction

When installing Chrome Remote Desktop on Ubuntu Linux the standard way, it will try to create a new session and show this message instead of showing you what is currently on the computer's screen:

<img width="400" src="https://user-images.githubusercontent.com/87048351/216847379-26bf946e-b132-4bdc-b1b5-82e8f96920f3.jpg">

If you instead do not want to create a new session and only mirror what is currently on the computer's screen, follow these steps.

## Installation

Go to [Chrome Remote Desktop's website](https://remotedesktop.google.com/access/) on Google Chrome and begin the steps to setup your computer for remote access. Once it downloads Chrome Remote Desktop, open terminal and run `sudo apt-get install [path of downloaded file]`, replacing `[path of downloaded file]` with the path of the downloaded `.deb` file. Once this finishes, return back to Google Chrome and finish the setup steps.

_Note: If there are any issues with Chrome Remote Desktop when following these steps, run `sudo apt-get --purge remove chrome-remote-desktop` to uninstall it and try again._

Now, try to connect to the computer using Chrome Remote Desktop on another device signed in with the same Google account. You should see a simmilar message to the one shown in the image below:

<img width="400" src="https://user-images.githubusercontent.com/87048351/216847379-26bf946e-b132-4bdc-b1b5-82e8f96920f3.jpg">

If you see this message, then everything is working as intended so far. You may now disconnect from the computer.

## Modifying Chrome Remote Desktop to use the current session

Open the folder at `/opt/google/chrome-remote-desktop/` and make 2 copies of the `chrome-remote-desktop` file in another folder. One copy will be a backup and one will be used for editing.

Open the copy you made for editing in a text editor, and find the line that says `DEFAULT_SIZES`. Change this to the resolution of your screen. Example: `DEFAULT_SIZES = "1920x1080"`

_Note: I don't reccomend editing the file in a terminal text editor like nano because the file is very large, so you may need to use Ctrl+F to find the lines you need to edit._

Now, open terminal and run `echo $DISPLAY`. Back in the `chrome-remote-desktop` file, find the line that says `FIRST_X_DISPLAY_NUMBER` and change it to the number returned by `echo $DISPLAY`. Example: `FIRST_X_DISPLAY_NUMBER = 0`

Next, find and comment out the lines that say:
```python
while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
    display += 1
```
_Note: Commenting out lines is done by adding a `#` in front of the line._

Then, find the line that says `def launch_session(self, server_args, backoff_time):`. The code below this line defines the `launch_session()` function. Inside this function, comment out the lines:
```python
self._launch_server(server_args)
if not self._launch_pre_session():
      # If there was no pre-session script, launch the session immediately.
      self.launch_desktop_session()
```
And add these lines after those:
```python
display = self.get_unused_display_number()
self.child_env["DISPLAY"] = ":%d" % display
```
Finally, save the file and run these commands in the terminal, replacing `[path of file you just edited]` with the path of the `chrome-remote-desktop` file copy you just edited:
```
sudo /opt/google/chrome-remote-desktop/chrome-remote-desktop --stop
sudo cp [path of file you just edited] /opt/google/chrome-remote-desktop
sudo /opt/google/chrome-remote-desktop/chrome-remote-desktop --start
```
_Note: You should not delete the edited copy of the file, because updating Chrome Remote Desktop may undo or break the edits made to the `chrome-remote-desktop` file. If this ever happens, you **may** be able to repeat the commands above to fix it as long as you kept the edited file._

To make sure this worked, connect to the computer again using Chrome Remote Desktop on another device signed in with the same Google account, and you **should not** see a message anymore about creating a new session. You should just see a mirror of what is currently on your screen, and be able to intereact with it. If your computer shows as offline, restart the computer and check again.

## Sources

All of the information in this tutorial comes from [this outdated Superuser StackExtange post](https://superuser.com/questions/778028/configuring-chrome-remote-desktop-with-ubuntu-gnome-14-04/850359#850359), and from my own experience with trying to get this solution to work on the latest version of Chrome Remote Desktop in 2023.

_Last updated 8/23/2023_

<p align="center">
<button name="button" onclick="navigator.share({
    title: 'How to install Chrome Remote Desktop on Ubuntu Linux without creating a new session',
    text: 'When installing Chrome Remote Desktop on Ubuntu Linux the standard way, it will try to create a new session. If you instead do not want to create a new session and only mirror what is currently on the computer’s screen, follow these steps.',
    url: 'https://🔄️.ainyaku.com/CRD-Ubuntu',
  });">Share</button>
<button name="button" onclick="print();">Print</button>
<button name="button" onclick="window.location.href='https://ainyaku.com';">Back to ainyaku.com</button>
</p>
