# Cluster Timelord

*Note: Out of date for Beta - will update soon*

The default distribution in alpha 1.5 will now launch 2 VDF instances on the current machine from the `run_all.sh` startup script. However one can have each VDF or couple of VDFs on different machines. We tend run a cluster in AWS with the fullnode and timelord running on a t3.small and then a single VDF on a group of e.g. c5.xlarge instances.

On the Timelord server you will need to edit the `scripts/run_timelord.sh` script and comment out `_run_bg_cmd python -m src.timelord_launcher`. In `config/config.yaml` you will add the IP address of each client machine under `vdf_clients` and enter an `ips_estimate` for each VDF client machine. VDF client machines must be able to see the port in `vdf_server: port:`. The default config has chosen port 8000. Additionally you will need to set the `vdf_server: host:` to either the IP that you want your clients to reach or leave the entry blank for all of your configured IP interfaces.

You can test your server configuration and port availability by running `nmap -Pn -p 8000 TIMELORD_SERVER` on the client machines where TIMELORD_SERVER is the hostname or IP address of the Timelord.

On each client, you place the IP of the Timelord server in the `timelord_launcher: host:` section of `config_yaml` on the client machine. Note that the `port:` here must match what you choose on the Timelord server. Also you can choose how many VDFs run on the client by setting `timelord_launcher: process_count` to 1 to only run one VDF on the client machine. The default is set at 2. Once the Timelord server is started you can start the VDF client(s) by issuing `python -m src.timelord_launcher &` from the Python virtual environment.