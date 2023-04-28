## 二. Redis的安装与配置

### 2.1 Redis的安装

#### 2.1.2 安装前的准备工作

##### 1) 安装 gcc

由于 Redis 是由 C/C++语言编写的，而从官网下载的 Redis 安装包是需要编译后才可安装 的，所以对其进行编译就必须要使用相关编译器。对于 C/C++语言的编译器，使用最多的是 gcc 与 gcc-c++，而这两款编译器在 CentOS7 中是没有安装的，所以首先要安装这两款编译器。 GCC，GNU Compiler Collection，GNU 编译器集合。

**安装 gcc： **`yum -y install gcc gcc-c++`

##### 2) 下载Redis

redis 的官网为： http://redis.io。点击下面的链接可以直接进行**下载**

[点击打开资源](C:\Users\^\Documents\aaa\Redis7动力节点\资源\资源)

##### 3) 上传到Linux

将下载好的压缩包**上传**到 Linux 的/opt/tools 目录中。（这个路径是随便创的）

#### 2.1.3 Redis 安装

##### 1) 解压Redis

- 将 Redis **解压到**/opt/apps 目录中。（这个路径是随便创的）
  `tar -zxvf redis-7.0.3.tar.gz -C /opt/apps`
- 进入到/opt/apps 目录中再将 Redis 解压包目录更名为 redis（不更名也无所谓）。
  `mv redis-7.0.3/ redis`

##### 2) 编译

编译过程是根据 Makefile 文件进行的，而 Redis 解压包中已经存在该文件了。所以可以 **直接进行编译**了

- 进入到解压目录中，然后执行编译命令 `make`。
  当看到如下提示时，表示编译成功
  <img src="..\MDImg\Redis7-动力节点\二\2.1eg.png" alt="img" style="zoom:67%;" />

##### 3) 安装

在 Linux 中对于编译过的安装包执行 `make install` 进行安装。
共安装了三个组件：redis 服务器、客户端与一个性能测试工具 benchmark。

##### 4) 查看bin目录

安装完成后，打开/usr/local/bin 目录，可以看到出现了很多的文件。
通过 `echo $PATH` 可以看到，/usr/local/bin 目录是存在于该系统变量中的，这样这些命令就可以在任意目录中执行了。

#### 2.1.4 Redis启动与停止

##### ####1) 前台启动

在任意目录执行 `redis-server `命令即可启动 Redis。这种启动方式会占用当前命令行窗口。
执行`ps aux | grep redis`可以查看到当前的 Redis 进程，默认端口号为 6379。
通过 `Ctrl + C` 命令可以停止 Redis

##### 2) 命令式后台启动

- 使用 `nohub` 命令，最后再添加一个`&`符，可以使要启动的程序在后台以守护进程方式运行。
  `nohup redis-server $`
  <img src="..\MDImg\Redis7-动力节点\二\2.1.4.2eg.png" alt="img" style="zoom:67%;" />
- 好处
  进程启动后不会占用一个会话窗口，且其还会在当前目录，即运行启动 命令的当前目录中创建一个 `nohup.out 文件`用于记录 Redis 的操作日志。

##### 3) Redis的停止

通过` redis-cli shutdown `命令可以停止 Redis。

##### 4) 配置式后台启动

- 使用 `nohup` 命令可以使 Redis 后台启动，
- 但每次都要键入` nohup` 与`&`符，比较麻烦。可以通过修改 Linux 中 Redis 的核心配置文件 `redis.conf` 达到后台启动的目的。
  `redis.conf` 文件在 Redis 的安装目录根下
  修改`redis.conf` 文件内容，将 `daemonize` 属性值由` no `改为` yes`，使 Redis 进程以守护进程方式运行。
