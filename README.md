# SEED-Lab-CloudLab-Setup


## Step 1: Install Software and Configure System


When the Ubuntu 20.04 VM is built, a default username with the root privilege
will be created in the system. The actual name of the user is typically
chosen by the cloud operator. Most cloud platforms will provide
a method for you to SSH into this account. Please log into the VM, and do the followings:

- Step 1.a: Update the system and install some necessary software.
  ```
  sudo apt update
  sudo apt -y install unzip git
  ```

- Step 1.b: Download [`src-cloud.zip`](https://github.com/monsterandy/SEED-Lab-CloudLab-Setup/archive/refs/heads/main.zip)
  from the link or using the following command (if copy-and-paste does not work
  for your SSH client, you may have to type the command; make sure you type
  the URL correctly):
  ```
  git clone https://github.com/monsterandy/SEED-Lab-CloudLab-Setup.git
  ```

- Step 1.c: After cloning the repo, you will see a `SEED-Lab-CloudLab-Setup` folder.
  Enter this folder, and run the following command to install software
  and configure the system.
  ```
  cd SEED-Lab-CloudLab-Setup
  chmod +x ./install.sh
  ./install.sh
  ```

- **Note:** This shell script will download and install all the software needed for
  the SEED labs. **The whole process will take around 20 minutes. 
  Do not put your computer into sleep or disconnect the SSH session.** Please
  don't leave, because you will be asked to make choices:

  - During the installation of `xfce4`, you will be asked to
    choose a default display manager. Choose `LightDM`.
  - Make sure to select 'No' when asked whether non-superuser should be able to capture packets.

**You do NOT need to reboot the cloud VM after above steps.** Please avoid rebooting your cloud VM as this may cause issues on the CloudLab platform.

After the script finishes, a new account called `seed` is created.
We will use this account for all the SEED labs, instead of the default one
created by the cloud. We intentionally did not set a password for this account,
so nobody can directly log into this account. We can switch to the `seed`
account using the following command (if you do not use `sudo`, the OS
will ask you to type the password, making it impossible to log in):
```
sudo su seed
cd  # go to the seed's home directory
```

## Step 2 (Option A): Access the VM Using VNC

For most labs, being able to SSH into the VM should be sufficient.
Some labs do need to access GUI applications on the VM, such as
Firefox and Wireshark. If your network bandwidth is not too
bad, being able to get a graphical desktop of the remote VM is
always more desirable than getting a text terminal via SSH.
We will use VNC (Virtual Network Computing) to get the remote
desktop.

- **On the cloud VM:** We need to make sure that we are in
  the `seed` account. If you are still in the default account, do
  the following, and you will be in the `seed` account:
  ```
  sudo su seed
  ```

  Our installation script has already installed
  the TigerVNC server program on the VM. You need to start the
  server. **Be aware, this command should only be run under the user `seed`.**
  ```
  vncserver -localhost no
  ```  

  By default, TigerVNC server only listens to localhost/127.0.0.1. The
  purpose of the `-localhost no` option means accepting access from the
  outside. **When we first start the `vncserver`, we will be asked to provide a
  password. Make sure this password is strong enough.** Moreover, VNC
  communication itself is not encrypted, so you should not send anything
  personal. If you do want to secure it, you can run an SSH tunnel or VPN
  tunnel to protect the VNC communication. 
  
  ```
  Would you like to enter a view-only password (y/n)? n
  ```
  You can select 'n' when asked for view-only password.

- **On your computer:** You need to have a VNC viewer installed
  on your computer, such as [RealVNC](https://www.realvnc.com/en/connect/download/viewer/).
  If you prefer other VNC viewers,
  it is fine. Most of them are compatible with one another.

  Start your VNC viewer program, and type the host name or IP address of the VM, along with
  the port number, such as `pc508.emulab.net:5901`. Most cloud VMs have two
  IP addresses; make sure you use the external IP address, not the internal
  one. You will be prompted for password, which is the one you typed
  when you first run the VNC server. If everything is done correctly,
  you will see the desktop of your remote VM.

- If you VNC does not work, check your firewall to make sure TCP traffics
  to the port `5901` on the VM is allowed. Also check whether your VNC server
  is running properly. Here are some useful commands to help you manage the VNC server on the VM:
  ```
  vncserver -list       # List the VNC server sessions
  vncserver -kill :0    # Kill the session for :0 display
  ```

## Step 2 (Option B): Access the VM Using SSH

(You should already have the access to the VM's SSH according to previous slides, so you can skip this)

To run VNC, you need to have reasonable bandwidth. If your VNC performance
is bad, you should switch to SSH. You can get by with many of the
SEED labs using just terminals. There are many ways to SSH into the
cloud VM:

- Most cloud platforms provide a default browser-based SSH client.
  Google cloud's SSH client even allows you to upload and download files,
  which is very convenient.

- You can also find many third-party SSH clients. Some clouds may have
  disabled the password authentication in SSH, so you have to use
  public keys for the authentication.
  You need to generate public/private key pairs on your SSH client machine,
  and save the public key into the `/home/seed/.ssh/authorized_keys` file on
  the server machine. You can easily find
  instructions from online resources, so we will not provide one here.
