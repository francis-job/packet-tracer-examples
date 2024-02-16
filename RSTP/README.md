This text file describe How you can setup and test STP and RSTP network protocols.

Primarily we need the Cisco packet tracer to be installed in your PC,

it can be downloaded from here : https://skillsforall.com/resources/lab-downloads?courseLang=en-US

There is a Free familiarisation course available in the cisco skill for all section for this packet tracer application.

Lets Gets Started, Where we were? Oh!! RSTP Gotcha..

So RSTP is a super cool version of the primitive STP, There are significant port roles and some other stuffs. I don't wanna talk about the whole thing, just because this text file won't be enough.Although I don't have much time for this. so lets begin...

I'm assuming you already configured cisco packet tracer and ready to hit the road..

Setup Requrements..
	1. 3 Switches
	2. 2/3 PC connected to each of the switches (I used a server in one switch and 2 PC in other two, That felt cool)
	3. LAN Cables for connecting each switches togethe
	
	
Procedure:

	Step 1: Place 3 switches like the corners of a triangle and  PC/Server connected to each of these corner (One PC/End device connects only to one switch, common sense)
	Step 2: Connect these switches together, Would look like a triangle if you have some art sense.
	Step 3: Tadaaa... The physical setup has been completed
	Step 4: Assign Manual IP address  to each of the PC/Server as there is no DHCP server running in this setup.
	Step 5: Once the IP assignments are complete, you may see that the network stabilises starts communicating with each other....
	Step 6: You can  start a PING from one of the PC or server to another PC/Server.
	
Now the setup is complete..... is that all??? Naah.... a little bit more to cover..

Now you see that the devices are communicating with each other via this network topology through the switches.. But is this RSTP, How we can test this?

Open CLI in any of the switches run the following command

>enable 
#show spanning-tree  
 
 	if it shows "Spanning tree enabled protocol ieee" it is the primitive STP else "Spanning tree enabled protocol rstp" it is RSTP
 	
 you can change the mode of the switch by using the following commands.
 
 >enable
 #config
 (config)# spanning-tree mode rapid-pvst  // vice-versa..  ///Do this on all switches
 
 
 You can test the protocol by shutting down a port via command or disconnecting the wire
 
 via CLI you can do this like this
 
 >enable
 #config
 (config)#int fa 0/2 // or any other ports
 (config-if)#shutdown // to disable the port
 (config-if)#no shutdown // to enable the port
 
 
 Test try a ping from one device to other and in meantime shutdown the appropriate ports to disable the link. Count the lost ping and also start a timer to check when the redundant link becomes live.
 
 Observation
 -------------
 
 In STP I could see more than 6 ping packet timeouts and 30 Sec to get the redundant link to become live. It is longer...
 In RSTP I could see only one timeout message in the Ping and around 2 to 5 sec to get the redundant link to become live and network stabilise. 
 
 