- 修改后再启动 Redis，就无需再键入 nohup 与&符了，但必须要指定启动所使用的 Redis 配置文件。这是为什么呢？ 使用` nohup redis-server &`命令启动 Redis 时，启动项中已经设置好了 Redis 各个参数的 默认值，Redis 会按照这些设置的参数进行启动。但这些参数是可以在配置文件中进行修改 的，修改后，需要在启动命令中指定要加载的配置文件，这样，配置文件中的参数值将覆盖 原默认值。 Redis 已经给我们提供好了配置文件模板，是 Redis 安装目录的根目录下的 redis.conf 文 件。由于刚刚对 redis.conf 配置文件做了修改，所以在开启 Redis 时需要显示指出要加载的 配置文件。配置文件应紧跟在 redis-server 的后面

### 2.2 连接前的配置

#### 2.2.1 绑定客户端ip

Redis 可以通过修改配置文件来限定可以访问自己的客户端 IP。
<img src="..\MDImg\Redis7-动力节点\二\2.2.1eg.png" alt="img" style="zoom:67%;" />
只允许当前主机访问当前的 Redis，其它主机均不可访问。所以，如果不 想限定访问的客户端，只需要将该行注释掉即可。
<img src="..\MDImg\Redis7-动力节点\二\2.2.1-1eg.png" alt="img" style="zoom:67%;" />

#### 2.2.2 关闭保护模式

默认保护模式是开启的。其只允许本机的客户端访问，即只允许自己访问自己。但生产 中应该关闭(no)，以确保其它客户端可以连接 Redis。
<img src="..\MDImg\Redis7-动力节点\二\2.2.2-1eg.png" alt="img" style="zoom:67%;" />

#### 2.2.3 设置访问密码

为 Redis 设置访问密码，可以对要读/写 Redis 的用户进行身份验证。没有密码的用户可 以登录 Redis，但无法访问。

##### 2.2.3.1 密码设置

访问密码的设置位置在 `redis.conf` 配置文件中。默认是被注释掉的，没有密码 
<img src="..\MDImg\Redis7-动力节点\二\2.2.3-1eg.png" alt="img" style="zoom:67%;" />
没有通过密码登录的用户，无法读/写 Redis。

##### 2.2.3.2 使用密码

对于密码的使用，有两种方式：登录时未使用密码，则访问时先输入密码；登录时直接 使用密码登录，访问时无需再输入密码。

1. 登陆时未使用密码`auth 111`
2. 登陆时使用密码`redis-cli -a 111`
3. 退出时使用密码`redis-cli -a 111 shutdown`

> 注意：为了方便后面的学习，我们这里就不设置访问密码了，直接将其注释掉即可。

#### 2.2.4 禁止/重命名命令

后面要学习两个非常危险的命令：flushal 与 flushdb。它们都是用于直接删除整个 Redis 数据库的。若让用户可以随便使用它们，可能会危及数据安全。Redis 可以通过修改配置文 件来禁止使用这些命令，或重命名这些命令。以下配置，禁用了 flushall 与 flushdb 命令。 当然，在学习过程中暂时不禁用它们
<img src="..\MDImg\Redis7-动力节点\二\2.2.3.3-1eg.png" alt="img" style="zoom:67%;" />

#### 2.2.5 启动Redis

当然，若要使客户端能够连接 Redis，则必须开启服务端的 Redis。
`redis-server redis.conf`

### 2.3 客户端的分类

#### 2.3.1 命令行客户端

Redis 提供了基本的命令行客户端。打开命令行客户端的命令为 `redis-cli`。

- `-h`：指定要连接的 Redis 服务器的 IP。 
- `-p`：指定要连接的 Redis 的端口号。
- `-a`：指定要连接的 Redis 的密码

<img src="..\MDImg\Redis7-动力节点\二\2.3.1-1eg.png" alt="img" style="zoom:67%;" />

若连接的是本机 Redis，且端口号没有改变，保持默认的 6379，则`-h` 与`-p` 选项可以省略不写。

#### 2.3.2 图形界面客户端

