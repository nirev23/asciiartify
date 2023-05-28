# Plugin fo Kubectl

**Kubeplugin** - tools for Kubernetes cluster

## Features
Output the statistics to the console
"Resource, Namespace, Name, CPU, Memory"

## Using a plugin

To use a plugin, download, rename and make the plugin executable:
```shell
sudo chmod +x ./kubectl-foo
```
and place it anywhere in your `PATH`:
```shell
sudo mv ./kubectl-foo /usr/local/bin
```
You may now invoke your plugin as a `kubectl` command:
```shell
kubectl foo arg1 arg2
```
where 
**arg1** - type of resources "pods" or "node"
**arg2** - namespaces

