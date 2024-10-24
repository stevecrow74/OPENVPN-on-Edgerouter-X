# OPENVPN-on-Edgerouter-X
setting up OpenVPN on an Ubiquiti edgerouter X 5 port hub

It took me a while to figure out how to get a Vpn running on my edgerouter X,
I mainly use it for the Smart TV and PC nothing else goes through it,  so 
I wanted to set one port with a Vpn for the tv so i could watch things on the BBC iPlayer.

As I said it took me a while to figure it out, and the Webpage that was supposed to explain it no longer exists, However the youtube videos (7) that accompanied the page still exists, so many thanks to Greg Pakes youtube channel @gregpakes5253 for the information provided.

Here's my summary of what to do:

1: Setup
do a basic setup in wizard Wan+2Lan2
set second lan to 192.168.4.1
and Lan (switch) to 192.168.3.1
save
reboot

2: Remove switch
Select switch and set no address
and in VLan tab remove deselect 2,3,4.
save
edit eth2 and add manual address 192.168.3.1/24
save

3: DNS
go to the Services Tab and into DNS tab.
remove switch
add eth2 
save

4: SSH
ssh into the router either with Putty or CLi on the Gui interface.
type: 
configure
set protocols static table1 interface route 0.0.0.0 nexthop interface vtun0
