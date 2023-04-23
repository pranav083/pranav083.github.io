---
title: "Control Android on Ubuntu with Internet" # Title of the blog post.
date: 2023-04-22T23:32:51+05:30 # Date of post creation.
description: "Article description." # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: false # Set to true to group assets like images in the same folder as this post.
featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
featureImageAlt: 'Description of image' # Alternative text for featured image.
featureImageCap: 'This is the featured image.' # Caption (optional).
thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Technology
tags:
  - Tag_name1
  - Tag_name2
# comment: false # Disable comment if false.
---
# How to Control Your Android Device with

# Ubuntu Using Scrcpy

Are you someone who uses their Android device to access the internet but doesn't have Wi-Fi access? Do
you wish to control your Android device from your Ubuntu computer?  

If yes, then `Scrcpy` is the perfect solution for you. In this blog post,
we'll guide you through the process of casting and controlling your Android device
on Ubuntu using `Scrcpy`. And luckily, this is  a great software called `Scrcpy` 
that makes this process a breeze.

## What You Need

### Hardware

* A computer running Ubuntu or another Linux distribution (tested on Ubuntu 22.04)
  but can work on other linux system also
* An Android device with a USB cable for connection

### Software You Need

* **Scrcpy** 
* **screen**
* **adb**
## Basic Setup :

1. Install these software from the given command :  
   - Open the terminal in the Ubuntu system by pressing `Ctrl+Alt+t`.
   - **Scrcpy** : Install `scrcpy` on your Ubuntu computer using the command :

      ```console
      sudo apt-get install scrcpy
      ```  

   - **screen** :  Install `screen` on your Ubuntu computer using the command :

      ```console
      sudo apt-get install scrcpy
      ```  
   - **adb** :  Install `adb` on your Ubuntu computer using the command :

      ```console
      sudo apt-get install adb
      ```  

2. Here are the steps for turning on USB debugging in different Android devices:

  <details> <summary><b>Stock Android </b>(Click to expand):</summary> Go to <b>Settings</b> > <b>About Phone</b>, then tap <i>Build Number</i> seven times to enable Developer Options.    
  Go back to <b>Settings</b> > <b>Developer Options</b>, and <u><i>toggle on</i></u> --> <b><i>USB debugging</i></b>.</details>

  <details> <summary><b>Google Pixel/Nexus devices </b>(Click to expand):</summary> Go to <b>Settings</b> > <b>About Phone</b>, then tap <i>Build Number</i> seven times to enable Developer Options.
  Go back to <b>Settings</b> > <b>System</b> > <b>Developer Options</b>, and <u><i>toggle on</i></u> --> <b><i>USB debugging</i></b>.</details>

  <details> <summary><b>Samsung devices </b>(Click to expand):</summary> Go to <b>Settings</b> > <b>About Phone</b>, then tap Software Information. Tap <i>Build Number</i> seven times to enable Developer Options. 
  Go back to <b>Settings</b> > <b>Developer Options</b>, and <u><i>toggle on</i></u> --> <b><i>USB debugging</i></b>.</details>

  <details> <summary><b>LG devices </b>(Click to expand):</summary> Go to <b>Settings</b> > <b>About Phone</b>, then tap Software Information. Tap <i>Build Number</i> seven times to enable Developer Options. 
  Go back to <b>Settings</b> > <b>Developer Options</b>, and <u><i>toggle on</i></u> --> <b><i>USB debugging</i></b>.</details>

  <details> <summary><b>Motorola devices </b>(Click to expand):</summary> Go to <b>Settings</b> > <b>About Phone</b>, then tap <i>Build Number</i> seven times to enable Developer Options. 
  Go back to <b>Settings</b> > <b>Developer Options</b>, and <u><i>toggle on</i></u> --> <b><i>USB debugging</i></b>.</details>

  <details> <summary><b>OnePlus devices </b>(Click to expand):</summary> Go to <b>Settings</b> > <b>About Phone</b>, then tap <i>Build Number</i> seven times to enable Developer Options. 
  Go back to <b>Settings</b> > <b>Developer Options</b>, and <u><i>toggle on</i></u> --> <b><i>USB debugging</i></b>.</details>


