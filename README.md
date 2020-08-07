# iotedge-vm-deploy

Detailed documentation is available on [Microsoft Docs](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-ubuntuvm?WT.mc_id=github-iotedgevmdeploy-pdecarlo)

## ARM Template to deploy a GPU enabled VM with IoT Edge Installed

ARM template to deploy a GPU enabled VM with IoT Edge pre-installed (via cloud-init)

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMSKeith%2Fiotedge-vm-deploy%2Fmaster%2FedgeDeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png" />
</a>

The ARM template visualized for exploration

<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2FMSKeith%2Fiotedge-vm-deploy%2Fmaster%2FedgeDeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.png" /></a>

## Azure CLI command to deploy IoT Edge enabled VM

```bash
az group deployment create \
  --name edgeVm \
  --resource-group replace-with-rg-name \
  --template-uri "https://aka.ms/iotedge-vm-deploy" \
  --parameters dnsLabelPrefix='my-edge-vm1' \
  --parameters adminUsername='azureuser' \
  --parameters deviceConnectionString=$(az iot hub device-identity show-connection-string --device-id replace-with-device-name --hub-name replace-with-hub-name -o tsv) \
  --parameters authenticationType='sshPublicKey' \
  --parameters adminPasswordOrKey="$(< ~/.ssh/id_rsa.pub)"
```

# Testing the IoT Edge installation
```bash
nvidia-smi
```
 The output should look like this:

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.33.01    Driver Version: 440.33.01    CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla V100-PCIE...  Off  | 00000001:00:00.0 Off |                  Off |
| N/A   30C    P0    38W / 250W |      0MiB / 16160MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```



```bash
sudo docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
```
The output should look like this (this will pull the docker image down and run nvidia-smi in the container):

```
Unable to find image 'nvidia/cuda:9.0-base' locally
9.0-base: Pulling from nvidia/cuda
976a760c94fc: Pull complete
c58992f3c37b: Pull complete
0ca0e5e7f12e: Pull complete
f2a274cc00ca: Pull complete
708a53113e13: Pull complete
371ddc2ca87b: Pull complete
f81888eb6932: Pull complete
Digest: sha256:56bfa4e0b6d923bf47a71c91b4e00b62ea251a04425598d371a5807d6ac471cb
Status: Downloaded newer image for nvidia/cuda:9.0-base
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.33.01    Driver Version: 440.33.01    CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla V100-PCIE...  Off  | 00000001:00:00.0 Off |                  Off |
| N/A   31C    P0    37W / 250W |      0MiB / 16160MiB |      4%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

##### Troubleshooting

To investigate what occurred during deployment the cloud-init logs can be found in the /var/log directory.  The files to view for this installation are:

cloud-init-output.log

cloud-init.log

cuda-installer.log




# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
