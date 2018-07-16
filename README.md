# HeatContrail

In order to launch a Contrail service-chain made of two Service-Instance (using Cirros with routing pre-enabled), follow those steps:

1. download Cirros image, credentials are `cirros/gocubsgo`

        wget http://download.cirros-cloud.net/0.4.0/cirros-0.4.0-x86_64-disk.img

1. install heat client if needed

        pip install python-heatclient

1. source your OpenStack env file

        source /etc/kolla/kolla-toolbox/admin-openrc.sh

1. upload basic Cirros image (use Cirros image of your choice)

        openstack image create cirros \
          --disk-format qcow2 \
          --container-format bare \
          --public \
          --file cirros-0.4.0-x86_64-disk.img

1. upload modified Cirros image with routing enabled, credentials are `cirros/cubswin:)`

        openstack image create CirrosVNF_mgnt_left_right \
          --disk-format qcow2 \
          --public \
          --container-format bare \
          --file HeatContrail/CirrosVNF_mgnt_left_right.img

1. make sure your OpenStack RC file contains the proper `OS_PROJECT_NAME`

        . openstackrc

1. create the stack with default domain and tenant or custom ones

        # 1. with default domain and tenant/project from .env
        openstack stack create NicoHeatCirros \
          -t HeatContrail/heat_contrail_service_chain_cirros.yaml \
          -e HeatContrail/heat_contrail_service_chain_cirros.env

        # 2a. with custom domain/tenant
        openstack stack create NicoHeatCirros \
          -t HeatContrail/heat_contrail_service_chain_cirros.yaml \
          -e HeatContrail/heat_contrail_service_chain_cirros.env \
          --parameter domain=<domain-name> \
          --parameter tenant=<tenant-name>

        # 2b. override OS_PROJECT_NAME
        OS_PROJECT_NAME=myproj openstack stack create NicoHeatCirros \
          -t HeatContrail/heat_contrail_service_chain_cirros.yaml \
          -e HeatContrail/heat_contrail_service_chain_cirros.env \
          --parameter domain=<domain-name> \
          --parameter tenant=$OS_PROJECT_NAME


1. Once stack is succesful, connect to Cirros VM `left_vm` (2.2.2.3) and ping remote cirros (3.3.3.3). Pings will go through and go through the service-chain.
