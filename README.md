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

You will need this [RabbitMQ
role](https://github.com/Mayeu/ansible-playbook-rabbitmq) and this [Redis
role](https://github.com/Mayeu/ansible-playbook-redis). And you have to put the
needed certificates in your `files/` folder:

    files/
     |- rabbitmq_cacert.pem
     |- rabbitmq_server_cert.pem
     |- rabbitmq_server_key.pem
     |- sensu_client_cert.pem
     |- sensu_client_key.pem

##Test

There is some really basic tests with the playbook. It just try to install
rabbitmq in a vagrant VM. Just run:

    $ vagrant up

and the VM will be provisioned by ansible with the test.yml.