##### 1)  Redis Desktop Manager

Redis 的图形界面客户端很多，其中较出名的是 Redis Desktop Manager 的客户端。不过，该软件原来是免费软件，从 0.8.8 版本后变为了商业化收费软件。

官网为：https://resp.app/（原来是 http://redisdesktop.com）。
[点击打开本地资源](C:\Users\^\Documents\aaa\Redis7动力节点\资源\资源)

#####  2) RedisPlus

RedisPlus 是为 Redis 可视化管理开发的一款开源免费的桌面客户端软件，支持 Windows 、

Linux、Mac 三大系统平台，RedisPlus 提供更加高效、方便、快捷的使用体验，有着更加现

代化的用户界面风格。

RedisPlus 的官网地址为 https://gitee.com/MaxBill/RedisPlus。

#### 2.3.2 Java 代码客户端

所谓 Java 代码客户端就是一套操作 Redis 的 API，其作用就像 JDBC 一样，所以 Java 代码客户端其实就是一个或多个 Jar 包，提供了对 Redis 的操作接口。

对 Redis 操作的 API 很多，例如 jdbc-redis、jredis 等，但最常用也是最有名的是 Jedis。

### 2.4 Redis 配置文件详解

Redis 的核心配置文件 redis.conf 在安装根目录下，默认包含 2000 多行。这些内容根据功能被划分为了很多部分。下面将一些重要部分进行介绍。

#### 2.4.1 基础说明

<img src="..\MDImg\Redis7-动力节点\二\2.4.1-1eg.png" alt="img" style="zoom:67%;" />这部分主要是给出一些说明，包含三部分意思：

- 第 1-6 行用于说明，如果要启动 Redis，需要指出配置文件的路径。
- 第 8-16 行用于说明当前配置文件中可以使用的的容量单位及意义。
- 第 18 行用于说明这些容量单位没有大小写之分

#### 2.4.2  includes

<img src="..\MDImg\Redis7-动力节点\二\2.4.2-1eg.png" alt="img" style="zoom:67%;" />

指定要在<mark>当前配置文件中包含的配置文件</mark>。这样做的目的主要是便于配置信息管理：可以将不同场景的配置都进行单独定义，然后在当前核心配置文件中根据不同场景选择包含进不同的配置文件。

**在`redis.conf`文件最后一行添加`include 自定义配置文件的位置`来将自定义配置文件里的配置覆盖掉`redis.conf`的配置**

#### 2.4.3 modules

<img src="..\MDImg\Redis7-动力节点\二\2.4.3-1eg.png" alt="img" style="zoom:67%;" />
Redis 配置文件中可以通过加载不同的<mark>第三方模块</mark>，来**增强、扩展 Redis 的功能。**

#### 2.4.4 network

<img src="..\MDImg\Redis7-动力节点\二\2.4.4-1eg.png" alt="img" style="zoom:67%;" />

Network 配置模块是比较重要的部分，主要进行网络相关的配置。其中较重要的有：

##### 1) bind

<img src="..\MDImg\Redis7-动力节点\二\2.4.4.1)-1eg.png" alt="img" style="zoom:67%;" /><mark>指定可以访问当前 Redis 服务的客户端 IP</mark>，**默认只允许本地访问**，即当前 Redis 自己访问自己。**为了使所有其它客户端都可访问，一般要将其注释掉。**

##### 2) protected-mode

<img src="..\MDImg\Redis7-动力节点\二\2.4.4.2)-1eg.png" alt="img" style="zoom:67%;" />默认保护模式是开启的。其只允许本机的客户端访问，即只允许自己访问自己。但<mark>生产中应该关闭，以确保其它客户端可以连接 Redis。</mark>

##### 3) port

<img src="..\MDImg\Redis7-动力节点\二\2.4.4.3)-1eg.png" alt="img" style="zoom:67%;" />
Redis 监听的连接<mark>端口号</mark>，默认 6379。 

