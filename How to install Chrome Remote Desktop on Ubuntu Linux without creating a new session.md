# How to install Chrome Remote Desktop on Ubuntu Linux without creating a new session

## Introduction

When installing Chrome Remote Desktop on Ubuntu Linux the standard way, it will try to create a new session and show this message instead of showing you what is currently on the computer's screen:

<img width="400" src="https://user-images.githubusercontent.com/87048351/216847379-26bf946e-b132-4bdc-b1b5-82e8f96920f3.jpg">

If you instead do not want to create a new session and only mirror what is currently on the computer's screen, follow these steps.

## Installation

Go to [Chrome Remote Desktop's website](https://remotedesktop.google.com/access/) on Google Chrome and begin the steps to setup your computer for remote access. Once it downloads Chrome Remote Desktop, open terminal and run `sudo apt-get install [path of downloaded file]`. Once this finishes, return back to Google Chrome and finish the setup steps.

_Note: if there are any issues with Chrome Remote Desktop when following these steps, run `sudo apt-get --purge remove chrome-remote-desktop` to uninstall it and try again._

Now, try to connect to the computer using Chrome Remote Desktop on another device signed in with the same Google account. You should see a simmilar message to the one shown in the image below:

<img width="400" src="https://user-images.githubusercontent.com/87048351/216847379-26bf946e-b132-4bdc-b1b5-82e8f96920f3.jpg">

If you see this message, then everything as intended so far. You may now disconnect from the computer.

## Modifying Chrome Remote Desktop to use the current session

Open the folder at `/opt/google/chrome-remote-desktop/` and copy the `chrome-remote-desktop` file to another folder make a backup. Then, make another copy of the file into a different folder so you can edit it.

Open the copy you made for editing in a text editor, and find the line that says `DEFAULT_SIZES`. Change this to the resolution of your screen like in this example: `DEFAULT_SIZES = "1920x1080"`

Now, open terminal again and run `echo $DISPLAY`, then remember the number that it returns. Then, in the `chrome-remote-desktop` file, find the line that says `FIRST_X_DISPLAY_NUMBER` and change it to the number returned by the `echo $DISPLAY` command like in this example: `FIRST_X_DISPLAY_NUMBER = 0`

Next, find the lines that say:
```python
while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
    display += 1
```
And comment them out so they look like this:
```python
#while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
   #display += 1
```

Now, find the line that says `def launch_session(self, server_args, backoff_time):`. The code below this defines the `launch_session()` function. Inside this section, comment out the lines:
```python
self._launch_server(server_args)
if not self._launch_pre_session():
      # If there was no pre-session script, launch the session immediately.
      self.launch_desktop_session()
```
So it looks like this:
```python
#self._launch_server(server_args)
#if not self._launch_pre_session():
      # If there was no pre-session script, launch the session immediately.
      #self.launch_desktop_session()
```
And then add these lines after those:
```python
display = self.get_unused_display_number()
self.child_env["DISPLAY"] = ":%d" % display
```
So now, the entire function should look like this:
```python
  def launch_session(self, server_args, backoff_time):
    """Launches process required for session and records the backoff time
    for inhibitors so that process restarts are not attempted again until
    that time has passed."""
    logging.info("Setting up and launching session")
    self._init_child_env()
    self.setup_audio()
    self._setup_gnubby()
    #self._launch_server(server_args)
    #if not self._launch_pre_session():
      # If there was no pre-session script, launch the session immediately.
      #self.launch_desktop_session()
    display = self.get_unused_display_number()
    self.child_env["DISPLAY"] = ":%d" % display
    self.server_inhibitor.record_started(MINIMUM_PROCESS_LIFETIME,
                                      backoff_time)
    self.session_inhibitor.record_started(MINIMUM_PROCESS_LIFETIME,
                                     backoff_time)   
```

Finally, save the file and run these commands in the terminal:
```
/opt/google/chrome-remote-desktop/chrome-remote-desktop --stop
sudo cp [path of file you just edited] /opt/google/chrome-remote-desktop
/opt/google/chrome-remote-desktop/chrome-remote-desktop --start
```
_Note: You should not delete the edited copy of the file, because updating Chrome Remote Desktop may undo or break the edits made to the `chrome-remote-desktop` file. If this ever happens, you **may** be able to repeat the commands above to fix it as long as you kept the edited file._

To make sure this worked, connect to the computer again using Chrome Remote Desktop on another device signed in with the same Google account, and you **should not** see a message anymore about creating a new session. You should just see a mirror of what is currently on your screen, and be able to intereact with it.

## Sources

All of the information in this tutorial comes from [this outdated Superuser StackExtange post](https://superuser.com/questions/778028/configuring-chrome-remote-desktop-with-ubuntu-gnome-14-04/850359#850359), and from my own experience with trying to get this solution to work on the latest version of Chrome Remote Desktop in 2023.

_Last updated 2/5/2023_

[button url="http://www.google.com"]
{% include button.html url="http://www.google.com" %}
<button name="button" onclick="http://www.google.com">Click me</button>
[Click me](http://www.google.com){: .btn}

| [Share](https://ainyaku.com/share-crd-linux-article.html)         | [Back to ainyaku.com](https://ainyaku.com/)
|---------------|--------------------|
