# HeatContrail

In order to launch a Contrail service-chain made of two Service-Instance (using Cirros with routing pre-enabled), follow those steps:

1. Install OpenStack CLI (if need be)
   * `yum install -y gcc python-devel \
    pip install python-openstackclient \
    pip install python-ironicclient`    
1. install heat client (if need be)
   * `pip install python-heatclient`
1. Install scripts
   * `git clone https://github.com/NicoJNPR/HeatContrail`
1. download Cirros image, credentials are `cirros/gocubsgo`
   * `wget http://download.cirros-cloud.net/0.4.0/cirros-0.4.0-x86_64-disk.img`
1. source your OpenStack env file
   * `source /etc/kolla/kolla-toolbox/admin-openrc.sh`
   1. create flavor
   * `openstack flavor create --ram 512 --disk 1 --vcpus 1 m1.tiny`
1. upload basic Cirros image (use Cirros image of your choice)
   * `openstack image create cirros \
          --disk-format qcow2 \
          --container-format bare \
          --public \
          --file cirros-0.4.0-x86_64-disk.img`
1. upload modified Cirros image with routing enabled, credentials are `cirros/cubswin:)`
   * `openstack image create CirrosVNF_mgnt_left_right \
          --disk-format qcow2 \
          --public \
          --container-format bare \
          --file HeatContrail/CirrosVNF_mgnt_left_right.img`
1. create the stack with default domain and tenant or custom ones

   1. `with default domain and tenant/project
        openstack stack create NicoHeatCirros \
        -t HeatContrail/heat_contrail_service_chain_cirros.yaml \
          -e HeatContrail/heat_contrail_service_chain_cirros.env`

   2. `with custom domain/tenant
        OS_PROJECT_NAME=Nicolas openstack stack create NicolasHeatCirros   -t HeatContrail/heat_contrail_service_chain_cirros.yaml   -e HeatContrail/heat_contrail_service_chain_cirros.env   --parameter domain=default-domain   --parameter tenant=$OS_PROJECT_NAME`
        (got this openstack bug in ocata https://bugs.launchpad.net/kolla-ansible/+bug/1690975)

1. Once stack is succesful, connect to Cirros VM `left_vm` (2.2.2.3) and ping remote cirros (3.3.3.3). Pings will go through and go through the service-chain.