##### 4) tcp-backlog

<img src="..\MDImg\Redis7-动力节点\二\2.4.4.4)-1eg.png" alt="img" style="zoom:67%;" />

tcp-backlog 是一个 TCP 连接的队列，其主要用于解决高并发场景下客户端**慢连接(连接速度慢)**问题。这里设置的值就是这个队列的长度。该队列与 TCP 连接的三次握手有关。不同的 Linux 内核，backlog 队列中存放的元素（客户端连接）类型是不同的。

- **Linux 内核 2.2 版本之前**，该队列中存放的是已完成了第一次握手的所有客户端连接
  其中就包含已完成三次握手的客户端连接。当然，此时的 backlog 队列中的连接也具有两种状态：

  1. 未完成三次握手的连接状态为 SYN_RECEIVED
  2. 已完成三次握手的连接状态为 ESTABLISHED

  只有 ESTABLISHED 状态的连接才会被 Redis 处理。

- **Linux 内核 2.2 版本之后 **TCP 系统中维护了两个队列：SYN_RECEIVED 队列与 ESTABLISHED队列。SYN_RECEIVED 队列中存放的是未完成三次握手的连接，ESTABLISHED 队列中存放的是已完成三次握手的连接。
  此时的 backlog 就是 ESTABLISHED 队列。

三次握手(补充)
<img src="..\MDImg\Redis7-动力节点\二\2.4.4.4)-6eg.png" alt="img" style="zoom:100%;" />

查看 Linux 内核版本：
<img src="..\MDImg\Redis7-动力节点\二\2.4.4.4)-2eg.png" alt="img" style="zoom:100%;" />

TCP 中的 backlog 队列的长度在 Linux 中由内核参数 `somaxconn `来决定。所以，<mark>在 Redis中该队列的长度由 Redis 配置文件设置与 somaxconn 来共同决定：取它们中的最小值。</mark>

查看当前 Linux 内核中 somaxconn 的值:
<img src="..\MDImg\Redis7-动力节点\二\2.4.4.4)-3eg.png" alt="img" style="zoom:67%;" />

**生产环境下**（特别是高并发场景下），**backlog 的值最好要大**一些，否则可能会影响系统性能。
修改/etc/sysctl.conf 文件`vim /etc/sysctl.conf`，在文件最后添加如下内容：`net.core.somaxconn=2048`
<img src="..\MDImg\Redis7-动力节点\二\2.4.4.4)-4eg.png" alt="img" style="zoom:67%;" />
修改过后可以重启虚拟机，也可以通过执行如下命令来使新的修改生效。`sysctl -p`(在不重启虚拟机的情况下重新加载配置文件)
<img src="..\MDImg\Redis7-动力节点\二\2.4.4.4)-5eg.png" alt="img" style="zoom:67%;" />

##### 5) timeout

<img src="..\MDImg\Redis7-动力节点\二\2.4.4.5)-1eg.png" alt="img" style="zoom:67%;" />空闲超时。当客户端与 Redis 间的<mark>空闲时间超过该时长后，连接自动断开</mark>。单位秒。**默认值为 0，表示永远不超时**

> eg：如果设置超时三分钟，则表示服务端与客户端在3分钟内没有数据通信， 连接会自动断开

##### 6) tcp-keepalive

<img src="..\MDImg\Redis7-动力节点\二\2.4.4.6)-1eg.png" alt="img" style="zoom:67%;" />
该配置执行的前提是`timeout 0`永不超时

该配置主要用于设置 Redis<mark> 检测与其连接的所有客户端的存活性时间间隔，单位秒</mark>。一般是在空闲超时 timeout 设置为 0 时进行配置。

> 解释：
> `tcp-keepout 300`：当`timeout 0`永不断开时，每300秒检测一次客户端，当发现客户端不在，在300秒后第二次检测客户端还不在，就断开连接

