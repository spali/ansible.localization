# Ansible Role: localization

[![Build Status](https://img.shields.io/travis/arillso/ansible.localization.svg?branch=master&style=popout-square)](https://travis-ci.org/arillso/ansible.localization) [![license](https://img.shields.io/github/license/mashape/apistatus.svg?style=popout-square)](https://sbaerlo.ch/licence) [![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-localization-blue.svg?style=popout-square)](https://galaxy.ansible.com/arillso/localization) [![Ansible Role](https://img.shields.io/ansible/role/d/id.svg?style=popout-square)](https://galaxy.ansible.com/arillso/localization)

## Description

This role configures different aspects of localization under Linux and Windows. It sets the timezone, the default locale and the keyboard layout during input.

## Installation

```bash
ansible-galaxy install arillso.localization
```

## Requirements

## Role Variables

### timezone

Linux and Windows use different Timezone formats. Therefore, for each OS a variable is provided for the Timezone

```yml
localization_timezone_windows: 'W. Europe Standard Time'
localization_timezone_linux: 'Europe/Zurich'
```

Alternatively, you can directly use the 'localization_timezone' can be used. The correct format must be observed.

```yml
localization_timezone:
```

### Linux

#### locale

Default locale. See `man 7 locale`

```yml
localization_default_locale: 'en_US.UTF-8'
```

#### console keyboard

Console keyboard layout (sg = swiss german)

```yml
localization_keymap: 'sg'
```

### Winodws

#### region

Set the location, format and unicode language settings of a Windows.

```yml
localization_region:
  location: 223
  format: de-CH
  unicode_language: de-CH
```

#### input keyboard

Sets the keyboard layout for Windows

```yml
localization_inputlang: 0807:00000807
```

## Dependencies

## Example Playbook

```yml
- hosts: all
  roles:
    - arillso.localization
```

## Author

- [Simon Bärlocher](https://sbaerlocher.ch)

## License

This project is under the MIT License. See the [LICENSE](https://sbaerlo.ch/licence) file for the full license text.

## Copyright

(c) 2019, Arillso
