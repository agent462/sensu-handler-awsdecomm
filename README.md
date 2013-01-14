awsdecomm
=========

A sensu handler for decomission of AWS nodes in sensu and chef-server.

Features
--------
* Checks state of node in AWS
* Decomission of node from Sensu
* Decomission of node and client from Chef Server
* Email on failure or success of decommission


Usage and Configuration
-----------------------
This handler uses the sensu-plugin.
  > gem install sensu-plugin


awsdecomm relies on a bunch of configuration files set in awsdecomm.json.  You will need to provide AWS credentials, Chef Server information and client key as well as smtp server information.
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
This plugin attempts to catch failures and will alert you so that manual intervention can be taken.
I've tried to incorporate a mildly verbose logging to the sensu-server.log on each step.

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