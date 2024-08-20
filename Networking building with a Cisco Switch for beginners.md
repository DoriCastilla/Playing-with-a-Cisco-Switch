# Build a Networking with a Cisco Switch by a console cable for beginners
## Steps:
1. Connect to the switch by Console cable
2. Reset the Cisco switch via console without administrative privileges
3. Set the Basic Configuration and Personalized Configuration.
4. Set up SSH to the switch
5. Assign an IP Address to the Management Interface (VLAN 1)
6. Test SSH Access
7. Make a Backup of the Cisco switch configuration (Optional)
## Tools:
- Computer
- Windows 11 
- [PuTTY](https://www.putty.org/)
- Switch Cisco (in this case: Catalyst 2960 series)
- Patch cable (ethernet) 
- Console cable  
- Internet connexion
## Configuration (used in this exercise):
- Hostname: tst-cisco-01
- Domain name: tst.lan 
- Static IP: 192.168.19.144/24, 
- Gateway (gw): 192.168.19.1 
- User identification (uid): ciscouser; psw: show
- Enable secret: ThreeHjul3
- Enable password: AnnieGrass4
- Virtual terminal password: ThreeHjul3

## STEP 1. Connect to the switch by Console cable
To connect to a Cisco device using PuTTY via a serial connection, follow these steps:

Install [PuTTY](https://www.putty.org/)

Connect the serial cable (in this case by USB) from your computer to the Cisco device's console port.

Identify the COM Port of the serial cable: <br>
Go to `Device Manager` > Expand `Ports (COM & LPT)` to see the COM port number (e.g., COM3, COM4).  In this case, it was **COM8**<br>
To identify the COM port, you may need to disconnect it and detect which COM prompted when you connect it again.

![Playing_with_Swtich_1](https://github.com/user-attachments/assets/593276db-783c-4c0c-a997-88befb5446e1)

Configure PuTTY:
- Open PuTTY**.
- Select "Serial" under the "Connection type" section.
- Enter the COM port number identified earlier (in this case COM8)) in the "Serial line" field.
- Set the speed (baud rate) to `9600` (standard for Cisco devices).
- Leave the other settings at their defaults:
   - Data Bits: 8
   - Stop Bits: 1
   - Parity: None
   - Flow Control: None
  
![Playing_with_Swtich_2](https://github.com/user-attachments/assets/9ca3a08a-0c2b-4f66-83c1-618aa4abca6b)   ![Playing_with_Swtich_3](https://github.com/user-attachments/assets/1ff2cf2e-e96f-4cb5-9f9b-3aceabad899d)

Open the Connection and log in:
- Click “Open” to start the session.
- If successful, you should see the Cisco device's console prompt in the PuTTY terminal.

## STEP 2. Reset the Cisco switch via console without administrative privileges
You can do it in two different ways: A) Via Mode button, or B) Using the CLI (Command Line Interface)

###  A) Reset the Cisco switch via console without administrative privileges via Mode button
Power off the switch unplugging the power cord.

Press and hold the Mode button in front of the switch. <br>
When holding it, plug the power cord into power on the switch.

Hold the button for 10-15 seconds until the lights turn from amber to solid green. Then release the button.
Open the Command prompt and see how it connects.

Next, we need to delete the configuration to install our own one:

Initialize the flash file system writing: 

``` 
flash_init
```

![Playing_with_Swtich_4](https://github.com/user-attachments/assets/4fa8129f-243b-4917-9fa4-e213d3b1af71)

Delete the current configuration file:

Go to the flash file:
```
 dir flash
```

Look for the configuration file, typically named `config.text` or similar, and delete it:
```
del flash:config.text
```
(In this case, I did also delete vlan.dat using the command: del flash:vlan.dat )

![Playing_with_Swtich_5](https://github.com/user-attachments/assets/64150aff-8d6c-4694-ad47-dbc3ce3840d8)

Reboot the switch:
```
boot
```

After the switch reboots, you may be prompted to enter the initial configuration dialog. Skip it typing:
```
no
```

The switch will reboot with the factory default settings, and the previous configuration (including any passwords or VLANs) will be erased.

###  B) Reset the Cisco switch via console without administrative privileges using the CLI
You are connected to the switch using the console cable via PuTTY (Step 1). 

In the command prompt, change to privileged mode: 
```
enable
```
The symbol > will change to #  ![Playing_with_Swtich_6](https://github.com/user-attachments/assets/f8c5eb57-9042-440f-8fec-b761b0c9737a)

Erase the startup configuration:
```
erase
```
Reload the switch to apply the reset:
```
reload
```
When prompted to save the configuration:
```
no
```
After the switch reboots, you may be prompted to enter the initial configuration dialog. Skip it: 
```
no
```
The switch will reboot with the factory default settings, and the previous configuration (including any passwords or VLANs) will be erased.

## STEP 3. Set the Basic Configuration and Personalized Configuration.
It will ask for to enter the initial configuration dialog: 
```
yes
```
 Next, will ask to enter the basic management setup:  
```
yes
```
Enter your personalized information. In this case: 
Hostname: 
```
tst-cisco-01
```
Enable secret: 
```
ThreeHjul3
```  
Enable password: 
```
AnnieGrass4
```
Enter virtual terminal password: 
```
ThreeHjul3
```
It will ask for configure SNMP network management: 
```
no
```
or just `enter` (no is the default answer).

When it prompts the list of interfaces for VLan1 will ask you for the interface name used to connect to the management network, select one, and copy-paste it. In this case, I did select FastEthernet0/8

![Playing_with_Swtich_8](https://github.com/user-attachments/assets/c169ced4-1241-4f32-9c0c-d346a870e8d0)

## STEP 4. Set up SSH to the switch
You are in privileged exec mode (Step 2)

We are now going to enter global configuration mode:
```
configure terminal
```
Set the Domain name, required for generating SSH keys with the command: ip domain-name yourdomain.com

In this case: 
```
ip domain-name tst.lan
```
SSH requires RSA keys for encryption. Generate them using the following command:
```
crypto key generate rsa
```
When prompted for the key size, choose a key size of at least 1024 bits: *How many bits in the modulus [512]:* <br>
A common choice is 2048 bits for better security, but in this case, I just pressed enter to set 512, as the default size.

Create a local User Account for SHH access and configure User Authentication: 

Create the local user account by typing: username admin privilege 15 secret YourPassword (Replace admin with your desired username and YourPassword with a secure password.): 
In this case:
```
username ciscouser privilege 15 secret ThreeHjul3
```
Configure the virtual terminal (VTY) lines to accept SSH connections:
```
line vty 0 15
transport input ssh
login local
exit
```
![Playing_with_Swtich_9](https://github.com/user-attachments/assets/b153caaf-8eef-4af5-912b-332b093f0819)

Configure SSH Version setting SSH version 2:
```
ip ssh version 2
```

## STEP 5. Assign an IP Address to the Management Interface (VLAN 1)
Configure the IP address by assigning an IP address to VLAN 1 or another management VLAN by typing the commands:<br>
*interface vlan 1<br>
ip address 192.168.1.10 255.255.255.0<br>
no shutdown<br>
exit<br>*
Replacing 192.168.1.10 and 255.255.255.0 with the appropriate IP address and subnet mask.

In this case: 
```
interface vlan 1
ip address 192.168.19.144 255.255.255.0
no shutdown
exit
```
![Playing_with_Swtich_10](https://github.com/user-attachments/assets/0c538b76-2e4f-45d1-960b-c255c60be2a8)

Save your configuration ensuring that your changes are saved and they persist after a reboot: 
```
copy running-config startup-config
```
And when it asks, press `Enter` (OK is the default answer).

![Playing_with_Swtich_11](https://github.com/user-attachments/assets/6fb62870-8dbc-46d4-8e6e-270c15f58c86)

Now, you can disconnect the console cable from the Cisco switch and your computer, the configuration part is done. 

## STEP 6. Test SSH Access
Connect the switch to the rutter by the patch cable and be sure it is turned on.

Go back to PuTTY and open a new session by selecting:
Host name (or IP address): The IP you set, 
in this case:
```
192.168.19.144
```
Connection type: SSH        
![Playing_with_Swtich_12](https://github.com/user-attachments/assets/34ca436c-af6f-4185-ae88-d8b0cd52a5d1)

And your network is working! 

## (Optional) STEP 7. Make a Backup of the Cisco switch configuration
Connect to the switch by PuTTY. For it, you need:
- IP of the switch
- Username
- Password

Go to administration role: 
```
enable
```
Access to the switch configuration:
```
show configuration
```
Select with the mouse all the settings and copy them. <br>
Paste and save them into a Notepad file.

Leave the configuration: 
```
exit
```
Back to user role:
```
exit
```






