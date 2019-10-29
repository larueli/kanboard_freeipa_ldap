# Kanboard x Docker x FreeIPA LDAP

If you ever needed to use kanboard with FreeIPA LDAP, without too much config thanks to docker, you certainly realized that [the official docker hub image for KanBoard](https://hub.docker.com/r/kanboard/kanboard) doesn't work with LDAP in it.

Here it is ! Just follow the steps below.

## What's more than the official docker image ?

* LDAP support
* Bind to the config.php file to easily edit it

## Installation

### Install Docker and Docker-compose

Follow [these steps](https://docs.docker.com/install/) to install docker, select your OS on the left.

Follow [these steps](https://docs.docker.com/compose/install/) to install docker-compose

### Download this project

Run `git clone https://github.com/larueli/kanboard_freeipa_ldap.git` or download the whole project as a zip and unzip it in a folder we'll call `kanboard_freeipa_ldap`.

### Get your LDAP ready

In your ldap, create :

* a user only allowed to bind and search in the LDAP. [Click here](https://www.freeipa.org/page/HowTo/LDAP#System_Accounts) to see how to do it with FreeIPA
* a group for the users, you'll call `kanboard_users`. Only these users will be allowed to log in.
* a group for the managers, you'll call `kanboard_managers`. These users will be allowed to create projects.
* a group for the admins, you'll call `kanboard_admins`. These users will be allowed to manage all the KanBoard web app. 

You must set up a link between kanboard_admins, kanboard_managers to kanboard_users to allow people in those groups to log in.

If you want to know more about the permissions in Kanboard, [click here](https://docs.kanboard.org/en/latest/user_guide/users.html#users-roles)

### Edit the config.php

You must edit several values inside the config.php :

* `LDAP_SERVER` : put the IP of your FreeIPA LDAP server
* `LDAP_USERNAME` : the full DN to the user used for the bind
* `LDAP_PASSWORD` : the password of the user used for the bind
* `LDAP_USER_BASE_DN` : Change the dc
* `LDAP_USER_FILTER` : Change the name of the users group if you didn't called it kanboard_users
* `LDAP_GROUP_ADMIN_DN` : Change the dc and the name of the admins group if you didn't called it kanboard_admins
* `LDAP_GROUP_MANAGERS_DN` : Change the dc and the name of the managers group if you didn't called it kanboard_managers

Check [this page](https://docs.kanboard.org/en/latest/admin_guide/ldap_parameters.html) to see all other LDAP vars. On the left you can see all the possibilities and explainations linked to LDAP in Kanboard (sync, filter, auth, groups, ...)

### Create folders and add SSL files

Make sure the following folders exists (used as volumes for docker-compose) :

* kanboard_data
* kanboard_plugins
* kanboard_ssl

Inside kanboard ssl put the SSL files (if you have them) :

* kanboard.crt
* kanboard.key

### Deploy

Run `docker-compose up -d` inside the folder to start the whole system

Note : if you make any change to the config.php file, you have to restart the system with `docker-compose restart -d`

# Author

I am [Ivann LARUELLE](https://www.linkedin.com/in/ilaruelle/), engineering student in Networks & Telecommunications at the [Université de Technologie de Troyes](https://www.utt.fr/) in France, which is a public engineering university.

This tool was made in collaboration with [Jonas DOREL](https://github.com/jdorel) for the UTT Net Group, an non profit organization which aims to provide IT Service to all UTT students and student organizations.

Contact me for any issue : ivann.laruelle[at]gmail.com

# Licence

You are free to download, use, modify, redistribute theses files. The only thing is that you must credit me and keep the header of the files.
