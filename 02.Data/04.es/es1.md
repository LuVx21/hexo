<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [QA](#qa)

<!-- /TOC -->
</details>

## QA

max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

```bash
echo 'vm.max_map_count=655360' >> /etc/sysctl.conf
sudo sysctl -p
```

max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]

```bash
echo 'es hard nofile 65536\nes soft nofile 65536' >> /etc/security/limits.conf
```

the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured

```yml
node.name: node-1
cluster.initial_master_nodes: ["node-1"]
```

can not run elasticsearch as root
w
```bash
adduser elasticsearch
passwd elasticsearch
chown -R elasticsearch /ope/install/es
```
