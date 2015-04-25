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

## Expectation

RabbitMQ is expecting to listen on its TLS port (`5671`). If your
server is setup otherwise, you'll have to change the
`sensu_server_rabbitmq_port` variable.

## Role Variables

|Name|Type|Description|Default|
|----|----|-----------|-------|
`sensu_install_client`|Boolean|Determine if we need to install the client part|`true`
`sensu_install_server`|Boolean|Determine if we need to install the server part|`true`
`sensu_user`|String|The user running sensu|`sensu`
`sensu_group`|String|the group running sensu|`sensu`
`sensu_client_hostname`|String|Hostname of this client|`"localhost"`
`sensu_client_address`|String|Address of this client|`"127.0.0.1"`
`sensu_client_subscription_names`|List|List of test to execute on this client| `[test]`
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
`sensu_checks`|Complex type|A variable representing the checks configuration. Will be auto converted to JSON|`[]`
`sensu_handlers`|Complex type|A variable representing the handlers configuration. Will be auto converted to JSON|`[]`
`sensu_server_embedded_ruby`|String|Indicate if Sensu should use the embedded Ruby, or the system one|`"true"`
`sensu_server_patch_init_scripts`|Boolean|Indicate if patched init scripts that start/stop rabbitmq-server/redis-server when the sensu server is started stopped. Disable this if your redis / rabbitmq servers are on different machines|`true`

## Files required

You need to have the following files in your playbook `files/` folder:

* SSL certificate for Sensu
* the handlers script needed

```
files/
 |- sensu_client_cert.pem
 |- sensu_client_key.pem
 |- sensu/
 |--- plugins/
 |----- <all your check script>
 |--- handlers/
 |----- <all your handler script>
 |--- extensions/
 |----- <all your extension script>
```

## Dependencies

Weak dependencies on:

* Redis
* RabbitMQ

## Tests

There is some really basic tests with the playbook. It just try to install a
server with client, a server only, and a client only, on 3 VM:

    $ vagrant up
    $ export ANSIBLE_HOST_KEY_CHECKING=False; ansible-playbook -vv test.yml -i hosts -k

The test depends on the following roles:

* Mayeu.RabbitMQ
* redis

## License

BSD
