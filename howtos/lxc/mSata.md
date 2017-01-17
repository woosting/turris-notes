After having executed the [Hardware installation instruction video](https://www.youtube.com/watch?time_continue=2&v=71_M2N3ga7s):

Just a note if somebody is adding mSata disk to omnia and finds them self wondering why one of the WiFi adapter is not working suddenly :wink: I know this might be common knowledge but hell, it was not mentioned in the video on CZ youtube... 

Issue:After adding mSata disk as per tutorial video:- Basic web setting shows 2 wifi interfaces but only the upper one is working ( 5Ghz )- 2.4 Ghz ( the one that was moved ) is shown but wifi is gone...

Solution:jump into ssh ( putty ) log into your omnia IP ( 192.168.1.1 ? :smiley: ) with root as user and password.

Backup file: cp /etc/config/wireless /etc/config/wireless.old

Edit file:  vim /etc/config/wirelessand remove block related to radio1, underneath is also radio2 just rename it to radio1 where it says radio2 ( two places ) Save, reboot and enjoy working configuration via web interface again :wink:

> REFERENCE: https://forum.turris.cz/t/info-small-thing-to-do-after-adding-msata-ssd/1831

> REFERENCE: [mSATA disk hardware mount instruction video](https://youtu.be/71_M2N3ga7s)
