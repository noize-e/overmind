# Understanding & Managing Network Interfaces In Ubuntu

   A network interface card is installed on every computer so that it can connect to a network. Without a network card, you can't use the network.

   If your server's hardware has been properly configured, you should have one or more network interfaces available for you to use.

   You can view the information regarding the interfaces in your server using the IP command, also, you can manage your interfaces with the IP command.

   For example, to show your currently assigned IP address, you can either use ip aor ip addr show:


To bring a device down, you use:
sudo ip link set enp0s3 down

   This removes its IP assignment and prevents it from connecting to networks or the internet.

   To bring up a device, you use:
sudo ip link set enp0s3 up

   This brings up the network and allows connecting back to the internet, this is the same as toggling the state of the interface enp0s3; on & off, you get the idea!

   If you are used to Ubuntu 16 and below, you will notice the naming convention is a little bit different, in Ubuntu 16, the network interface names are eth0, wlan0, and the likes.

   The issue with that naming convention is that the names aren't predictable and aren't persistent between boots.

   For example, if you add a second network card (which then becomes eth1; eth0 are the first network card, so, if you add a second one, it becomes eth1), it is likely your configuration breaks if
   names were to get switched during the next boot, this happens to me a lot, and it wasn't a nice thing at all.

   To solve this issue, you use the udev to make the names stick, this way, the order won't get changed during the next boot. While you don't need this anymore in new the new version of Ubuntu
   (18.04 and above), it doesn't hurt to know how it works.

   To do this, you open up the following configuration file:
/etc/udev/rules.d/70-persistent-net-rules

## veth interface

Certainly veth are virtual ethernet interfaces used for providing an internet connection to virtual machines.

to shut down the drivers for these interface use
ifconfig vethxxxxxx down

       Also, you can do it by using ip command.
ip link set $veth down

       But since there are too many of them, use for loop like this :
for veth in $(ifconfig | grep "^veth" | cut -d' ' -f1)
do
    ifconfig $veth down    # OR this ip link set $veth down
done

     * Here's one-liner for copy paste :
for veth in $(ifconfig | grep "^veth" | cut -d  ' ' -f1); do ifconfig $veth down; done


>> BTW, I'd recommend using ip over the older ifconfig - ip can make use of network features of the kernel ifconfig can't use, though it's not important for this particular problem.

Bring veth Interfaces down

for veth in $(ip a | grep veth | cut -d' ' -f 2 | rev | cut -c2- | rev | cut -d '@' -f 1 )
do
    sudo ip link set $veth down
done

Delete veth Interfaces

for veth in $(ip a | grep veth | cut -d' ' -f 2 | rev | cut -c2- | rev | cut -d '@' -f 1 )
do
    sudo ip link delete $veth
done
