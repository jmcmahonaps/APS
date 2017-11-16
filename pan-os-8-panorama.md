Change: Upgrade Panorama & Log Collector to Version 8.0.6
Systems Affected: Panorama (10.0.42.xx), Log Collector (10.0.42.xx)

**Important Resource:** https://www.paloaltonetworks.com/content/dam/pan/en_US/assets/pdf/framemaker/80/pan-os/NewFeaturesGuide/section_1.pdf

**Pre-Upgrade Steps**

1. Delete mid-versions of PAN-OS, leaving 7.1.0 and 7.1.8, to free up space
2. Prestage/Download Panorama 8.0 Software (Panorama/Software/Download 8.0) on Panorama
3. Prestage/Download PAN-OS 8 Software on Log Collector
4. Ensure all firewalls are running the latest content release version, or a later version than Panorama.
5. Back up existing Panorama configuration (Panorama/Setup/Operations/Export named configuration snapshot)


**Upgrade Steps: Panorama**

1. Panorama is upgraded first, followed by the log collector. (page 16)
2. Log-in to Panorama, select "Panorama" context. PAN-OS 8.0.2 is already downloaded to Panorama. PAN-OS 8.0.2 must be installed before PAN-OS 8.0.6 can be installed.
3. Allow the installer for PAN-OS 8.0.2 to go through the install process. Reboot Panorama when install is complete to finalize installation.
4. Ensure that Panorama is reachable after installation and still functional.
5. Log-in to Panorama, select "Panorama" context. Click on "download" next to PAN-OS 8.0.6. Repeat steps 3-4 for PAN-OS 8.0.6.

**Upgrade Steps: Log Collector**

1. PAN-OS 8.0.2 is already downloaded to Panorama.
2. Log-in to Panorama. Click on the "Panorama" tab. On the left sidebar, expand "Device Deployment." Click on "Software."
3. Use the search bar to search fo 8.0.2. Click on "install" under "action."
4. Select "Data" from the device list. (Data is the log collector)
5. Check the "Reboot device after install" button, then click "OK" to install.
6. When Data comes back up, log into PANORAMA (NOT DATA) via the CLI.
7. Run the command "request logdb migrate lc serial-number 007307***" to begin the migration of Data's logs to the new PAN-OS 8 log format. This is the serial number for Data. (more info: https://live.paloaltonetworks.com/t5/Community-Blog/Upgrade-Panorama-logs-to-PAN-OS-8-0-FAQ/ba-p/154755)


**Upgrade Considerations/Notes:**


	- Check GlobalProtect Gateways for 8.0 System Requirements/Compatibility Before Upgrading to 8.0 (page 9)
	- "After upgrading, the Panorama virtual appliance remains in Legacy mode by default and can still support NFS log storage. However, after you switch to Panorama mode, the virtual appliance can no longer support NFS storage; you must then migrate the logs on the NFS to the Log Collectors."  (page 13)
	- Upgrades to 8.0 are done through Panorama. For log collector upgrades, click here
	- Close to 100% CPU utilization during the upgrade process is normal. Log migration is throttled in favor of log collection and other processes. (page 17)
