# openstack-lxd
Openstack with lxd as compute

## Procedure how to use this repo
1. First change all of your settings in host_vars accordingly.
1. Run common_pkg.yml to install common and required packages in both controller and compute node
2. Run controller.yml with just pkgs and mariadb tag to install packages and mariadb service.
3. Run mysql_secure_installation on controller node to setup root password of mariadb.
2. Run controller.yml again to install and configure rest of the services.

