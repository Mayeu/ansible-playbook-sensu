# Sensu

Playbook to install and configure Sensu.

## Requirements

Sensu does not have a strong dependency on others software (it come by default
as an omnibus). But you will need a RabbitMQ server and a Redis server. They do
not have to be on the same host as Sensu thought.

## Supported system

Currently only Debian Wheezie & Jessie amd64 are supported and tested. Patch
welcome to support other OS.

## Installation

Just use Galaxy:

    $ ansible-galaxy install Mayeu.sensu

## Semantic versioning 2.0.0

Starting with the tag
[0.1.0](https://github.com/Mayeu/ansible-playbook-sensu/tree/0.1.0),
this playbook use semantic versioning.

The public API defined in the semantic versioning correspond to the
settings available to the user. Breaking the API (incrementing from
X.Y.Z to (X+1).Y.Z) in this context mean that the user need to change
its own configuration for the playbook to run.

Any new feature added (from X.Y.Z to X.(Y+1).Z) should have a working
default value that need no user interaction by default. If a feature
addition require user interaction, then it is not a minor upgrade,
but a major one.

## Expectation

RabbitMQ is expected to be listening on its TLS port (`5671`). If your server
is setup otherwise, you'll have to change the `sensu_server_rabbitmq_port`
variable.

This also mean that you will need to have the client certificate for your
RabbitMQ server in your `files/` folder:

```
files/
 ├── sensu_client_cert.pem
 └── sensu_client_key.pem
```

Note that you can find a complete set of certificates and keys in
`vagrant/files/` for **test purpose only**. Be smart, don't use certificates &
keys publicly available in production! You can also customize this path, see
[Roles Variables](#roles-variables).

The playbook also expect that you have the following folder organisation for
Sensu script in your `files/` folder:

```
files/
 └── sensu/
     ├── plugins/
     ├── handlers/
     └── extensions/
```

This follow the organisation of the Sensu community plugins repository.

## Configuration organisation

The configuration of Sensu does not uses the `/etc/sensu/config.json` file
anymore, but split all the files in the `/etc/sensu/conf.d` folder:

```
/etc/sensu
└── conf.d
    ├── api.json
    ├── checks.json
    ├── client.json
    ├── handlers.json
    ├── rabbitmq.json
    ├── redis.json
    └── settings.json
```

All those files are created from a template, that use the variable you'll setup
for your playbook. See [Role Variables](#role-variables).

If you have custom key/value you want to add to your client configuration,
please uses the `sensu_settings` variable, that will generate the
`settings.json` file.

## Role Variables

### Behaviour variables

|Name|Type|Description|Default|
|----|----|-----------|-------|
`sensu_install_client`|Boolean|Determine if we need to install the client part|`true`
`sensu_install_server`|Boolean|Determine if we need to install the server part|`true`
`sensu_install_uchiwa`|Boolean|Determine if we need to install the uchiwa. Will only work in `sensu_install_server` is also true|`true`

### General variables

|Name|Type|Description|Default|
|----|----|-----------|-------|
`sensu_settings`|Hash|Custom key => value you want to add to your configuration|`{}`
`sensu_user`|String|The user running sensu|`sensu`
`sensu_group`|String|the group running sensu|`sensu`
`sensu_checks`|Complex type|A variable representing the checks configuration. Will be auto converted to JSON|`[]`
`sensu_handlers`|Complex type|A variable representing the handlers configuration. Will be auto converted to JSON|`[]`
`sensu_embedded_ruby`|String|Indicate if Sensu should use the embedded Ruby, or the system one|`"true"`
`sensu_cert_dir`|String|Directory to look for the certificates|`files`
`sensu_cert_file_name`|String|Filename of the client certificate|`sensu_client_cert.pem`
`sensu_key_file_name`|String|Filename of the client key|`sensu_client_key.pem`

### Client variables

|Name|Type|Description|Default|
|----|----|-----------|-------|
`sensu_client_hostname`|String|Hostname of this client|`"{{ ansible_hostname }}"`
`sensu_client_address`|String|Address of this client|`"{{ ansible_default_ipv4.address }}"`
`sensu_client_subscription_names`|List|List of test to execute on this client| `[test]`

### Server variables

|Name|Type|Description|Default|
|----|----|-----------|-------|
`sensu_server_redis_host`|String|Hostname of the Redis server|`"127.0.0.1"`
`sensu_server_api_host`|String|Adress of the Sensu API server|`"127.0.0.1"`
`sensu_server_api_host`|String|Port of the Sensu API server|`4567`
`sensu_server_api_user`|String|User to connect to the api|`"sensu"`
`sensu_server_api_password`|String|Password for the api user|`"placeholder"`
`sensu_server_rabbitmq_vhost`|String|RabbitMQ virtual host|`"/sensu"`
`sensu_server_rabbitmq_hostname`|String|Hostname of the RabbitMQ server|`"127.0.0.1"`
`sensu_server_rabbitmq_port`|Integer|Port of the RabbitMQ server|`5672`
`sensu_server_rabbitmq_insecure`|String|Disabel TLS connection to RabbitMQ|`"false"`
`sensu_server_rabbitmq_user`|String|Username to connect to RabbitMQ|`"sensu"`
`sensu_server_rabbitmq_password`|String|Password to connect to RabbitMQ|`"placeholder"`
`sensu_server_dashboard_host`|String|The address on which Uchiwa will listen|`"0.0.0.0"`
`sensu_server_dashboard_port`|String|The port on which Uchiwa will listen|` "3000"`
`sensu_server_dashboard_user`|String|The username of the Uchiwa dashboard|`"uchiwa"`
`sensu_server_dashboard_password`|String|The password for the Uchiwa dashboard|`"placeholder"`
`sensu_server_dashboard_refresh`|Integer|Determines the interval to pull the Sensu APIs, in seconds|`5`
`sensu_server_patch_init_scripts`|Boolean|Indicate if patched init scripts that start/stop rabbitmq-server/redis-server when the sensu server is started stopped. Disable this if your redis / rabbitmq servers are on different machines|`true`

## Dependencies

Weak dependencies on:

* Redis
* RabbitMQ

## Tests

There is some really basic tests with the playbook. It just try to install a
server with client, a server only, and a client only, on 3 VM:

    $ vagrant up

The test depends on the following roles (that are already present in the
`vagrant/roles` folder):

* Mayeu.RabbitMQ
* redis


## License

BSD

## Author Information

Original author: Matthieu Maury, [https://6x9.fr](https://6x9.fr).

Contributor to the playbook:
- [Jeremy Johnson](https://github.com/jeremyajohnson)
- [Bob Renwick](https://github.com/bobbyrenwick)
- [rob](https://github.com/roobert)
- [Jennifer Blight](https://github.com/jgblight)
- [Andrii Kostenko](https://github.com/gugu)
- [scottginger](https://github.com/scottginger)
