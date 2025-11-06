# Linux 网络配置

## 网络基础知识

### IP 地址

**IP 地址** 是计算机在网络中的逻辑编号，用来定位设备。

目前主要有两种版本：

- **IPv4**：由 4 个字节组成，如 `192.168.1.5`，总共约 43 亿个可用地址。
- **IPv6**：由 16 个字节组成，如 `2408:4004:1234::1`，为未来的物联网准备的。

在 Linux 中查看当前 `IP`：

```bash
ip addr show
```

### MAC 地址

**MAC（Media Access Control）地址** 是写在网卡硬件里的唯一编号，用来在局域网内识别设备。

它由 6 组十六进制数构成，如：

```bash
92:a9:20:cc:ed:88
```

在以太网中，数据包实际上是通过 **MAC 地址** 发送的。

`IP` 地址只是逻辑上的目的地，操作系统需要用 `ARP`（地址解析协议）把 `IP` 转成目标 MAC 才能真正发包。

在 Linux 中查看本机 MAC 地址：

```bash
ip link show
```

### 子网掩码

**子网掩码（Subnet Mask）** 决定了一个 `IP` 地址属于哪个网络段，以及哪些地址是同一个局域网。
 例如：

```bash
IP:        192.168.1.5
子网掩码:  255.255.255.0
```

换算成 `CIDR` 表示法是 `/24`，表示前 24 位是网络号（即 192.168.1），后 8 位用于主机编号。

因此这一网段可用的主机范围是 `192.168.1.1` 到 `192.168.1.254`。

查看 Linux 当前网络配置：

```bash
ip route
```

### 网关

如果目标 `IP` 不在当前子网里，系统就会把数据包交给 **默认网关（Default Gateway）**。

当主机要访问另一个网段（比如访问互联网）时，数据包会先发送到默认网关，再由网关转发出去。

查看默认网关：

```bash
ip route show default
```

### DNS

**`DNS`（Domain Name System）** 是域名系统，它可以把人类容易记的域名转换成机器能识别的 `IP` 地址。

例如，当你访问 `www.baidu.com` 时，系统会先查询 `DNS`，把它解析为一个 `IP` 地址，再发起连接。

Linux 的 `DNS` 服务器配置一般在：

```bash
/etc/resolv.conf
```

### 端口

**端口（Port）** 是主机内部区分不同网络服务的编号。一个 `IP` 地址对应一台主机，但主机可能同时运行多个服务。

而端口号就是区分这些服务的标志。

常见端口：

| 服务    | 协议  | 默认端口 |
| ------- | ----- | -------- |
| `HTTP`  | `TCP` | 80       |
| `HTTPS` | `TCP` | 443      |
| `SSH`   | `TCP` | 22       |
| `MySQL` | `TCP` | 3306     |
| `DNS`   | `UDP` | 53       |

查看本机端口占用：

```bash
sudo netstat -tulnp
```

或新命令：

```bash
sudo ss -tuln
```

### 协议

**协议（Protocol）** 是网络通信的规则。网络通信的每一层都有自己的协议。最常见的几种：

- **`TCP`（Transmission Control Protocol）**：面向连接、可靠传输（例如浏览网页、发邮件）。
- **`UDP`（User Datagram Protocol）**：无连接、轻量传输（例如视频直播、`DNS` 查询）。
- **`HTTP`/`HTTPS`**：在 TCP 之上定义的网页通信协议。
- **`ICMP`（Internet Control Message Protocol）**：用于诊断，比如 `ping` 命令。

## Linux 网络配置

### `ip` 命令

`ip` 命令是现代 Linux 系统中用于管理网络的核心工具，它功能强大，已经基本替代了旧的 `ifconfig`、`route`、`arp` 等命令。它可以用来查看和配置 **网络接口**、**IP 地址**、**路由**、**邻居表** 等。

- **网络接口**

```bash
ip link show       # 查看所有网络接口的状态
```

![image-20251103161242705](./Linux网络配置.assets/image-20251103161242705.png)