1. Once you are done with `Step-2`. Turn on `USB tethering` as default setting from the develop setting, follow these steps :
   
   1. Once Developer Options are enabled, go to **Settings** > **System** > **Developer options**.

   2. Scroll down to the "**Networking**" section and enable the "USB configuration" option by.

   3. Tap on the "**USB configuration**" option and select "**USB tethering**" from the list of options.

   4. Now, whenever you connect the Android device to a computer via USB, USB tethering will be enabled by default.  

    `REMEMBER` : before connecting your android device to computer. It should be in unlock state.

    Note, that the exact steps and options may vary depending on the Android device and version of the operating system.

<p align="center">
<img align="center" width="600" height="600" class="special-img-class" src="/images/linux_android/2023-04-23-16-25-49.png" alt ="lsusb" title ="USB tethering" >
</p>

4. Follow these steps : 

   1. Open the terminal in the Ubuntu system by pressing `Ctrl+Alt+t`.

   2. Make the `scrcpy` executable bash script, Put command in the terminal : 
       ```console
       cd Documents/
       mkdir script
       nano auto_connect_scrcpy.sh 
       ```
   3. Paste this inside the `nano` text editor and save the file by pressing `Ctrl + s` and `Ctrl + x`:
       ```bash
       #!/bin/bash
       sleep 2
       export XDG_RUNTIME_DIR=/run/user/$(id -u)
       export LD_LIBRARY_PATH=/usr/lib:/usr/lib64
       export XDG_RUNTIME_DIR=/run/user/$(id -u)

       scrcpy -V verbose --max-size 800 --disable-screensaver --legacy-paste -t -w --always-on-top  
       sleep 2
       ```
       
         <details> <summary>(Click to expand for code explanation):</summary> 
         
         <p>Here's a breakdown of the different arguments being used:</p>

         <p><strong>export XDG_RUNTIME_DIR=/run/user/</strong>$(id -u): This line sets the <code>XDG_RUNTIME_DIR</code> environment variable to the current user&#39;s runtime directory, which is used by many Linux applications to store runtime data. The $(id -u) command returns the current user&#39;s user ID, which is used to create a unique runtime directory for each user.</p>

         <p><strong>export LD_LIBRARY_PATH=/usr/lib:/usr/lib64</strong>: This line sets the <code>LD_LIBRARY_PATH</code> environment variable to include the system library paths for 64-bit and 32-bit libraries. This is useful for ensuring that the scrcpy software can access the necessary system libraries when running on the Linux operating system.</p>

         <p><strong>export XDG_RUNTIME_DIR=/run/user/$(id -u)</strong>: This line sets the <code>XDG_RUNTIME_DIR</code> environment variable again, likely as a redundant measure to ensure that it is correctly set.</p>

         <br>
         <pre><code class="lang-console"><span class="hljs-comment">scrcpy</span> <span class="hljs-literal">-</span><span class="hljs-comment">V</span> <span class="hljs-comment">verbose</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">max</span><span class="hljs-literal">-</span><span class="hljs-comment">size</span> <span class="hljs-comment">800</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">disable</span><span class="hljs-literal">-</span><span class="hljs-comment">screensaver</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">legacy</span><span class="hljs-literal">-</span><span class="hljs-comment">paste</span> <span class="hljs-literal">-</span><span class="hljs-comment">t</span> <span class="hljs-literal">-</span><span class="hljs-comment">w</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">always</span><span class="hljs-literal">-</span><span class="hljs-comment">on</span><span class="hljs-literal">-</span><span class="hljs-comment">top</span>
         </code></pre>

         <p><b>scrcpy</b>: This is the command used to launch the scrcpy software.</p>

         <p><b>-V verbose</b>: This argument enables verbose logging, which provides more detailed output during the execution of the script.</p>

         <p><b>--max-size 800</b>: This argument sets the maximum display size of the Android device being cast to 800 pixels. This is useful for limiting the size of the device's display to fit within the available screen real estate on the computer.</p>

         <p><b>--disable-screensaver</b>: This argument disables the screensaver on the computer, preventing the display from turning off during use.</p>

         <p><b>--legacy-paste</b>: This argument enables the use of legacy paste functionality. This is useful for users who are experiencing issues with clipboard functionality when copying and pasting text between the Android device and the computer.</p>

         <p><b>-t</b>: This argument enables touch events on the Android device, allowing the user to interact with the device via the computer's mouse and keyboard.</p>

         <p><b>-w</b>: This argument sets the window mode to &quot;windowed&quot;, which displays the Android device in a separate window on the computer screen.</p>

         <p><b>--always-on-top</b>: This argument keeps the Android device window always on top of other windows on the computer screen, ensuring that it remains visible and easily accessible.</p>
         
         </details>

   4. Make the above bash script executable by putting this command:
       ```bash
       chmod +x auto_connect_scrcpy.sh 
       ```    
   5. Put these command in the terminal : 
       ```console
       nano Documents/script/virtual_terminal.sh 
       ```
   6. Paste this inside the `nano` text editor ans save the file by pressing `Ctrl + s` and `Ctrl + x`:
       ```bash
       #!/bin/bash
       /bin/bash -c '/home/mighty/Documents/script/auto_connect_scrcpy.sh; exec bash'
       ```
   7.  Make the above bash script executable by putting this command:
       ```bash
       chmod +x virtual_terminal.sh 
       ```  
 
