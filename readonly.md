
https://medium.com/@andreas.schallwig/how-to-make-your-raspberry-pi-file-system-read-only-raspbian-stretch-80c0f7be7353

blikvm@mangopimcore(rw):~$ cat /etc/fstab
UUID=a990fd36-0857-4856-b0b8-5c84a7df516f / ext4 ro,noatime,commit=600,errors=remount-ro 0 1
tmpfs /tmp tmpfs defaults,nosuid 0 0
/dev/mmcblk0p3  /mnt            ext4    defaults        0       0
tmpfs        /var/log        tmpfs   nosuid,nodev         0       0
tmpfs        /var/tmp        tmpfs   nosuid,nodev         0       0
tmpfs        /var/blikvm        tmpfs   nosuid,nodev         0       0
