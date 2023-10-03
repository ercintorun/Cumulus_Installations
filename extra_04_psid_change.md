/usr/share/mellanox/firmware/5.10.0-cl-1-amd64/Spectrum/

lspci | grep Mel -->give the bus ID 

sudo flint -d "bus-id" q   (PSID --> firmware version)  

find the version that is applicable to cumulus 
usr/share/mellanox/firmware/5.10.0-cl-1-amd64/Spectrum/*MSN2100*
sudo flint -d 01:00.0 -i fw-SwitchEN-rel-13_2010_5034-MSN2100-CxxxC_Ax_Bx.bin --allow_psid_change burn
