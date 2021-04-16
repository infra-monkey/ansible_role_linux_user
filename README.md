# Overview
This role manages linux users. You can define the username, groupname, password, ssh public key etc...

# Variables

| Name  | Type | Required | Default Value | Description |
| ----- | ---- | -------- | ------------- | ----------- |
| users.name | string | yes | n.a. | The username to define |
| users.comment | string | no | `user.name` | The user's description |
| users.group | string | no | `user.name` | The principal group the user belongs to |
| users.groups | string | no | '' | List of additional groups the user belongs to (they must exist) |
| users.password | string | no | '!!' | The encrypted password to set for the user |
| users.createhome | boolean | no | `true` | Create home directory for the user |
| users.exclusive | boolean | no | `true` | Is the ssh public the only one allowed |
| users.sshpubkey | string | no | n.a. | The ssh public key for the user |
| users.state | string | no | `present` | Should the use be present or absent |

# Variable example
testuser has no comment and password is disabled.
testuser2 has a comment defined and a password is defined (encrypted).

    users:
      - name: testuser
        group: testgroup
        password: "!!"
        exclusive: true
        sshpubkey: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDnSHTi+pfSKt4Aa5CvnYH5RaH1phb1VsizHjmgRPXYXCWUvGZdEf8lB1BLm9IZkGnYkaNHMzqUsqDblwmRGIs7LlFFWUeUGM3d8Ndj7vkUUysm58MvyrJ1MwHABAf/LWoskW4mN3gq6hw7mtYS9ksI++vO12IOhURaH9L1eUakfhUClUaVkZ7ZuT8IZeAoAPWCxegV+YGkimBxxAXUQoPpBpt3HiY71oXWARmbIYgV1URPabwgaPwP85P+YZx5/2zP0u08UL+zXHgECKQWJIbOo3gc4H5YWX4zwsGZN6cDjo740ge5AYUNgn3lRSgebn7Ug40iOgeBg4bP2K2igF testuser@example.com"
        state: present
      - name: testuser2
        comment: "Test User 2 Name"
        group: testgroup2
        groups:
          - docker
          - myapp
        password: "$6$/2Dw9xika4PAS0oT$aZsWkB/j38ksvfCuCL4xCb3t2J0GvCCa4JvQ8M0Y6xwzgbitqKVybXnTNf5ayN00O80bhAfU2KUenQ2J/mpCL0"
        exclusive: true
        sshpubkey: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDnSHTi+pfSKt4Aa5CvnYH5RaH1phb1VsizHjmgRPXYXCWUvGZdEf8lB1BLm9IZkGnYkaNHMzqUsqDblwmRGIs7LlFFWUeUGM3d8Ndj7vkUUysm58MvyrJ1MwHABAf/LWoskW4mN3gq6hw7mtYS9ksI++vO12IOhURaH9L1eUakfhUClUaVkZ7ZuT8IZeAoAPWCxegV+YGkimBxxAXUQoPpBpt3HiY71oXWARmbIYgV1URPabwgaPwP85P+YZx5/2zP0u08UL+zXHgECKQWJIbOo3gc4H5YWX4zwsGZN6cDjo740ge5AYUNgn3lRSgebn7Ug40iOgeBg4bP2K2igF testuser2@example.com"
        state: present

# Resources
generate encrypted password :

    $ python -c "import crypt; print crypt.crypt('testpassword2')"
    $6$/2Dw9xika4PAS0oT$aZsWkB/j38ksvfCuCL4xCb3t2J0GvCCa4JvQ8M0Y6xwzgbitqKVybXnTNf5ayN00O80bhAfU2KUenQ2J/mpCL0

# Automatique Testing

This role is tested using Molecule against:
- Python 3.7 and 3.8
- CentOS 7/8
- Debian 9/10