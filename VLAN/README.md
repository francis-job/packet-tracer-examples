This document will describe How you can setup and test a collection of VLAN network.

Here we are going to create 3 VLAN network and setup inter-vlan communication between these networks. These network also has to be in a different subnet. Will explain on the way.

Components Required :
--------------------
1. 8 PC ( 3 for VLAN 10, 2 for VLAN 20, 2 for VLAN 30) , you can name  each vlan as you wish.
2. A switch
3. A router
4. LAN Cables to connected each device to each other.

Procedure : 
--------------
For simplicity we can keep the devices intended for a particular vlan together as you see in the example simultaion.

Step 1: After arranging the PC's appropriately, connect each PC to the swith via cable.

Step 2: place the router and make a connection between the router and the switch.

Step 3 : Configure the PC's . Assign IP address, consider dividing them to different subnets.
		
			Network 1 (will be vlan 10 later now only subnet divsion) 
			------------
			PC1 : 192.168.0.2/26 (255.255.255.192)
			PC2 : 192.168.0.3/26 (255.255.255.192)
			PC3 : 192.168.0.4/26 (255.255.255.192) 
			Configure the gateway as 192.168.0.1/26 (255.255.255.192)
			
			Network 2 (will be vlan 20 later now only subnet divsion)
			----------
			PC4 : 192.168.0.66/26 (255.255.255.192)
			PC5 : 192.168.0.67/26 (255.255.255.192)
			Configure the gateway as 192.168.0.65/26 (255.255.255.192)
			
			Network 2 (will be vlan 20 later now only subnet divsion)
			-----------
			PC6 : 192.168.0.130/26 (255.255.255.192)
			PC7 : 192.168.0.131/26 (255.255.255.192)
			PC8 : 192.168.0.132/26 (255.255.255.192)
			Configure the gateway as 192.168.0.129/26 (255.255.255.192)
			
You can check ping between devices in the same network as well as other networks. You could see that ping between devices in the same network becomes successfull and different network becomes failed, that's because of this subnet division. However if you start a ping from any device with destination address as 255.255.255.255 (broadcast message), you can see that it can reach other network also, so the network isolation is not pretty string with this method only. That's were VLAN comes in. VLAN gives a complete isolation to make the network security to a different level.

Step 4 : Configure VLAN's in the switches. You can configure a range of interfaces at once using the following command examples

	> enable
	# configure
	(config)# interface range fa 0/1 - 3  //configuring interface 1 to 3 at once, if you want to configure one port only just remove the range and add only the port number (fa0/0)
	(config-if)# switch-port mode access //configuring them as access ports.
	(config-if)# switch-port access vlan 10 //adding these access port to vlan10, if vlan10 is not yet created, it will create a new vlan 10. So no worries.
	 
	 Now repeat the steps for other ports and vlans too. You don't have to exit and go back to the config option, do everything from here just switch to the next interface configuration using 
	 "interface range fa 0/4 - 5". Just like that.
	
Now VLAN's are configured, You can check the status by the following command

	#show vlan brief // from the enable screen
	
	if you are on any other mode you can put a do infront of the command and know the details.
	
Step 5 : Configure trunk port in the switch, trunk port carries packets from different VLAN's. here fa0/9 is the port in switch connecting the router. So follow the command examples to configure the trunk port. 
	
	> enable
	# configure
	(config)# interface fa 0/9
	(config-if)# switch-port mode trunk
	
	You can view the trunk port config using the following command
	
	#show interface trunk
	
	here if you don't see anything even after configuring the trunk port, make sure that the link is UP.ie the other end is also UP.

Step 6: Configure the Router. We have to configure the router for inter-vlan communication.So we have to configure the sub interfaces as the gateway of each vlan's. Here is how you do it.
	Here the switch is connected to the router via fa0/0 port. the subinterfaces will be fa0/0.10, fa0/0.20, fa0/0.30 .

	//configuring sub interface for vlan 10
	
	> enable
	# configure
	(config)# interface fa 0/0
	(config-if)# interface fa 0/0.10
	(config-subif)# encapsulation dot1Q 10
	(config-subif)# ip address 192.168.0.1 255.255.255.192
	
	jump to other sub-interface in this mode using the "interface fa 0/0.20" command (change the sub-if number appropriately)
	
	After setting up all the sub interface you can view the configuration using
	
	#show ip interface brief
	
	
	Here if you see the sub-interface and main interfaces are down, just push "no shutdown" command from the fa0/0 interface config mode. then check again. Hopefully you can see the trunk port becomes active in the switch.
	
	
Now try ping (broadcast as well as unicast) between same VLAN and different VLAN. Try the simulation mode to view how the packets are being sent and recieved and their path of travel.


Hope you enjoyed!!!!!!...  
	

