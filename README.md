# Ansible Role: Raspberry Pi

This role configures a Raspberry Pi running Raspbian or Ubuntu (ARM).

The configuration includes:

- Configure the locale/timezone/hostname.
- Change the default password.
- Create new users and set their passwords.
- Configure sudoers.
- Install extra packages.
- Enable Debian buster-backports repository and install packages from there.
- Install, customize and enable [log2ram](https://github.com/azlux/log2ram).

## Requirements

None.

## Role Variables

Available variables are listed below, along with the default values. For more information, see `defaults/main.yml`.

#### Locale, timezone and hostname configuration

```yaml
raspberry_pi_locale: "en_US.UTF-8"
raspberry_pi_timezone: "Europe/Athens"
raspberry_pi_hostname: "raspberrypi"
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
  raspberry_pi_new_user_requires_passwd: true
  ```

## Dependencies

A Raspberry Pi, with Raspbian or Ubuntu _already_ installed.

## Example Playbook

TODO..

## License

MIT / BSD

## Author Information

Xristos Zervakis

```

```
