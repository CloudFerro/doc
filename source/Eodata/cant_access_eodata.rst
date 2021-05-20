Can't access eodata
===================

.. toctree::
  :maxdepth: 3
  :caption: Elasticity


If you have problems with access to eodata try the following:


install arping:

in CentOS:<br>
``sudo yum install arping``

in Ubuntu:
``sudo apt install arping``

 

check the name of the interface connected to eodata network:
``ifconfig``

based on the response, find the number of  the interface of 10.111.x.x (eth<number> or ens<number>)

after that invoke the following commands:

in CentOS:
``sudo arping -U -c 2 -I eth<number> $(ip -4 a show dev eth<number> | sed -n 's/.*inet \([0-9\.]\+\).*/\1/p')`` 


in Ubuntu:
``sudo arping -U -c 2 -I ens<number> $(ip -4 a show dev ens<number> | sed -n 's/.*inet \([0-9\.]\+\).*/\1/p')``


Next ping data.cloudferro.com again. If you receive answers, remount the resource:

``sudo umount -lf /eodata``

``sudo mount /eodata``

 

in Windows:

in command line run as administrator:

``route add 10.97.0.0/16 10.11.0.1``

     

and than run "mount_eodata" script from desktop.
