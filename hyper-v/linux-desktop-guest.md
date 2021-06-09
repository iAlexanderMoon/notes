Enable Hyper-V

Full desktop development environments
---------------------------------------------------------------------------------------
xubuntu minimal

Created VM "development"
	Gen2 UEFI 
	Processors:	2
	RAM:		16384
	Disabled Secure boot
	


Installed xubuntu minimal

How to get hyper-v guest extensions

Hyper-V integration services should be built into ubuntu already

in powershell

get-vmintegrationservice -VMName "development"

	Showed Guest services false

Enabled through VM settings in the Hyper-V gui

> lsmod | grep hv look for 
	hv_vmbus
	hv_storvsc
	hv_blkvsc
	hv_netvsc


apt install vim 

https://www.nakivo.com/blog/run-linux-hyper-v/
turned off quiet splash and turned off io disk scheduler 

> vim /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX_DEFAULT="elevator=noop"
> update-grub2
> init 6

Changed screen resolution from default 1024x768 to 1920x1080
> vim /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="... video=hyperv_fb:1920x1080"
> update-grub2
> init 6


Copy and paste from hyper-v
Guest Services is enabled.

systemctl status hv-fcopy-daemon.service

Was this necessary?
apt install linux-virtual 
apt install linux-cloud-tools-virtual 
apt install linux-tools-virtual


Enhanced Session:
https://francescotonini.medium.com/how-to-install-ubuntu-20-04-on-hyper-v-with-enhanced-session-b20a269a5fa7
Don't enable auto-login or enhanced session won't work.
https://raw.githubusercontent.com/Microsoft/linux-vm-tools/master/ubuntu/18.04/install.sh
https://raw.githubusercontent.com/Hinara/linux-vm-tools/ubuntu20-04/ubuntu/20.04/install.sh 

/etc/xrdp/xrdp.ini



I Turned on enhanced mode but that didn't seem necessary to copy and paste things as I was already doing that with xrdp without doing step 2 and 3.

https://docs.microsoft.com/en-us/answers/questions/138093/hyper-v-advanced-session-doesn39t-work.html
1.	Either use Quick create VM (lot easier but doesn't give you gen2) or install from ubuntu ISO and do XRDP setup
2.	Enable Secure Boot and select template "Microsoft UEFI Certificate Authority" template.. This is in VM Security tab.
3.	From Admin Powershell: Set-VM -vmname "development" -EnhancedSessionTransportType HvSo


Unstalling xRDP using script:  use xrdp
note... you can't be logged into a session through the hyper-v manager to do this.

You should be able to see your windows drives mounted through the rdp connection at the thin_clients folder in your home folder.

Xfce4 feels weird
- How to transport Xfce4 settings

do we really want these from the distribution or from flathub or snap or somewhere else?

apt install vim					cli text editor
apt install mousepad				Simple GUI Text Editor
apt install gedit				Simple GUI Text Editor
apt install evince				PDF Reader

apt install gnome-software			gnome software centre
apt install gnome-software-plugin-flatpak	flatpack support
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

apt install gnome-system-monitor
apt install xfce4-taskmanager
apt install htop

flatpak install flathub com.visualstudio.code


Change xrdp sessions

echo xfce4-session >~/.xsession

or in /etc/xrdp/startwm.sh start it directly
startxfce4 

startwm.sh checks to see if /etc/X11/Xsession is executable and executes it if its or use sh to execute it
it is executable on my installation... what does it do?


https://unix.stackexchange.com/questions/281858/difference-between-xinitrc-xsession-and-xsessionrc

~/.xinitrc is executed by xinit, which is usually invoked via startx. This program is executed after logging in: first you log in on a text console, then you start the GUI with startx. The role of .xinitrc is to start the GUI part of the session, typically by setting some GUI-related settings such as key bindings (with xmodmap or xkbcomp), X resources (with xrdb), etc., and to launch a session manager or a window manager (possibly as part of a desktop environment).
~/.xsessionrc is executed on Debian (and derivatives such as Ubuntu, Linux Mint, etc.) by the X startup scripts on a GUI login, for all session types and (I think) from all display managers. It's also executed from startx if the user doesn't have a .xinitrc, because in that case startx falls back on the same session startup scripts that are used for GUI login. It's executed relatively early, after loading resources but before starting any program such as a key agent, a D-Bus daemon, etc. It typically sets variables that can be used by later startup scripts. It doesn't have any official documentation that I know of, you have to dig into the source to see what works.

.xprofile is very similar to .xsessionrc, but it's part of the session startup script some display managers including GDM (the GNOME display manager) and lightdm, but not others such as xdm and kdm.

https://forum.xfce.org/viewtopic.php?id=14108

sudo update-alternatives --config x-session-manager

* this writes a symlink in /etc/alternatives x-session-manager -> /usr/bin/x-session-manager

https://thunderboltlaptop.com/install-xrdp-ubuntu/#Switching_Between_Desktop_Environments_for_xRDP


Export xfce4 settings:
cp -r ~/.config/Thunar 
cp -r ~/.config/xfce4

xfconfd
When Xfce is first started for a user, it looks into ~/.config/xfce4 for config files. If it doesn't find them, it copies from XDG_CONFIG_DIRS path to the user's ~/.config/xfce4 directory
Any changes to XDG_CONFIG_DIRS: /etc/xdg or /etc/xlinux/xdg won't be reflected to the individual user unless they remove .config/xfce4


Ubuntu 20.04 Ubuntu Desktop with resource monitor running 1.2 GB Ram
When installed through hyper-v options it looks like it gets a special vhdx that is stored in c:/Users/Public/Public Documents/Hyper-V/
using 20.04 means using old firefox etc from repos... I don't like that really.

---------------------------------------------------------------------------------------
PopOS 20.10
Gen2 UEFI 
Processors:	2
RAM:		8194
Disabled Secure boot



	

