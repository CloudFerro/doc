
OpenStack instance migration - command line
===========================================

.. toctree::
  :maxdepth: 3
  :caption: Elasticity
  

Downloading an instance

.. warning::
  The instance should be checked if it is not created from image or volume. It will not create a proper image for migration in such cases.

List your instances.

openstack server list

Shut off the instance.

openstack server stop 6dd809ce-2c1a-44a1-b4b6-9a505a9b2685

Check if it is turned off.

openstack server show 638b6900-6aac-4f1c-8c8b-f0bb902ada39 | grep power_state

Create an image for migration.

openstack server image create --name "Migration image" c1f3602d-3b83-4c59-995c-168dd77a2242

Shelve option also creates a proper image and changes instance status to "Shelved Offloaded".

openstack server shelve c1f3602d-3b83-4c59-995c-168dd77a2242

Check if the new image was created properly. The size can not be 0 bytes. Should have a similar size as the instance itself.

openstack image list --private
+--------------------------------------+-----------------------------------+---------+
| ID                                   | Name                              | Status  |
+--------------------------------------+-----------------------------------+---------+
| 1fc30884-b35e-4eaf-8afc-e0a6b488e8b4 | Migration image                   | active  |
+--------------------------------------+-----------------------------------+---------+
 
openstack image show 1fc30884-b35e-4eaf-8afc-e0a6b488e8b4 | grep min_disk
| min_disk         | 8

Download new created image to local disk.

openstack image save --file ./image.raw 1fc30884-b35e-4eaf-8afc-e0a6b488e8b4

The image is ready. We need to move to different cloud and load proper credentials again. When new credentials are ready, check your cloud.
Uploading an instance

Load your image. There should be a table at the end of upload with detailed information about the newly uploaded image

openstack image create --file ./image.raw "Migration image"
 
+------------------+----------------------------------------------------+
| Field            | Value                                              |
+------------------+----------------------------------------------------+
| checksum         | 5049307eb2652778c00a645150db4e57                   |
| container_format | bare                                               |
| created_at       | 2020-02-07T13:36:23Z                               |
| disk_format      | raw
(...)

The new image should also be presented in the images list

openstack image list
+--------------------------------------+-----------------------------+--------+
| ID                                   | Name                        | Status |
+--------------------------------------+-----------------------------+--------+
| 6b28930c-e493-4b8b-ab85-3718269dde46 | Migration image             | active |
| 5ef3c17a-f9e6-4e2c-bc06-80508a443635 | Ubuntu 16.04 LTS            | active |
| 602485f6-7dc1-4796-b3f2-71847e48f67b | Ubuntu 18.04 LTS            | active |
+--------------------------------------+-----------------------------+--------+

Now you can use it in new instance creation.
There is no difference when migrating Windows instance that way

