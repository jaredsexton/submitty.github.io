---
title: VM Install using Vagrant
category: Developer
order: 2
---

The instructions below will setup an instance of Submitty on your own
hardware that will have several courses, many sample assignments, and
a hundred students with assignment submissions so you can explore the
features of Submitty as it would appear "mid-semester".  Your host
computer can run any modern operating system (Windows, Mac, or
Unix/Linux).  The installation process will create a new Virtual
Machine (VM) on your computer and the VM will use the Ubuntu GNU/Linux
operating system.


1. To develop with a Virtual Machine (VM), your computer should have
   at least 8GB of RAM and a 64-bit host OS.  AMD-V or Intel VT-x are
   also required (most computers have these).  Submitty is RAM and I/O
   intensive, so more RAM and a fast disk are better.


2. Enable [Virtualization](http://tinyurl.com/enable-virtualization)
   
   **Mac Instructions**  
   Virtualization is generally enabled by default
   
   **Windows 10 Instructions**  
   Open Settings, navigate to _Advanced Startup_ and select _Restart Now_.
   ![Relevant Screenshot](https://github.com/jaredsexton/submitty.github.io/blob/master/images/Virtualization_Instructions_1.png?raw=true)
   
   Navigate to _Troubleshoot -> Advanced Options -> UEFI Firmware Settings_ and restart as suggested.
   
   Navigate to _BIOS Settings_ from your PC's startup menu, locate _Virtualization Technology_ and enable it.
   
   **Ubuntu Instructions**  
   Enter BIOS (generally by pressing Del or F12 while booting) and navigate the _BIOS Settings_,
   locate _Virtualization_, and enable it.
   
   Be sure to choose _Hardware Virtualization_ in the _System -> Acceleration_ settings of the virtual machine you are using.


3. Download and install [VirtualBox](https://www.virtualbox.org/), [Vagrant](https://www.vagrantup.com), and [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest).

   The latest Vagrant + VirtualBox combo that has been tested is _Vagrant 1.9.3_ and _VirtualBox 5.0.38_ (on Mac 10.12.6)

   Below are quick steps to get everything installed and running.

   **Windows Installation**  
   You can just go to the respective sites and download the necessary binaries.

   **Mac Installation**  
   Install [homebrew](http://brew.sh/) if you don't have it and then run:
   ```
   brew cask install virtualbox
   brew cask install vagrant
   sudo vagrant plugin install vagrant-vbguest
   ```

   **Ubuntu Installation**
   
   **NOTE:** The Ubuntu repository does not contain the latest version of Vagrant or VirtualBox.
   To accomodate for this, we have changed the setup instructions until this is resolved.
   Please make sure to replace *`<mydist>`* with your distribution name
   (see [here](https://www.virtualbox.org/wiki/Linux_Downloads))
   ```bash
   # Add VirtualBox
   sudo add-apt-repository "deb http://download.virtualbox.org/virtualbox/debian <mydist> contrib"
   wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
   wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
   
   # Add Vagrant
   sudo add-apt-repository "deb https://vagrant-deb.linestarve.com/ any main"
   sudo apt-key adv --keyserver pgp.mit.edu --recv-key
	
   sudo apt-get update
   
   # Install Packages
   sudo apt-get install virtualbox-5.2 vagrant
   sudo vagrant plugin install vagrant-vbguest
   ```

2. Clone [the Submitty repository](https://github.com/Submitty/Submitty) to a location on
   your computer (the "host").

   ```
   git clone https://github.com/Submitty/Submitty.git
   ```


3. Navigate to that directory on your computer in a shell/terminal and
   type:

   ```
   vagrant up
   ```

   Vagrant will build your VM.  This will take maybe 30 minutes to a
   few hours depending on your Internet connection speed.  When this
   command finishes, your VM is ready to use.


5. To stop and restart the VM:

   * When you are finished working, you can suspend the virtual
     machine (save state, a little faster to restart):

     ```
     vagrant suspend
     ```

     or halt the virtual machine (complete VM shutdown, a little
     slower to restart):

     ```
     vagrant halt
     ```

   * To resume work on a VM that is suspended or halted:

     ```
     vagrant up
     ```

   * If you just want to restart the VM (same as halt/up), type:
     ```
     vagrant reload
     ```

     If you haven't made any drastic changes to the build script,
     you should be able to just re-provision the VM. You can do this by
     using the `--provision` flag with either `up` or  `reload`. This is
     will be faster than doing a full `destroy`/`up`, however depending on
     the changes you've done to the VM, could leave it potentially unstable.
     If the VM breaks, simply `destroy`/`up` as normal.

     NOTE: when resuming work, you may see this warning several
     times, `default: Warning: Remote connection
     disconnect. Retrying..  .` These warnings are not harmful and can
     be ignored.

6. To completely delete the virtual machine (such as to start over from
   scratch with a fresh VM), type:

   ```
   vagrant destroy
   ```

   And if desired:

   ```
   vagrant up
   ```


7. When the VM is "up", you can go visit the homework submission
   website.

   * From a web browser (Chrome, Firefox, IE, etc.) on your host
     computer, go to:   
     <http://192.168.56.101/index.php>  
     (see the VM login & password info below)

   * You can test the submission, autograding, and viewing of the
     grades details by uploading sample submissions from the Submitty
     repository, located in one of these these directories:

     For the "tutorial" course:  
     <https://github.com/Submitty/Tutorial/tree/master/examples> 

     For the "sample" course:  
     <https://github.com/Submitty/Submitty/tree/master/more_autograding_examples>


8. When the VM is "up", you can connect from your host computer to the
   virtual machine via ssh.  Windows users will need to install SSH
   software (e.g., 
   [Cygwin](https://www.cygwin.com/) or 
   [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)).  
   From a terminal in the
   repository directory type:

   ```
   vagrant ssh
   ```

   You will connect to the VM as user `vagrant` initially.


9. The following users exist on the VM:

   | user | password |
   |------|----------|
   | vagrant | vagrant |
   | root | vagrant |
   | hsdbu | hsdbu |
   | hwcgi | hwcgi |
   | hwphp | hwphp |
   | hwcron | hwcron |
   | instructor | instructor |
   | ta | ta |
   | developer | developer |
   | postgres | postgres |
   | student | student |


10. The VM has the following four courses by default and they are all part of the current semester:

    * tutorial
    * sample
    * development
    * blank

    *Note: The current semester is calculated by either using an `s` if in the month is < 7 else use `f`
    and then take the last two digits of the current year. So April 2017 would be `s17` while September
    2017 would be `f17`.*