5. Once these above step are done, Now, we have to configure the `udev rule` for the android system in the Ubuntu system. So, that whenever we connect the android device it will trigger the custom script.Follow these steps :  
 * First, find the id of your `android device`. Type `lsusb` in the terminal:
        ```console
        lsusb
        ```
<p align="center">
<img align="center" width="600" height="600" class="special-img-class" src="/images/linux_android/2023-04-23-12-43-04.png" alt ="lsusb" title ="lsusb output" >
</p>

  * In that list my device `ATTR{idVendor}=="22b8"` is, we will use in udev script:

<p align="center">
<img align="center" width="600" height="600" class="special-img-class" src="/images/linux_android/2023-04-23-12-48-46.png" alt ="lsusb" title ="lsusb output" >
</p> 

   * Now we know our device id, Make the `udev` script, Open the terminal in the Ubuntu system by pressing `Ctrl+Alt+t` and type/paste.
       ```console
      sudo nano /etc/udev/rules.d/71-udev-rule.rules
       ```
   * Paste this inside the `nano` text editor and save the file by pressing `Ctrl + s` and `Ctrl + x`:
       ```bash
      SUBSYSTEM=="usb", ATTR{idVendor}=="<your id vendor>", RUN+="/bin/su -c '/usr/bin/Xvfb :1 -screen 0 1024x768x16 & /usr/bin/screen -dmS myscript /usr/bin/bash -c \"export DISPLAY=:1; export XAUTHORITY=/home/<your username>/.Xauthority; /home/<your username>/Documents/script/virtual_terminal.sh\"' <your username>"
       ```
   * Here `<your id vendor>` is `"22b8"`(form previous step) To get your `<your username>`, Open terminal and type  : 
      
      ```console
      whoami
      ```
      Expected output :
<p align="center">
<img align="center" width="600" height="600" class="special-img-class" src="/images/linux_android/2023-04-23-13-07-43.png" alt ="lsusb" title ="your username" >
</p> 

   *  the above script will look something like this :  
      ```console    
        SUBSYSTEM=="usb", ATTR{idVendor}=="22b8", RUN+="/bin/su -c '/usr/bin/Xvfb :1 -screen 0 1024x768x16 & /usr/bin/screen -dmS myscript /usr/bin/bash -c \"export DISPLAY=:1; export XAUTHORITY=/home/mighty/.Xauthority; /home/mighty/Documents/script/virtual_terminal.sh\"' mighty"
      ```  
6. Now once done with the above steps, type this to reload the udev rules :
  ```console
  sudo udevadm control --reload-rules # reload udev rules
  sudo service udev restart # restart the udev
  ```
7. Reconnect your android usb device you will see the output like this :
<iframe width="1280" height="978" src="https://www.youtube.com/embed/c55KvYbHep8?list=PLttoix_9Us2yEm9AvkGHrOhk76oFrhHyY" title="Control Your Android Device with Ubuntu Using Scrcpy  with internet (without ROOT)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Conclusion :

One of the best features of scrcpy is its auto-connect functionality. This means that whenever you connect your Android device to your computer, scrcpy will automatically launch and begin displaying your device on your screen. This saves you time and hassle and makes it even more convenient to use your phone as an internet source with USB tethering turn on.

Overall, scrcpy is a fantastic tool for anyone who wants to cast and control their Android device from their Ubuntu system. With its easy installation, automatic connection, and customizable display settings, it's the perfect solution for people who need to use their phone for internet access but don't want to deal with the limitations of a small screen or touchscreen typing.

