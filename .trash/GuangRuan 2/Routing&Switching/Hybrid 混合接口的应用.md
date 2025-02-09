Hybrid 是一种特殊的二层接口类型，被称为混杂接口类型。Hybrid 类型与 Trunk 类型的接口类似， Hybrid 接口也能够承载多个 VLAN 的数据帧，而且用户可以灵活地指定特定 VLAN 的流量从该接口发送出去时是否携带 Tag 。另一方面， Hybrid 接口还能用千部署基千 IP 地址的 VLAN 划分。

## Hybrid 接口连接 PC

设备连接图如下：

![[Pasted image 20230413211437.png]]

在上图中， PC1 连接在 SW1 的 `GE 0/0/1` 接口上， SWl 的 `GE 0/0/15` 接口则连接着一个交换网络（图中忽略了这部分网络），现在，SW1 通过 Hybrid 接口为 PC1 提供接入服务，它的配置如下：

```text
[SWl]interface GigabitEthernet 0/0/1
[SW1-GigabitEthemet0/0/1]port link-type hybrid
```

以上命令将 `GE 0/0/1` 接口配置为 Hybrid 类型。

以华为 S 5700 交换机为例，接口缺省即为该类型，而且缺省将 VLAN 1 设置为 PVID，即缺省地配置了如下命令：

```
port hybrid pvid vlan 1
```

并且已经允许 VLAN 1 的流量以无标记的形式通过该接口，即缺省地配置了如下命令：

```
port hybrid untagged vlan 1
```

因此在这个场景中完成上述配置后， PC1 即可与 SW1 所连接的其他 VLAN 1 的设备进行通信。此时 PCl 被认为属于 VLAN 1 。PC1 发出的数据帧为无标记帧， SW1 在其 `GE 0/0/1` 接口上收到该帧后，为其打上缺省 VLAN (VLAN 1) 的 Tag ，而由于 VLAN 1 是该接口允许通过的 VLAN-ID，因此这个数据帧会被交换机接收。

## Hybrid 接口连接交换机

设备连接图如下：

![[Pasted image 20230413213032.png]]

在上图中， SW1 及 SW2 分别连接着 PC1 和 PC2，这两台交换机的 `GE 0/0/1` 接口均被配置为 Access 类型，并且都加入了 VLAN 10 。现在 SW1 的 `GE 0/0/15` 被配置为 Trunk 类型，并且放通了 VLAN 10。

```
[SW1-GigabitEthernet0/0/15]port link-type trunk
[SW1-GigabitEthernet0/0/15]port trunk allow-pass vlan 10
```

而 SW2 的 `GE 0/0/15` 接口如果配置为 Hybrid 类型，该如何配置才能使得 PC1 与 PC2 可正常通信？

由于对端接口 (SW1 的 `GE 0/0/15`) 以标记帧的方式发送 VLAN 10 的数据帧，因此 SW2 的 `GE 0/0/15` 接口也必须将 VLAN 10 的流量以标记帧的方式处理：

```
[SW2-GigabitEthernet0/0/15]port link-type hybrid
[SW2-GigabitEthernet0/0/15]port hybrid tagged vlan 10
```

在以上配置中，`port hybrid tagged vlan 10` 命令用于将 GE 0/0/15 接口加入 VLAN 10，并且 VLAN 的数据帧以标记帧方式通过接口。

现在考虑另一种情况，如下图所示：

![[Pasted image 20230413213616.png]]

SW1 左侧连接着 VLAN 10 、20 以及 1000，现在 SW1 的 `GE 0/0/15` 做了如下配置：

```
[SW1-GigabitEthernet0/0/15]port link-type trunk
[SW1-GigabitEthernet0/0/15]port trunk allow-pass vlan 10 20 1000
[SW1-GigabitEthernet0/0/15]port trunk pvid vlan 1000
```

当 SW1 从 `GE 0/0/15` 往外发送数据帧时，对于 VLAN 10 及 VLAN 20 的数据采用标记帧的形式发送，而对于 VLAN 1000 的数据则采用无标记帧的形式发送，那么如果 SW2 采用 Hybrid 接口与其对接，此时该接口的配置应该如下：

```
[SW2-GigabitEthernet0/0/15]port link-type hybrid
[SW2-GigabitEthernet0/0/15]port hybrid tagged vlan 10 20
[SW2-GigabitEthernet0/0/15]port hybrid pvid vlan 1000
[SW2-GigabitEthernet0/0/15]port hybrid untagged vlan 1000
```

## 实验报告

### 实验拓扑

![[Pasted image 20230413213853.png]]

### 实验编址

| 设备 | IP 地址 | 子网掩码 |
|-----|-----|-----|
|PC 1|192.168.1.1|255.255.255.0|
|PC 2|192.168.1.11|255.255.255.0|
|PC 3|192.168.1.2|255.255.255.0|
|PC 4|192.168.1.12|255.255.255.0|
|PC 5|192.168.1.100|255.255.255.0|

### 实验步骤

利用 Hybrid 接口模式按照 “通过接口发送加标签，接收去标签” 原则配置 S1与 S2，实现组内（即同一 VLAN 内）通信，组间隔离及 IT 部门对两个 VLAN 的访问管理。

配置 S1和 S2的三个接口 E0/0/1、E0/0/2和 E0/0/3实现组内通信组间隔离。

```S1
vlan batch 10 20
quit
int g0/0/1
port hybrid pvid vlan 10
port hybrid untagged vlan 10
int g0/0/2
port hybrid pvid vlan 20
port hybrid untagged vlan 20
int g0/0/24
port hybrid tagged vlan 10 20
quit
```

```S2
vlan batch 10 20
quit
int g0/0/1
port hybrid pvid vlan 10
port hybrid untagged vlan 10
int g0/0/2
port hybrid pvid vlan 20
port hybrid untagged vlan 20
int g0/0/24
port hybrid tagged vlan 10 20
quit

```

配置 S1和 S2的接口 E0/0/1、E0/0/2、E0/0/3和 E0/0/4实现 IT 部门对所有网络的访问。

```S1
int g0/0/23
port hybrid pvid vlan 30
port hybrid untagged vlan 10 20 30
int g0/0/1
port hybrid untagged vlan 30
int g0/0/2
port hybrid untagged vlan 30
int g0/0/24
port hybrid	tagged vlan 30
quit
```

```S2
int g0/0/1
port hybrid untagged vlan 30
int g0/0/2
port hybrid untagged vlan 30
int g0/0/24
port hybrid	tagged vlan 30
quit
```

### 测试

测试 PC-5对 PC-2和 PC-3的连通性：

![[Pasted image 20230413214628.png]]

测试 PC-1对 PC-2和 PC-3的连通性：

![[Pasted image 20230413214649.png]]
