# ansible-avahi

Installs an avahi daemon on an ubuntu server.

## Tested operating systems

* Ubuntu 16.04 (xenial)
* Ubuntu 18.04 (bionic)
* Ubuntu 20.04 (focal)

## System requirements

* Python 3

## Tasks

* Installs package `avahi-daemon`
* Setup avahi services
* Reload service (if necessary)
* Start service (if not started)

## Role parameters

| Variable           | Type                     | Mandatory? | Default        | Description           |
|--------------------|--------------------------|------------|----------------|-----------------------|
| published_services | array of publish_service | no         | []             | The services to be published by avahi daemon |

### Definition publish_service

| Property      | Type             | Mandatory? | Description           |
|---------------|------------------|------------|-----------------------|
| name          | text             | yes        | The published name of the service |
| file          | text             | yes        | The filename of the service in your config |
| services      | array of service | yes        | The avahi services defined in service xml file |

### Definition service

| Property      | Type             | Mandatory? | Description           |
|---------------|------------------|------------|-----------------------|
| type          | text             | yes        | The avahi type of the service. See [avahi service type database @ github](https://github.com/lathiat/avahi/blob/master/service-type-database/service-types) for further information |
| port          | number (port)    | yes        | The port number of the service to be published |
| txt_records   | array of texts   | no         | Avahi txt records to be published |

## Usage

### requirements.yml

```yaml
- name: install-avahi
  src: https://github.com/borisskert/ansible-avahi.git
  scm: git
```

### Playbook

minimal playbook:

```yaml
    - hosts: servers
    - role: install-avahi
```

Typical playbook:

```yaml
    - hosts: servers

    - role: install-avahi
      published_services:
        - name: timemachine
          file: afpd
          services:
            - type: _afpovertcp._tcp
              port: 548
            - type: _device-info._tcp
              port: 0
              txt_records:
                - model=TimeCapsule
```
