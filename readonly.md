
https://medium.com/@andreas.schallwig/how-to-make-your-raspberry-pi-file-system-read-only-raspbian-stretch-80c0f7be7353
```
blikvm@mangopimcore(rw):~$ cat /etc/fstab
UUID=a990fd36-0857-4856-b0b8-5c84a7df516f / ext4 ro,noatime,commit=600,errors=remount-ro 0 1
tmpfs /tmp tmpfs defaults,nosuid 0 0
/dev/mmcblk0p3  /mnt            ext4    defaults        0       0
tmpfs        /var/log        tmpfs   nosuid,nodev         0       0
tmpfs        /var/tmp        tmpfs   nosuid,nodev         0       0
tmpfs        /var/blikvm        tmpfs   nosuid,nodev         0       0
```

Edit the file /etc/bash.bashrc and add the following lines at the end:
```
set_bash_prompt() {
fs_mode=$(mount | sed -n -e "s/^\/dev\/.* on \/ .*(\(r[w|o]\).*/\1/p")
PS1='\[\033[01;32m\]\u@\h${fs_mode:+($fs_mode)}\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
}
alias ro='sudo mount -o remount,ro / ; sudo mount -o remount,ro /boot'
alias rw='sudo mount -o remount,rw / ; sudo mount -o remount,rw /boot'
PROMPT_COMMAND=set_bash_prompt
```
Finally ensure the file system goes back to read-only once you log out. Edit (or create) the file /etc/bash.bash_logout and add the following lines at the end:
```
mount -o remount,ro /
mount -o remount,ro /boot
```
