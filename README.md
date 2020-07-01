# Ansible Role: Raspberry Pi

This role configures a Raspberry Pi running Raspbian or Ubuntu (ARM).

The configuration includes:

- Configure the locale/timezone/hostname.
- Change the default password.
- Create new users and set their passwords.
- Configure sudoers.
- Install extra packages.
- Install, customize and enable [log2ram](https://github.com/azlux/log2ram).

## Requirements

None.

## Role Variables

Available variables are listed below, along with the default values. For more information, see `defaults/main.yml`.

### Locale, timezone and hostname configuration

```yaml
raspberry_pi_locale: "en_US.UTF-8"
raspberry_pi_timezone: "Europe/Athens"
raspberry_pi_hostname: "raspberrypi"
```

### Packages

- Install extra packages.

  ```yaml
  raspberry_pi_packages:
    - apt-transport-https
    - curl
    - git
  ```

  **Note** that `apt-transport-https` is required, and therefore installed by default, because I have setup the template for the `log2ram` repository to use HTTPS.

### log2ram

[log2ram](https://github.com/azlux/log2ram) helps extend the SD Card's life, by writing logs in the RAM instead of the SD Card.
If you're using an SSD/HDD instead of a SD Card it's probably unnecessary.

- Whether to enable log2ram or not.

  ```yaml
  raspberry_pi_enable_log2ram: true
  ```

- log2ram customization.

  The default settings usually work just fine. However, in some occasions, especially when running Ubuntu, the error `/var/log.hdd/ doesn't exist! Can't sync.` might appear. This means that `/var/log` is larger than log2ram's default size (40M), and you have to use a bigger size.

  ```yaml
  raspberry_pi_log2ram_size: "40M"
  raspberry_pi_log2ram_use_rsync: "false"
  raspberry_pi_log2ram_mail: "true"
  raspberry_pi_log2ram_path_disk: "/var/log"
  raspberry_pi_log2ram_zl2r: "false"
  raspberry_pi_log2ram_comp_alg: "lz4"
  raspberry_pi_log2ram_log_disk_size: "100M"
  ```

### User configuration

#### Default user configuration

- The default user. Default values are `pi`for Raspbian, and `ubuntu` for Ubuntu.

  ```yaml
  raspberry_pi_default_user: "pi"
  ```

- The password to use for the default user. Default passwords are `raspberry` for Raspbian and `ubuntu` for Ubuntu.

  ```yaml
  raspberry_pi_default_user: "pi"
  raspberry_pi_default_user_password: []
  raspberry_pi_default_user_requires_passwd: true
  ```

- Whether the default user should be allowed to use `sudo` without a password or not. The default for Raspbian/Ubuntu is `false`.

  ```yaml
  raspberry_pi_default_user_sudo_passwd: true
  ```

#### New user configuration

- The name of the new user to add.

  ```yaml
  raspberry_pi_new_user: []
  ```

- The password for the new user.

  ```yaml
  raspberry_pi_new_user_password: []
  ```

- The primary group of the new user.

  ```yaml
  raspberry_pi_new_user_primary_group: users
  ```

- Additional groups to append to the new user.

  - For Raspbian, the default groups are: `adm,dialout,audio,video,plugdev,input,netdev,gpio,i2c,spi,sudo`
  - For Ubuntu, the default groups are:
    `adm,dialout,cdrom,floppy,audio,video,plugdev,netdev,dip,lxd,sudo`

  ```yaml
  raspberry_pi_new_user_groups: []
  ```

- Whether the new user should be allowed to use `sudo` without a password or not.

  ```yaml
  raspberry_pi_new_user_sudo_passwd: true
  ```

## Dependencies

A Raspberry Pi, with Raspbian or Ubuntu _already_ installed.

## Example Playbook

TODO..

## License

MIT / BSD

## Author Information

Xristos Zervakis
