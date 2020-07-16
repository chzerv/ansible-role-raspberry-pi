# Ansible Role: Raspberry Pi

This role configures a Raspberry Pi running Raspbian (stretch, buster) or Ubuntu (19.04+). Although it's named "raspberry-pi", tt can be probably used to setup any Debian-based installation.

The configuration includes:

- Configure the locale/timezone/hostname.
- Change the default password.
- Create new users and set their passwords.
- Install extra packages.
- Install, customize and enable [log2ram](https://github.com/azlux/log2ram).
- Setup a static IP.

## Requirements

A Raspberry Pi, with Raspbian or Ubuntu _already_ installed.

## Role Variables

Available variables are listed below, along with the default values. For more information, see `defaults/main.yml`.

|                Name                 |    Default Value    |                                            Description                                            |
| :---------------------------------: | :-----------------: | :-----------------------------------------------------------------------------------------------: |
|         raspberry_pi_locale         |     en_US.UTF-8     |         The locale to be used. Use `cat /etc/locale.gen` for a list of available locales.         |
|        raspberry_pi_timezone        |    Europe/Athens    |     The timezone to be used. Use `ls /usr/share/zoneinfo` for a list of available timezones.      |
|        raspberry_pi_hostname        |     raspberrypi     |                                    The hostname of the device.                                    |
|      raspberry_pi_default_user      |          -          |                                 (Required )The default username.                                  |
| raspberry_pi_default_user_password  |          -          |                     (Optional) A password for the default user defined above.                     |
|        raspberry_pi_new_user        |          -          |                         (Optional) A new user to be added to the system.                          |
|   raspberry_pi_new_user_password    |          -          |                          (Optional) Provide a password for the new user.                          |
| raspberry_pi_new_user_primary_group |        users        |                                The primary group of the new user.                                 |
|    raspberry_pi_new_user_groups     |          -          |        (Optional) A comma separated list of the groups that the new user will be part of.         |
|     raspberry_pi_enable_log2ram     |        true         |                                 Whether to enable log2ram or not.                                 |
|      raspberry_pi_log2ram_size      |         40M         | The ramdisk size. Needs a value > 40 in case of `/var/log.hdd/ doesn't exist! Can't sync.` error. |
|   raspberry_pi_log2ram_use_rsync    |        false        |                              Whether to use `rsync` instead of `cp`.                              |
|      raspberry_pi_log2ram_mail      |        false        |                     Disable the error system mail if there is not enough RAM.                     |
|   raspberry_pi_log2ram_path_disk    |      /var/log       |                                     Where the logs are saved.                                     |
|      raspberry_pi_log2ram_zl2r      |        false        |     Disable zram compatibility. zram must be enable on the Pi before you set this to `true`.      |
|    raspberry_pi_log2ram_comp_alg    |         lz4         |                                    The compression algorithm.                                     |
| raspberry_pi_log2ram_log_disk_size  |        100M         |                                     The max size of the logs.                                     |
|        raspberry_pi_packages        | apt-transport-https |                          A list of additional packages to be installed.                           |
|     raspberry_pi_set_static_ip      |        false        |                                Whether to set a static IP or not.                                 |
|   raspberry_pi_static_ip_address    |          -          |                           (Required) The IP to be used as a static IP.                            |
|   raspberry_pi_static_ip_gateway    |          -          |                                  (Required) The gateway address.                                  |
|   raspberry_pi_static_ip_netmask    |    255.255.255.0    |                                      (Required) The netmask.                                      |
|     raspberry_pi_static_ip_dns1     |          -          |                                 (Optional) Static DNS to be used.                                 |
|     raspberry_pi_static_ip_dns2     |          -          |                                (Optional) Fallback DNS to be used.                                |

### Notes

- `apt-transport-https` is required, and therefore installed by default, because I have setup the template for the `log2ram` repository to use HTTPS.
- [log2ram](https://github.com/azlux/log2ram) helps extend the SD Card's life, by writing logs in the RAM instead of the SD Card.
  If you're using an SSD/HDD instead of a SD Card it's probably unnecessary.
- The device WILL reboot after installing `log2ram`, as per the project's README.
- For Raspbian, the default groups are: `adm,dialout,audio,video,plugdev,input,netdev,gpio,i2c,spi,sudo`
- For Ubuntu, the default groups are: `adm,dialout,cdrom,floppy,audio,video,plugdev,netdev,dip,lxd,sudo`
- For setting a static IP, the variables tagged as "required" are only required if `raspberry_pi_set_static_ip` is set to `true`.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: server
  vars_files:
    - vars/main.yml

  roles:
    - { role: chzerv.ansible_role_raspberry-pi }
```

_Inside `vars/main.yml`_:

```yaml
raspberry_pi_default_user: "ubuntu"
raspberry_pi_default_user_password: "foobar"
raspberry_pi_default_user_sudo_passwd: true

raspberry_pi_enable_log2ram: true
raspberry_pi_log2ram_size: "50M"

raspberry_pi_packages:
  - apt-transport-https
  - curl
  - git
  - lm-sensors
  - vim
  - cowsay
```

## License

MIT / BSD

## Author Information

Xristos Zervakis
