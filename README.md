# 🔴 A "simple" and "straight forward" way to setup a Windows debugging environement:

# 1 - 🧰 REQUIREMENTS

	A target machine able to run Windows, either VM based or physical (in this case I will be using a VM)
	1.1 Turn on BIOS settings (AMD SVM / INTEL VT-x)
	1.2 Install a Hypervisor (such as VMware workstation / Virtualbox / Hyper-V)
	1.3 Install a Windows ISO (idk one that you will need ??)
	- https://www.microsoft.com/en-us/software-download/windows10 < In my case WIN10, just install and choose "Create installation media"
	1.4 Install Windows SDK (ISO for offline install) <- Needed for the Debugging tools [ON HOST and TARGET]
	ALL LINKS CAN BE FOUND AT THE BOTTOM OF THE PAGE/GUIDE !

# 2 - 🎛️ SETUP (TARGET / VM) 
	2.1 Create a virtual machine and choose the WINDOWS ISO, you installed earlier.
	Finish the initial WINDOWS setup.
	(OPTIONAL) I recommend to debloat Windows using tools such as "Win-Debloat-Tools"(by LeDragoX) or "optimizer"(by hellzerg) 
	(IMPORTANT) Highly recommend to setup a way for easy communication between the HOST and TARGET 
	(ex: VMWARE TOOLS:They will let met copy and paste text,files,img etc... between the HOST and TARGET) 
	(ex: Setup a network share or folder share between the HOST and TARGET)

	2.2 Before continuing, the "HOST" is the PC where your going to be using the debugger such as WinDBG,
	"TARGET" is the Windows machine/VM your going to play around with !

# 3 - 🔁 COMMUNICATION HOST<->TARGET
	3.1 Transfer the Windows SDK to the target and start the installation, choose "debugging tools for Windows" in the selection menu.
	
	3.2 Goto "[TARGET] C:\Program Files (x86)\Windows Kits\10\Debuggers\x64 and search for kdnet.exe"
	(You can right-click kdnet > send to > desktop [to have quick access in the future])
	You have multiple choices, connection through COM or Network 
	(Recommended Network due to personal experience with COM being unstable and slow)
	
	3.3 I use NAT for the Network connection (Virtual machine settings > Network adapter > Network connection)
	On HOST and TARGET open CMD: type "whoami", keep the names somewhere. now type ipconfig, 
	search for the Network adapter shared/linked between HOST-TARGET, keep the IPV4 somewhere.
	
	3.4 Now on both HOST-TARGET, go to "Allowed apps through the firewall" 
	search for "File and Printer Sharing" and turn on PUBLIC and PRIVATE on the left side.

	- REBOOT HOST and TARGET
	open cmd on HOST type "ping -4 [TARGET NAME]" < the TARGET NAME we got from "whoami" command (section 3.3) 
	only type in the first part of the name (ex: whoami= desktop-ballzy/GREG | only type in the first part "desktop-ballzy" same for HOST and TARGET)
	you should get a reply from the TARGET, do the same but opposite. in TARGET "ping -4 [HOST NAME]"
	If this does not work, than there is an issue somewhere 🤷‍♂️

	3.5 Boot up the VM,[TARGET] open the CMD as admin (ctrl-alt+enter) 
	type "cd" command to go back into the KDNET path (specified earlier: section 3.2)
	(btw you can right click on the KDNET short-cut we made earlier and click on "Open the file location" to access the folder quicker)
	now you can type in "kdnet.exe [HOST-IPV4 WE GOT EARLIER] [Choose a port (recommended range of 50000-50039)]
	(ex: kdnet.exe 192.168.126.1 50000)
	You should get result like : windbg -k net:port=50000,key=24gmyqeju14fw.35qda53xgubbg.3c61jmqzm878g.1tyu2870jcxdm
	⚠️ Keep it somewhere (even save it like on the desktop HOST/TARGET)
	💾 BTW!!! MAKE A SNAPSHOP OR A SAVE OF THE CURRENT VM, IN CASE SOMETHING BAD HAPPEN ! (❗IT TEND TO HAPPEN❗)
	
 	3.6 Open a debugger (in my case WinDBG)
	In our case you goto "Attach to kernel" in "Net" tab you can enter the port you specified earlier
	And the key that starts after "key="
	EXAMPLE = [ port= 50000 | key= 24gmyqeju14fw.35qda53xgubbg.3c61jmqzm878g.1gyi2370ccxdm ]

Now you can restart the TARGET, you should break on Windows entry.

# 4 - 🧩 TROUBLESHOOT / ADVICES
	Nothing is perfect and we tend to fail, dont worry, on my first attempt I took over 2 hours to setup.
	Be patient check properly the steps we tend to jump over some information at first.
	
	Personally I had a issue with Hamachi network adapter f'king up something so I had to unintall/turn it off !
	
	If you have any issue with getting a response/reply when you ping the HOST or TARGET, check the VM network adapter settings.
	Be sure that you chose the right Network adapter for the kdnet command (section3.3/3.5) in my case VMWare have multiple adapters !
	Be sure that no external anti-virus or firewall is blocking the communication.
	
	If nothing works, use REVOUninstaller and remove the hypervisor and Windows SDK, delete and create a new VM.
	Don't forget to restart your PC between major changes like Windows SDK install etc....

# 5 - ❓ What's the use ?
	If you searched for this guide, you already know but for those who don't.
	Research Windows internals without risking a working Windows System.
	Debugging software, drivers etc...

# 🔗 Useful Links
💡 Microsoft Official Debugging Guide

https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/setting-up-kernel-mode-debugging-in-windbg--cdb--or-ntsd

🖥️ Hyper-V Installation Guide (Requires Windows Pro) <- You can circumvent the requirement of Windows Pro, easy guide on Github 😁

https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/install-hyper-v?pivots=windows#check-requirements-for-windows

📦 VMware Workstation Direct Download Index (The index got removed 🫤 by Broadcom.....)

https://web.archive.org/web/20240906214841/https://softwareupdate.vmware.com/cds/vmw-desktop/ws/17.6.0/24238078/windows/core/VMware-workstation-17.6.0-24238078.exe.tar

https://softwareupdate.vmware.com/cds/vmw-desktop/ws/

📥 Windows SDK

https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/

📥 Windows ISO's

https://www.microsoft.com/en-us/software-download/windows10

https://www.microsoft.com/en-us/software-download/windows11

🧹 Win-Debloat-Tools | 🧼 Optimizer (hellzerg)

https://github.com/LeDragoX/Win-Debloat-Tools

https://github.com/hellzerg/optimizer

TAGS: Windows, Windows 11, Windows 10, HyperV, VMWARE, Debug, VM Debug, KDNET, WinDBG, debug windows, virtualize, virtual machine.
