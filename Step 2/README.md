# Create a cusom NAT network for the virtual domain
- For this virtual network, we want the machines inside the network to be able to communicate with each other, and that they have access to the Internet
- As a private network, the machines are isolated from the physical network and would not interfere with your home router's DHCP and DNS services. Configuring NAT for this network would allow the machines to access the Internet through the host IP
- It is recommended that a new network is created instead of modifying existing ones

1. Navigate to /Library/Preferences/VMware\ Fusion
- This is the path for MacOS VMware Fusion. The path should be different for Windows and Linux

2. Modify "networking" file with vim
- First, create a copy of the networking file and store it safely
- Use "sudo vi networking" to start modifying the file
- We shoudln't touch any of the existing information, instead, simply add these lines:
```
answer VNET_2_DHCP no
answer VNET_2_HOSTONLY_NETMASK 255.255.255.0
answer VNET_2_HOSTONLY_SUBNET 10.0.0.0
answer VNET_2_NAT yes
answer VNET_2_VIRTUAL_ADAPTER yes
``` 
- These lines would create a new network called vnet2 that has no DHCP (since we will be using our own DHCP server) and has NAT service. Subnet and netmask options are purely my own choice, so feel free to change them as you see fit.
- Press *ESC* and then ```:wq:``` then Enter to save and exit vim

3. Restart VMware Fusion networking service
```
sudo "/Applications/VMware Fusion.app/Contents/Library/vmnet-cli" --configure
sudo "/Applications/VMware Fusion.app/Contents/Library/vmnet-cli" --stop
sudo "/Applications/VMware Fusion.app/Contents/Library/vmnet-cli" --start
```
- These commands reconfigure the networking service of VMware Fusion and will generate vnet2 folder in ```/Library/Preferences/VMware\ Fusion``` that will contain NAT config of the network
- The other two commands restarts the service
- Use command ```sudo "/Applications/VMware Fusion.app/Contents/Library/vmnet-cli" --status``` to make sure vnet2 NAT service is running

4. Verify NAT config file
- Navigate to ```/Library/Preferences/VMware\ Fusion/vmnet2``` and make sure that file nat.conf exists and is not corrupted
- In ```nat.conf``` file, search for ip address of NAT gateway address as this IP would be important in the next step
