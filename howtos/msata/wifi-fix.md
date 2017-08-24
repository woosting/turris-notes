# Pci WiFi fix

> SYMPTOMS: After adding mSata disk the Basic web setting shows 2 wifi interfaces but only the upper one works (5Ghz) the 2.4Ghz one (that got moved) is shown but wifi is gone...

To fix WiFi should one of the WiFi adapters not work after [mounting mSATA disk][1]:

1. Backup the wireless configuration file:

	```
	# cp /etc/config/wireless /etc/config/wireless.old
	```

2. Open the file for editing:

	```
	# vim /etc/config/wirelessand
	```

3. Remove the block related to `radio1`.

4. Rename `radio2` to radio1 where it says `radio2` (two places)

5. Save and reboot the Turris Omnia.

## References

> Adapted from: Turis Omnia
> [Small thing to do after adding msata ssd][2]

> Also see: Youtube
> [mSATA disk hardware mount instruction][1]


<!-- REFERENCES -->

[1]:https://www.youtube.com/watch?time_continue=2&v=71_M2N3ga7s

[2]:https://forum.turris.cz/t/info-small-thing-to-do-after-adding-msata-ssd/1831
