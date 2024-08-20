# Build a Networking with a Cisco Switch by a console cable for begynners
## Steps:
1. Connect to the switch by Console cable
2. Give switch ip and gateway
3. Create Network ssh access to switch
4.  Connect switch to ap
5.  Connect to switch via LAN  
## Tools:
- Computer
- Windows 11 
- [PuTTY](https://www.putty.org/)
- Switch
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





