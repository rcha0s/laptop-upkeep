# MacOS exFAT detection issue

- `diskutil list` - Check all the mounted/unmounted disks
- `sudo diskutil mount /dev/disk2s1` or  `sudo diskutil unmount /dev/disk2s1` - try mounting or unmounting the disk
- See if Disk Repair option in Disk Utility works
- `sudo fsck_exfat -d /dev/disk2s1` - Use manual exfat detection. Long run time depending on the filesystem size. You can stop the long process in between and it still works.
   - If `Resource is busy` check the process using it with `ps -ax | grep disk2` and kill it.
- If fsck is holding the disk hostage, kill it with `sudo pkill -f fsck`
