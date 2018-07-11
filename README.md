# HeatContrail

In order to launch a Contrail service-chain made of two Service-Instance (using Cirros with routing pre-enabled), follow those steps:

1) pip install python-heatclient #(in case Heat client not installed in your cluster)
2) source /etc/kolla/kolla-toolbox/admin-openrc.sh
3) openstack image create cirros --disk-format qcow2 --public --container-format bare --file cirros-0.4.0-x86_64-disk.img #(use Cirros image you want)
4) openstack image create CirrosVNF_mgnt_left_right --disk-format qcow2 --public --container-format bare --file HeatContrail/CirrosVNF_mgnt_left_right.img #(prepared Cirros image with routing enabled, credentials are "cirros/cubswin:)")
5) openstack stack create NicoHeatCirros -t HeatContrail/heat_contrail_service_chain_cirros.yaml -e HeatContrail/heat_contrail_service_chain_cirros.env
6) Once stack is succesful, connect to Cirros VM (2.2.2.3) and ping remote cirros 3.3.3.3. Pings will go through and go through the service-chain.