#### 2.4.5 general

##### 1) daemonize

<img src="..\MDImg\Redis7-动力节点\二\2.4.5.1)-1eg.png" alt="img" style="zoom:67%;" />该配置可以控制 Redis 启动是否采用<mark>守护进程方式</mark>，即是否是后台启动。yes 是采用**后台启动**。

##### 2) pidfile

<img src="..\MDImg\Redis7-动力节点\二\2.4.5.2)-1eg.png" alt="img" style="zoom:67%;" />该配置用于指定 Redis **运行时 pid 写入的文件**，无论 Redis 是否采用守护进程方式启动，pid 都会写入到该配置的文件。

注意，**如果没有配置 pid 文件**，不同的启动方式，pid 文件的产生效果是不同的：

- 采用守护进程方式启动即后台启动（daemonize 为 yes）：pid 文件为/var/run/redis.pid。 
- 采用前台启动（daemonize 为 no）：不生产 pid 文件

##### 3) loglevel

<img src="..\MDImg\Redis7-动力节点\二\2.4.5.3)-1eg.png" alt="img" style="zoom:67%;" /><mark>配置日志的级别</mark>。Redis 中共有四个级别，由低到高依次是：

- debug：可以获取到很多的信息，一般在开发和测试时使用。
- verbose：可以获取到很多不太有用的信息，但不像 debug 级别那么多。
- notice：可以获取到在生产中想获取到的适当多的信息，默认级别。
- warning：只记录非常重要/关键的信息

##### 4) logfile

<img src="..\MDImg\Redis7-动力节点\二\2.4.5.4)-1eg.png" alt="img" style="zoom:67%;" /><mark>指定日志文件。</mark>

- 如果设置为空串，且是后台运行，则强制将日志记录到标准输出设备（显示器）。
- 如果使用的是守护进程启动方式，设置为空串，则意味着会将日志发送到设备/dev/null（空设备）即不显示。 

##### 5) darabases

<img src="..\MDImg\Redis7-动力节点\二\2.4.5.5)-1eg.png" alt="img" style="zoom:67%;" />
<mark>设置数据库的数量</mark>。默认数据库是 0 号数据库。可以使用 `select <dbid>`在每个连接的基础上选择一个不同的数据库，其中 dbid 是介于 0 和'databases'-1'之间的数字。

#### 2.4.6 security

<img src="..\MDImg\Redis7-动力节点\二\2.4.6.1)-1eg.png" alt="img" style="zoom:67%;" />
**用户设置 ACL 权限、Redis 访问密码相关配置**。该模块中最常用的就是 `requirepass `属性。
<img src="..\MDImg\Redis7-动力节点\二\2.4.6.1)-2eg.png" alt="img" style="zoom:67%;" />
<mark> `requirepass `设置客户端访问密码</mark>。注释掉后则没有密码。

#### 2.4.7 clients

<img src="..\MDImg\Redis7-动力节点\二\2.4.7-1eg.png" alt="img" style="zoom:67%;" />该模块用于设置与客户端相关的属性，其中**仅包含一个属性 `maxclients`。**

- maxclients 用于设置 Redis <mark>可并发处理的客户端连接数量</mark>，默认值为 10000。如果达到了该最大连接数，则会拒绝再来的新连接，并返回一个异常信息：已达到最大连接数。
- 注意，该值不能超过 Linux 系统支持的可打开的文件描述符最大数量阈值。查看该阈值的方式如下。修改该值，可以通过修改`/etc/secutiry/limits.conf 文件`（自己去改）。
  <img src="..\MDImg\Redis7-动力节点\二\2.4.7-2eg.png" alt="img" style="zoom:67%;" />

#### 2.4.8 memory management 内存管理

<img src="..\MDImg\Redis7-动力节点\二\2.4.8-1eg.png" alt="img" style="zoom:67%;" />
该配置可以控制最大可用内存及相关内容移除问题。

