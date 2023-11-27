* Identify the current PCI controller
* Identify the PSID and installed software
* Determine the PSID for switch model running Cumulus Linux
* Locate the firmware in the local repository
* Install the firmware and PSID
* Reboot the switch to apply new firmware

1.Begin by determining the PSID and firmware installed on the switch. Run the “lspci” command to see the active PCI controllers running on the switch.  You can filter to list only Mellanox controllers with “lspci | grep Mellanox”.

Use the flint command to query the firmware. 
flint -d 01:00.0 q


Refer to the PSID table in the section above to determine if the right PSID and firmware has been installed. You can check the switch type and model with “net show system” command.

2.Find the firmware which contains the Cumulus Linux PSID. The firmware files  are located at the following path: 
/usr/share/mellanox/firmware/5.10.0-cl-1-amd64/Spectrum/


Use the following command to find the Cumulus PSID: 
-Ensure the path is correct per version of OS
for i in `ls /usr/share/mellanox/firmware/5.10.0-cl-1-amd64/Spectrum/*MSN2100*` ; do flint -i ${i} q | grep -H MT_3000113033 && echo $i ; done

3.Check the specific file and re-confirm the firmware image has the correct PSID to burn.
Navigate to the path where the firmware files are located run the flint command
cd /usr/share/mellanox/firmware/5.10.0-cl-1-amd64/Spectrum/


4.Now that the PSID has  been confirmed , proceed to burn in the firmware and update the PSID.
Use the following command: 
sudo flint -d 01:00.0 -i fw-SwitchEN-rel-13_2010_5034-MSN2100-CxxxC_Ax_Bx.bin --allow_psid_change burn

5.The new firmware and corresponding PSID will become active after a reboot. Refer to the image in Figure 49 above. Confirm this with the following command:
sudo flint -d 01:00.0 q

6.Reboot the switch and login after restart. Validate the PSID is correct.

===========

lspci | grep Mel -->give the bus ID 

sudo flint -d "bus-id" q   (PSID --> firmware version)  

find the version that is applicable to cumulus 
usr/share/mellanox/firmware/5.10.0-cl-1-amd64/Spectrum/*MSN2100*
sudo flint -d 01:00.0 -i fw-SwitchEN-rel-13_2010_5034-MSN2100-CxxxC_Ax_Bx.bin --allow_psid_change burn
