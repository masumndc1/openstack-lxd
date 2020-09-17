# openstack-lxd
Openstack with lxd as compute

## Procedure how to use this repo
1. First change all of your settings in host_vars accordingly.
1. Run common_pkg.yml to install common and required packages in both controller and compute node
2. Run controller.yml to install packages and keep running services.
3. Run mysql_secure_installation on controller node to setup root password of mariadb.
4.

