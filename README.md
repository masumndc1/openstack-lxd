# openstack-lxd
Openstack with lxd as compute

## Procedure how to use this repo
1. First change all of your settings in host_vars accordingly.
1. Run common_pkg.yml to install common and required packages in both controller and compute node
1. Run controller.yml with just pkgs and mariadb tag to install packages and mariadb service.
1. Run mysql_secure_installation on controller node to setup root password of mariadb.
1. Run controller.yml again to install and configure rest of the services.

## Keystone
1. Run contoller.yml --tags keystone to install and configure keystone.
1. Populate the Identity service database on controller node:
```
su -s /bin/sh -c "keystone-manage db_sync" keystone
```
1. Initialize Fernet key repositories on controller node:
```
keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone
keystone-manage credential_setup --keystone-user keystone --keystone-group keystone
```
1. Bootstrap the Identity service on controller node:
```
keystone-manage bootstrap --bootstrap-password ADMIN_PASS \
  --bootstrap-admin-url http://controller:5000/v3/ \
  --bootstrap-internal-url http://controller:5000/v3/ \
  --bootstrap-public-url http://controller:5000/v3/ \
  --bootstrap-region-id RegionOne
```
Replace ADMIN_PASS with a suitable password for an administrative user.

## Glance
1. Create glance database, glace db user and grant user access
```
ansible-playbook -i inventory/hosts controller.yml --tags glance_db
```
. Source the admin credentials to gain access to admin-only CLI commands

```
admin-openrc

```
. Create the glance user and give it admin role and create service project
. Create the glance service entity and image service api endpoints.

```
$ openstack user create --domain default --password-prompt glance
$ openstack role add --project service --user glance admin
$ openstack service create --name glance \
    --description "OpenStack Image" image
$ openstack endpoint create --region RegionOne \
    image public http://controller:9292
$  openstack endpoint create --region RegionOne \
    image internal http://controller:9292
$ openstack endpoint create --region RegionOne \
    image admin http://controller:9292
```
. Run again contoller.yml playbook to configure rest of glance related service
```
ansible-playbook -i inventory/hosts controller.yml --tags glance
```
. Populate the Image service database:
```
# su -s /bin/sh -c "glance-manage db_sync" glance
```
. Run one more time contoller.yml playbook.

```
ansible-playbook -i inventory/hosts controller.yml --tags glance
```

