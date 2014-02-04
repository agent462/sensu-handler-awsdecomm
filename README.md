awsdecomm
=========

A sensu handler for decomission of AWS nodes in sensu and chef-server.

A walkthrough to create your own http://www.ragedsyscoder.com/blog/2014/01/14/sensu-automated-decommission-of-clients/

Features
--------
* Checks state of node in AWS
* Decomission of node from Sensu
* Decomission of node and client from Chef Server
* Email on failure or success of decommission
* Handles normal resolve/create keepalive events when decomm is not needed

Usage and Configuration
-----------------------
This handler uses the sensu-plugin.
  > gem install sensu-plugin

You will need to attach this to the default handler in sensu.  Sensu sends client keepalive failures to the default handler.  If a client keepalive gets sent to this handler it will proceed to check if it should be removed from sensu and chef.
`/etc/sensu/conf.d/handlers/default.json`
````
{
  "handlers": {
    "default": {
      "type": "set",
      "handlers": [
        "awsdecommission"
      ]
    }
  }
}
````

`/etc/sensu/conf.d/handlers/awsdecommission.json`
````
{
  "handlers": {
    "awsdecommission": {
      "type": "pipe",
      "command": "/etc/sensu/handlers/awsdecomm.rb",
      "severities": [
        "ok",
        "warning",
        "critical"
      ]
    }
  }
}
````

awsdecomm relies on a bunch of configuration files set in awsdecomm.json.  You will need to provide AWS credentials, Chef Server information, Chef client key and smtp server information.

`/etc/sensu/conf.d/handlers/awsdecomm.json`
````
{ 
  "awsdecomm":{
      "chef_server_host": "127.0.0.1",
      "chef_server_port": "4000",
      "chef_server_version": "0.10.16.2",
      "chef_client_user": "sensu",
      "chef_client_key_dir": "/etc/sensu/conf.d/handlers/sensu.pem",
      "access_key_id": "ACCESS_KEY_ID",
      "secret_access_key": "SECRET_ACCESS_KEY",
      "mail_from": "sensu@example.com",
      "mail_to": "nobody@example.com",
      "smtp_address": "localhost",
      "smtp_port": "25",
      "smtp_domain": "localhost"
    }
}
````

Notables
--------
* This plugin attempts to catch failures and will alert you so that manual intervention can be taken.
* I've tried to incorporate a mildly verbose logging to the sensu-server.log on each step. 
* Right now this handler only checks us-east-1 and us-west-2.  This will end up configurable.  You can manually change this in the meantime.  
* This handler never terminates servers in AWS itself.  It simply takes action on nodes that do not exist or are in a terminated or shutting-down state.

Contributions
-------------
Please provide a pull request.  


License and Author
==================

Author:: Bryan Brandau <agent462@gmail.com>

Copyright:: 2013, Bryan Brandau

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
