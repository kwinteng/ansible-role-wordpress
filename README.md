# Ansible role `wordpress`

An Ansible role for installing Wordpress. Specifically, the responsibilities of this role are to:

- install the EPEL repository and Wordpress dependencies
- install Wordpress
- set up the database and configure Apache
- fetch security keys and salts
- generate `wp-config.php`

## Requirements

no specific requirements

## Role Variables

| Variable             | Default     | Comments (type)                                   |
| :---                 | :---        | :---                                              |
| `wordpress_database` | 'wordpress' | The name of the database for Wordpress.           |
| `wordpress_user`     | 'wordpress' | The name of the database user.                    |
| `wordpress_password` | 'wordpress' | The password of the database user.                |
| `wordpress_plugins`  | []          | Plugins to be installed. See below. (since 1.1.0) |
| `wordpress_themes`   | []          | Themes to be installed. See below. (since 1.1.0)  |

**Remark:** it is **very strongly** suggested to change the default password.

To install plugins and themes (from the Wordpress Plugin and Theme Directory), you need to specify at least the name. Most plugins and themes also have a version, in which case you need to provide it as well. The version number should not be given if the plugins does't have one. An example:

```yaml
wordpress_plugins:
  - name: wp-super-cache
    version: 1.4.5
  - name: jetpack
    version: 3.7.2
  - name: lipsum  # Plugin without a version
wordpress_themes:
  - name: xcel
    version: 1.0.9
```

### Configuring Apache and Mariadb

The variables for this role are not mandatory, but in the dependent roles (`bertvv.httpd` and `bertvv.mariadb`), some variables have to be set:

```Yaml
httpd_scripting: 'php'
mariadb_databases:
  - wordpress_db
mariadb_users:
  - name: wordpress_usr
    password: ywIapecJalg6
    priv: wordpress_db.*:ALL
```

* PHP scripting should be enabled
* A database should be created. Variable `wordpress_database` should have the same value as `mariadb_databases`
* A database user with access to the database should be created. Variables `wordpress_user` and `wordpress_password` should have the same values as the respective settings here.

## Dependencies

- [bertvv.httpd](https://galaxy.ansible.com/list#/roles/3047)
- [bertvv.mariadb](https://galaxy.ansible.com/list#/roles/3518)

## Example Playbook

See the [test playbook](tests/test.yml).

## Testing

The `tests` directory contains tests for this role in the form of a Vagrant environment with a VM for each supported platform.  The playbook [`test.yml`](tests/test.yml) applies the role to a VM, setting role variables.

The directory `tests/roles/wordpress` should be a symbolic link that points to the root of this project in order to work. To create it, do

```ShellSession
$ cd tests/
$ mkdir roles
$ ln -frs ../../PROJECT_DIR roles/wordpress
```

Before running the test, install the dependent roles:

```ShellSession
$ ansible-galaxy install -p roles/ bertvv.httpd
$ ansible-galaxy install -p roles/ bertvv.mariadb
```

After executing the command `vagrant up`, a Wordpress site should be available at https://192.168.56.[345]/wordpress

## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section. Pull requests are also very welcome. Preferably, create a topic branch and when submitting, squash your commits into one (with a descriptive message).

## License

BSD

## Author Information

Bert Van Vreckem (bert.vanvreckem@gmail.com)

Contributions by:

- [Jordi Stevens](https://github.com/Xplendit)