##### 1) maxmermory

<img src="..\MDImg\Redis7-动力节点\二\2.4.8.1)-1eg.png" alt="img" style="zoom:67%;" />将内存使用限制设置为指定的字节数。**当达到内存限制时**，Redis 将**根据选择的逐出策略** maxmemory-policy 尝试**删除**符合条件的 **key。**
如果不能按照逐出策略移除 key，则会给写操作命令返回 `error`，但对于只读的命令是没有影响的

##### 2) maxmamory-policy

 maxmermory的**逐出策略**
<img src="..\MDImg\Redis7-动力节点\二\2.4.8.2)-1eg.png" alt="img" style="zoom:67%;" />该属性用于设置，当达到 maxmemory 时，Redis 将如何选择要移除的内容。当然，如果没有符合相应策略的内容要删除，则在执行写入命令时会给出 errors 的响应。Redis 中共支持 8 种移除策略：

- volatile-lru：使用近似** LRU** 算法移除，仅适用于**设置了过期时间的 key。 **
- allkeys-lru：使用近似** LRU** 算法移除，可适用于**所有类型的 key。 **
- volatile-lfu：使用近似 **LFU** 算法移除，仅适用于**设置了过期时间的 key。 **
- allkeys-lfu：使用近似 **LFU** 算法移除，可适用于**所有类型的 key。 **
- volatile-random：**随机**移除一个 key，仅适用于**设置了过期时间的 key。** 
- allkeys-random：**随机**移除一个 key，可适用于**所有类型的 key。 **
- volatile-ttl：移除**距离过期时间最近的 key。** 
- noeviction：不移除任何内容，**只**是在写操作时**返回一个错误**，默认值。

##### 3) maxmemory-samples

<img src="..\MDImg\Redis7-动力节点\二\2.4.8.3)-1eg.png" alt="img" style="zoom:67%;" />该属性用于指定挑选要删除的 key 的样本数量。样本的选择采用的是 LRU 算法，其不能修改。但从样本中再选择要移除的 key，则采用的是 maxmamory-policy 指定的策略。

> `maxmemory-samples 5`：先选择5个key的样本，在使用指定的逐出策略在这五个样本中移除
> maxmemory-samples   3：很快但不准确，10：非常接近真正的LRU算法但要耗费更多的CPU，我们采用折中的5

##### 4) maxmemory-eviction-tenacity

<img src="..\MDImg\Redis7-动力节点\二\2.4.8.4)-1eg.png" alt="img" style="zoom:67%;" /><mark>设置移除容忍度(找到需要移除的样本后，能容忍它存在的度)</mark>
数值越小表示容忍度越低，需要移除的数据移除延迟越小；
数值越大表示容忍度越高，需要移除的数据移除延迟越大。

maxmemory-eviction-tenacity 0最小延迟，10是默认值，100已经不再考虑移除问题了

#### 2.4.9 threaded I/O

<img src="..\MDImg\Redis7-动力节点\二\2.4.9-1eg.png" alt="img" style="zoom:67%;" />
该配置模块用于配置 Redis 对多线程 IO 模型的支持

##### 1) io-threads

<img src="..\MDImg\Redis7-动力节点\二\2.4.9.1)-1eg.png" alt="img" style="zoom:67%;" />该属性用于指定要启用多线程 IO 模型时，<mark>要使用的线程数量。</mark>

查看当前系统中包含的 CPU 数量`lscpu`：
<img src="..\MDImg\Redis7-动力节点\二\2.4.9.1)-2eg.png" alt="img" style="zoom:67%;" />

##### 2) io-threads-do-reads

<img src="..\MDImg\Redis7-动力节点\二\2.4.9.2)-2eg.png" alt="img" style="zoom:67%;" />
该属性用于**启用**多线程 IO 模型中的多线程**处理读请求的能力**。
一般来说这个东西保持默认no就行了