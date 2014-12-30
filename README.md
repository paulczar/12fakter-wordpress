12Fakter Wordpress
==================

Sort of but not quite 12 factor wordpress.

What does it do?
----------------

Utilizes Docker, CoreOS and Confd to provide a sort of but not quite 12 factor wordpress.

The included example application will now be available via a port mapping so that you can interact with it:

```
$ vagrant up
$ firefox http://localhost:8080
```

How it works ?
==============

Vagrant spins up 3 coreos nodes,  on the first node it spins up a docker registry and a mysql server ( caching both images for faster rebuilds ).   It also builds a Wordpress Docker image that contains wordpress, and nginx+hhvm as well as confd ( and etcd ).   A boot script sets up the environment including any default variables ( see `bin/boot` for variables that can be changed by passing `-e` into `docker run`) and finally calls Supervisord to run the necessary apps.   All apps called by cond pass their stdout back to confd which passes it back out to stdout so that `docker logs` can see all activity.

Finally the wordpress container is started on each of the VMs utilizing environment variables to pass to confd to create a `wp-config.php` file with database paths etc and  a volume mount is used to mount the Vagrant shared wordpress files into the containers.   Obviously in a production environment you wouldn't have a docker volume share, but creating a shared filesystem in the cloud is not a hard problem to solve and can be do in a container which is then volume mounted to wordpress.


Author(s)
======

Paul Czarkowski (paul@paulcz.net)

License
=======

Copyright 2014 Paul Czarkowski

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
