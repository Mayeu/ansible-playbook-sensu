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

## Role Variables

|Name|Type|Description|Default|
|----|----|-----------|-------|
`sensu_install_client`|Boolean|Determine if we need to install the client part|`true`
`sensu_install_server`|Boolean|Determine if we need to install the server part|`true`
`sensu_client_hostname`|String|Hostname of this client|`"localhost"`
`sensu_client_address`|String|Address of this client|`"127.0.0.1"`
`sensu_client_subscription_names`|List|List of test to execute on this client| `[test]`
`sensu_server_redis_host`|String|Hostname of the Redis server|`"127.0.0.1"`
`sensu_server_api_host`|String|Adress of the Sensu API server|`"127.0.0.1"`
`sensu_server_rabbitmq_hostname`|String|Hostname of the RabbitMQ server|`"127.0.0.1"`
`sensu_server_rabbitmq_user`|String|Username to connect to RabbitMQ|`"sensu"`
`sensu_server_rabbitmq_password`|String|Password to connect to RabbitMQ|`"placeholder"`
`sensu_server_dashboard_host`|String|Hostname of the Sensu Dashboard|`"127.0.0.1`"
`sensu_server_dashboard_password`|String|Password for the sensu dashboard|`"placeholder"`
`sensu_checks`|Complex type|A variable representing the checks configuration. Will be auto converted to JSON|`''`
`sensu_handlers`|Complex type|A variable representing the handlers configuration. Will be auto converted to JSON|`''`
`sensu_server_embedded_ruby`|String|Indicate if Sensu should use the embedded Ruby, or the system one|`"true"`

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
 |----- <all your .rb check script>
 |--- handlers/
 |----- <all your .rb handler script>
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
