3 node alpine vagrant
=====================

First
-----

Since we are using alpine linux we must install the alpine plugin for vagrant.

To make this easy for you you can run

``` sh
$ ./init-vagrant.sh
```

First run
---------

``` sh
$ vagrant up
$ vagrant halt
$ vagrant up
```

Run
---

When it's not your first run and there is no more a need to install docker you
can just do vagrant up

``` sh
$ vagrant up
```

ssh into one of the nodes

``` sh
$ vagrant ssh node-1
$ vagrant ssh node-2
$ vagrant ssh node-3
```

Init swarm
----------

run the following command to initialize the 3 node swarm

``` sh
./init-swarm.sh
```
