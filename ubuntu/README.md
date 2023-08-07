# Ubuntu Applications and Configs

## Extensions

- [Clipboard Indicator by Tudmotu](https://extensions.gnome.org/extension/779/clipboard-indicator/)

- ~~[cpufreq by konkor](https://extensions.gnome.org/extension/1082/cpufreq/)~~


- [Resource Monitor by 0ry0n](https://extensions.gnome.org/extension/1634/resource-monitor/)

- ~~[Sensory Perception by HarlemSquirrel](https://extensions.gnome.org/extension/1145/sensory-perception/)~~


- [Status Area Horizontal Spacing by p91paul](https://extensions.gnome.org/extension/355/status-area-horizontal-spacing/)

- [Sound Input & Output Device Chooser by kgshank](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)

- ~~[Transparent Top Bar (Adjustable transparency) by Gonzague](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)~~

- [Rounded Window Corners by Luo Yi](https://extensions.gnome.org/extension/5237/rounded-window-corners/)

- [Hibernate Status Button by p91paul](https://extensions.gnome.org/extension/755/hibernate-status-button/)


- [Add Username to Top Panel by brendaw](https://extensions.gnome.org/extension/1108/add-username-to-top-panel/)

- [Blur my Shell by aunetx](https://extensions.gnome.org/extension/3193/blur-my-shell/)

- [Workspace indicator by tty2](https://extensions.gnome.org/extension/3952/workspace-indicator/)

- [Search Light by icedman](https://extensions.gnome.org/extension/5489/search-light/)

- [GSConnect by dlandau](https://extensions.gnome.org/extension/1319/gsconnect/)

## Create Swap Partition after Installation

First create a `linux-swap` partition from Gparted.

To create a swap partition after installation, create an empty partition. It should have no holes. You can then format this partition with:

`sudo mkswap /dev/sdX`
replacing `/dev/sdX` with your partition. Mount this partition as swap with

`sudo swapon -U UUID`
where UUID is that of your `/dev/sdX` as read from this:

`blkid /dev/sdX`
Bind your new swap in `/etc/fstab` by adding this line:

`UUID=xxx    none    swap    sw      0   0`

If you want to use your swap for hibernating then you need to update the UUID in `/etc/initramfs-tools/conf.d/resume` with this content `RESUME=UUID=xxx`. Don't forget to `$ sudo update-initramfs -u`.

I assume you have a swap partition ready to use (if you have a swap file you cannot hibernate). Follow these steps:

1. Install pm-utils and hibernate:

    `sudo apt install pm-utils hibernate`

2. Then:

    `cat /sys/power/state`

3. You should see:

    `freeze mem disk`

4. Then run one of the following lines:

    ```
    grep swap /etc/fstab
    blkid | grep swap
    ```
5. Copy the UUID value. You will need it later.

6. Then run (use your favorite editor if not nano):

    `sudo nano /etc/default/grub`

7. Change the line that says:

    `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`
    so that it instead says:

    `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash resume=UUID=<YOUR_COPIED_UUID>"`
    Be careful not to miss the `UUID=` part.

8. Then, after saving the file and quitting the text editor, run:

    `sudo update-grub`

9. To test it, run:

    `sudo systemctl hibernate`

### Adding the Hibernate Option in the System Tray Power Off/Log Out Menu of Ubuntu 22.04 LTS

- To do that, create a new file which is com.ubuntu.enable-hibernate.pkla in the /etc/polkit-1/localauthority/50-local.d/ directory and open it with the “gedit” text editor as follows:

    `sudo gedit /etc/polkit-1/localauthority/50-local.d/com.ubuntu.enable-hibernate.pkla`

    Type in the following lines of codes in the com.ubuntu.enable-hibernate.pkla file:

    ```
    [Re-enable hibernate by default in upower]

    Identity=unix-user:*

    Action=org.freedesktop.upower.hibernate

    ResultActive=yes

    [Re-enable hibernate by default in logind]

    Identity=unix-user:*

    Action=org.freedesktop.login1.hibernate;org.freedesktop.login1.handle-hibernate-key;org.freedesktop.login1;org.freedesktop.login1.hibernate-multiple-sessions;org.freedesktop.login1.hibernate-ignore-inhibit

    ResultActive=yes
    ```

- Once you’re done, save the file by pressing <Ctrl> + S.

- Now, update the APT package repository cache with the following command:
`$ sudo apt update`

- Install the GNOME Extension Manager app with the following command:
`$ sudo apt install gnome-shell-extension-manager`

- Then download **Hibernate Status Button** from the extensions list and we're all set :)

# Getting My Preferred Color (Color Accuracy)

- Download `sudo apt-get install gnome-color-manager` 

- **Go to Settings -> Color -> Laptop Screen -> Add Profile -> Select Standard Space - sRGB**

- Night Light

    ![Color Temp](./assets/color-temp.png)

### Fix Bootloader
-  https://help.ubuntu.com/community/Boot-Repair
