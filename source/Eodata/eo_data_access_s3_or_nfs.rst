EO Data access - S3 or NFS?
===================

.. toctree::
  :maxdepth: 3
  :caption: Elasticity


Attention!
Your virtual machine has to be launched in project with EO DATA!

When creating new Linux VM in CREODIAS, you have the following /etc/fstab:

``````$ cat /etc/fstab``
``# /etc/fstab: static file system information.``
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/vda1 during installation
UUID=7069e088-5191-4a89-9aef-3f1517e961d9 /               ext4    errors=remount-ro 0       1
#nfs.eodata.cloudferro.com:/eodata/repository   /eodata nfs     ro,noauto,_netdev       0 0
s3fs#DIAS       /eodata fuse passwd_file=/root/.passwd-s3fs,_netdev,allow_other,use_path_request_style,uid=0,umask=0222,mp_umask=0222,mp_umask=0222,gid=0,url=http://data.cloudferro.com,use_cache=1,max_stat_cache_size=60000,list_object_max_keys=10000 0 0

 

By default the /eodata is mount using s3

If you want to mount it using nfs, you should invoke:

``sudo umount /eodata``

modify /etc/fstab

uncomment the line:

``nfs.eodata.cloudferro.com:/eodata/repository   /eodata nfs     ro,noauto,_netdev       0 0``

 

comment the line:

``#s3fs#DIAS       /eodata fuse passwd_file=/root/.passwd-s3fs,_netdev,allow_other,use_path_request_style,uid=0,umask=0222,mp_umask=0222,mp_umask=0222,gid=0,url=http://data.cloudferro.com,use_cache=1,max_stat_cache_size=60000,list_object_max_keys=10000 0 0``

save /etc/fstab

and invoke:

``sudo mount /eodata``
