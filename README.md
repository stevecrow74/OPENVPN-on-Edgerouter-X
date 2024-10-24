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
and in VLan tab deselect 2,3,4.
save
edit eth2 and add manual address 192.168.3.1/24
save



3: DNS

go to the Services Tab and into DNS tab.
remove switch
add eth2 
save



4: WinSCP

you need to edit a few lines to the .ovpn file
below client add the line:
route-nopull
then add to the line 
auth-user-pass /config/auth/vpn.txt  (make sure you poing to the file as it is case sensative)
add the .ovpn file to the /config/ directory
and the password .txt file to the /config/auth/ directory
the text file shoule have two lines that you use to login to your vpn service.
"your username"
"your password"



5: SSH

ssh into the router either with Putty or CLi on the Gui interface.
type: 

configure
set interfaces openvpn vtun0 config-file /config/VPN.ovpn
commit
set protocols static table 1 interface-route 0.0.0.0/0 next-hop-interface vtun0
set firewall modify OPENVPN_ROUTE rule 10 description 'traffic from IoT to vtun0'
set firewall modify OPENVPN_ROUTE rule 10 source address 192.168.3.0/24
set firewall modify OPENVPN_ROUTE rule 10 modify table 1
set interfaces ethernet eth2 firewall in modify OPENVPN_ROUTE
set interfaces ethernet eth3 firewall in modify OPENVPN_ROUTE
set interfaces ethernet eth4 firewall in modify OPENVPN_ROUTE
commit
save



6: finish up

In the Gui, select firewall/Nat tab
then select Nat tab
Add Source NAT Rule
edit description "masquerade for VPN"
Outbound Interface- vtun0
Src Address- 192.168.3.0/24
save




There are Firewall rules I have left out, The above will get one port eth1 as a seperate network (handy for a wireless hub) 
andone port eth2 that will run through an OpenVPN connection.

Also to note, some VPN service providers need a .crt file to work with OpenVPN, if it requires this file add it to the /config/directory in step4.


This works for me and I hope you have the same success.

for more detail see https://www.youtube.com/@gregpakes5253
