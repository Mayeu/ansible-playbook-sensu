#Sensu server playbook

Playbook to install and configure a sensu-server. This is a work in progress,
it will come with various configuration tweaking later on :)

##Supported system

Currently only Debian Jessie amd64 is supported and tested. Patch welcome to
support other OS.

##Installation

Just clone (or submodule) this repository under the name `sensu-server` in your
`roles` directory. This file, `test.yml` and `Vagrantfile` will be ignored by
ansible anyway.

###Using it

####Roles Dependancies

You will need this [RabbitMQ
role](https://github.com/Mayeu/ansible-playbook-rabbitmq) and this [Redis
role](https://github.com/Mayeu/ansible-playbook-redis).

####Variables

You may need to redefine the following variables:

|Name|Type|Description|Default|
|----|----|-----------|-------|
`sensu_server_redis_host`|String|Hostname of the Redis server|"127.0.0.1"
`sensu_server_api_host`|String|Adress of the Sensu API server|"127.0.0.1"
`sensu_server_rabbitmq_hostname`|String|Hostname of the RabbitMQ server|"127.0.0.1"
`sensu_server_rabbitmq_user`|String|Username to connect to RabbitMQ|"sensu"
`sensu_server_rabbitmq_password`|String|Password to connect to RabbitMQ|"placeholder"
`sensu_server_dashboard_password`|String|Password for the sensu dashboard|"placeholder"
`sensu_server_dashboard_host`|String|Hostname of the Sensu Dashboard|"127.0.0.1"
`sensu_checks`|Complex type|A variable representing the checks configuration. Will be auto converted to JSON|''
`sensu_handlers`|Complex type|A variable representing the handlers configuration. Will be auto converted to JSON|''
`sensu_server_embedded_ruby`|String|Indicate if Sensu should use the embedded Ruby, or the system one|"false"

####Files

You need to have the following files in your playbook `files/` folder:

* SSL certificate for rabbitmq
* the handlers script needed

    files/
     |- rabbitmq_cacert.pem
     |- rabbitmq_server_cert.pem
     |- rabbitmq_server_key.pem
     |- sensu_client_cert.pem
     |- sensu_client_key.pem
     |- sensu/
     |--- handlers/
     |----- <all your .rb handler script>


##Test

There is some really basic tests with the playbook. It just try to install
rabbitmq in a vagrant VM. Just run:

    $ vagrant up

and the VM will be provisioned by ansible with the test.yml.