- **接口标识信息** `2: eth0:`
  - **序号**：系统内部接口编号
  - **接口名**：接口的名字，常见有：
    - **`lo`** → 回环接口
    - **`eth0`~`eth3`** → 有线以太网接口
    - **`docker0` / `vethXXX`** → 虚拟网桥或容器接口
    - **`loopback0`** → 手动创建的回环接口
- **接口状态标志（Flags）** `<BROADCAST,MULTICAST,UP,LOWER_UP,LOOPBACK>`
  - **`UP`** → 接口已启用，可以发送/接收数据
  - **`DOWN`** → 接口未启用
  - **`LOWER_UP`** → 链路层正常（物理连接存在或虚拟链路有效）
  - **`BROADCAST`** → 支持广播
  - **`MULTICAST`** → 支持组播
  - **`LOOPBACK`** → 回环接口
- **`MTU`（Maximum Transmission Unit）** `mtu 1500`
  - 表示接口可以传输的最大帧大小（字节数）
  - 回环接口通常很大（65536），物理网卡一般 1500
- **队列调度（`qdisc`）** `qdisc noqueue / mq`
  - **`qdisc (Queueing Discipline)`**，Linux 网络接口的数据包调度策略
  - 常见类型：
    - **`noqueue`** → 无队列，通常回环或虚拟接口
    - **`mq（multiqueue）`** → 多队列，物理网卡可以并行发送

- **状态字段（`state`）**`state UNKNOWN`
  - 描述内核对接口链路层的检测状态

- **接口模式**`mode DEFAULT`
  - 描述接口的工作模式或特定状态。

- **接口分组**`group default`
  - 逻辑分组标识，用于管理接口集合。

- **队列长度**`qlen 1000`
  - 发送队列长度，Linux 内核中数据包等待发送的缓冲区大小

- **链路类型**`link/loopback`
  - **`link/ether`** → 接口类型为以太网，后跟 MAC 地址
  - **`link/loopback`** → 回环接口，没有真实 MAC
  - **`brd`** → 广播地址


- **网络地址**

```bash
ip address show        # 查看所有接口的 IP 地址
```

![image-20251103164510569](./Linux网络配置.assets/image-20251103164510569.png)

- **`IP`地址**
  - **`IPv4`地址**`inet`
  - **`IPv6`地址**`inet6`

- **子网掩码** `/n`
  - `CIDR`表示

- **作用域**`scope`
  - **`host`** → 本机使用
  - **`link`** → 局域链路范围
  - **`global`** → 可路由

- **网络路由**

```bash
ip route show           # 查看路由表
```

![image-20251103170210514](./Linux网络配置.assets/image-20251103170210514.png)

- **`default`**：默认路由，匹配所有不明确的目的地址。
- **`via`**：指定下一跳网关。
- **`dev`**：通过哪个网卡发送。
- **`proto kernel`**：内核创建的路由。
- **`scope`**：IP 作用范围（`host`/`link`/`global`）。
- **`metric`**：路由优先级。

-----

- **接口类型**

| 类型         | 示例             | 说明                         |
| ------------ | ---------------- | ---------------------------- |
| 物理接口     | `eth0`, `ens33`  | 真实的以太网卡               |
| 回环接口     | `lo`             | 本机内部通信（127.0.0.1）    |
| 虚拟桥接接口 | `br0`, `docker0` | 相当于一个虚拟交换机         |
| 隧道接口     | `tun0`, `tap0`   | 用于 `VPN`、容器网络         |
| `VLAN` 接口  | `eth0.100`       | 在同一物理网卡上划分逻辑子网 |
| 绑定接口     | `bond0`          | 多网卡绑定成一条逻辑链路     |
| 虚拟以太接口 | `veth0`, `veth1` | 成对存在，用于容器通信       |

-----

- **启用/禁用接口**

```bash
sudo ip link set eth0 up
sudo ip link set eth0 down
```

- **修改 `IP` 地址**

```bash
sudo ip addr add 192.168.1.10/24 dev eth0
sudo ip addr del 192.168.1.10/24 dev eth0
```

- **修改网关**

```bash
sudo ip route add default via 192.168.1.1
sudo ip route del default via 192.168.1.1
```



