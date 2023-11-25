# Unifi

This was my experiance when migrating my controller from a Raspberry Pi (7.3.83) to a Firewalla Docker container (7.5.176) in November of 2023.

## Migrating Self Hosted Controller to a new Device

1. If using remote access, verify it is working first
2. Download a backup file front the active conroller
3. Install and start the new controller
4. Restore the backup file
5. Verify device show, but are offline
6. Verify controller shows on remote access site - unifi.ui.com
7. On the OLD controller go to "Setting -> System -> General" and click **Export Site** (foller the steps presented)
8. Download the export file, but there is no need to import it, the backup that was restored had everything needed
9. Enter the IP (or DNS name) of the new controller and select the the devices to migrate (all of them) - **Clicking continue will cause a brief outage**
10. Verify all of the devices are visible in the new controller and are online
11. Shutdown the old controller

References
- [Similar process, but keeping the old IP on the new controller](https://community.ui.com/questions/How-to-migrate-UniFi-Controller-from-one-host-to-another/1c4bb6b7-f9b2-4628-8903-5b9a09cc5294#answer/ef467264-86c3-4f49-99f7-9b9af95182dc)
- [Setting up on a Google Compute VM](https://metis.fi/en/2018/02/unifi-on-gcp)

## Migrating to a Docker Container (on a firewalla)

The steps are the same as above, but there are somee [specific instructions for step 3](https://help.firewalla.com/hc/en-us/articles/360053441074-Guide-How-to-run-UniFi-Controller-on-the-Firewalla-Gold-Series-Boxes)

## Unifi Controller - Does not show up on the unifi.ui.com site and reemote access is enabled on the controller

Disable remote access in the UI

Look for issues with the connection to the Unifi site
```
# sudo tail -f /var/log/unifi/server.log
```
Updated the MongoDB to remove the cloud access key
```
# sudo mongo —-port 27117
use ace
db.setting.find({"key" : "super_cloudaccess"}).forEach(printjson)
db.setting.remove({"key" : "super_cloudaccess"})
exit
```
Restart the unifi controller
```
systemctl restart unifi
```

Re-enable remote accesss in the UI