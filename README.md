# DC/OS installer

This is a script to install DC/OS on baremetal nodes or VM instances. It downloads all dependencies, checks for configuration parameters, autogenerates all required configuration files and finally completes the *`bootstrap`* node installation.
It also generates another script to be ran (copied and pasted) in all other nodes in the cluster (*`master`*/*`slave`*/*`slave_public`*). This downloads all dependencies and completes the node installation automatically.

Installation is performed running a single command in the *`bootstrap`* node, and copying & pasting the resulting command in each additional node of the cluster. This copy&paste command can optionally include as a parameter the `ROLE` (*`master`*/*`slave`*/*`slave_public`*) that the node in particular will have. If the parameter is not passed, the script will ask for the `ROLE` interactively.

This installer is a simplified and scripted version of the **[official DC/OS Advanced Installation process](https://docs.mesosphere.com/1.8/administration/installing/custom/advanced/)**. Before using this script, you should make sure to be familiar with that installation method. This includes understanding the roles of each node type, including the *`bootstrap`* node, the *`master`* node(s), the (private) *`slave`* node(s), and the *`public_slave`* node(s).

***THIS SCRIPT IS PROVIDED "AS IS", AND HAS ABSOLUTELY NO WARRANTY OR SUPPORT. USE AT YOUR OWN RISK.***

## Usage

Run the script in the node that will be used as bootstrap by copying and pasting the `curl` command below. Optionally, edit the first section according to the desired cluster configuration. This includes:

- Adding the adequate *download link for the desired version* (default: latest **Open DC/OS testing** version available)
- Modifying the default bootstrap username/password (default: **bootstrapuser/deleteme**).
- Adjusting the security level (default: **permissive**).
- Optionally, adjust the cluster name or the directory for the installer to use as storage.

The script requires to have open connectivity for the ports required for the download (configurable in the script) and for DC/OS to work properly.
The script assumes some default values. If you wish to modify these parameters, edit the first section of the script and re-run.

**IMPORTANT**: Run as root in the bootstrap node. Do `sudo su`, `cd`, then run the commands.
Do NOT `sudo command` instead.

**Direct Installation** with default parameters including **"testing" Open Source DC/OS** branch, simply login to the bootstrap node, and download+run the script::

```
sudo su -
cd
source <(curl https://raw.githubusercontent.com/fernandosanchezmunoz/DCOS_installer/master/dcos_install_centos7.sh)
```
 For other versions, releases, or installation options (such as **Enterprise DC/OS**), edit the script before running it and modify the required parameters:
```
sudo su -
cd
curl -O https://raw.githubusercontent.com/fernandosanchezmunoz/DCOS_installer/master/dcos_install_centos7.sh
#Edit download link
vi dcos_install_centos7.sh +16
```
Edit the installation file as required (e.g. modify download link) and then:
```
bash dcos_install_centos7.sh
```

The script will provide a command during the installation process pointing to the node installer script in the bootstrap node, to be copied & pasted in the cluster nodes with the following format:

```curl -O http://BOOTSTRAP_NODE_IP:PORT/node_installer.sh && sudo bash node_installer.sh [ROLE]```

After installation has completed, and depending on the options selected in the menu, the script may provide additional commands to install other features, such as the Marathon-LB load balancer, or the Ceph distributed storage solution.
