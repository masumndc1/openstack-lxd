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


