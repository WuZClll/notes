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

## 三. Redis 命令

Redis 根据命令所操作对象的不同，可以分为三大类：对 Redis 进行基础性操作的命令，对 Key 的操作命令，对 Value 的操作命令。

### 3.1 Redis 基本命令

首先通过` redis-cli` 命令进入到 Redis 命令行客户端，然后再运行下面的命令。

#### 3.1.1 `ping`心跳命令 

键入 ping 命令，会看到 PONG 响应，则说明该客户端与 Redis 的连接是正常的。该命令亦称为心跳命令。
<img src="..\MDImg\Redis7-动力节点\三\3.1.1-1eg.png" alt="img" style="zoom:67%;" />

#### 3.1.2 `set`,`get`读写键值命令

`set key value` 会将指定 key-value 写入到 DB。`get key` 则会读取指定 key 的 value 值。关于更多 set 与 get 命令格式，后面会详细学习。
 <img src="..\MDImg\Redis7-动力节点\三\3.1.2-1eg.png" alt="img" style="zoom:67%;" />

#### 2.1.3  `select`数据库切换

Redis 默认有 16 个数据库。这个在 Redis Desktop Manager（RDM）图形客户端中可以直观地看到。
<img src="..\MDImg\Redis7-动力节点\三\3.1.3-1eg.png" alt="img" style="zoom:67%;" />
默认使用的是 0 号 DB，可以通过 `select db` 索引来切换 DB。

例如，如`select 3`会切换到DB3，并会将 age-23 写入到 DB3 中。
<img src="..\MDImg\Redis7-动力节点\三\3.1.3-2eg.png" alt="img" style="zoom:67%;" />
并且这个结果在 RDM 中是可以直观地看到的。
<img src="..\MDImg\Redis7-动力节点\三\3.1.3-3eg.png" alt="img" style="zoom:67%;" />

#### 3.1.4 `dbsize`查看 key 的数量 

`dbsize` 命令可以查看当前数据库中 key 的数量。
<img src="..\MDImg\Redis7-动力节点\三\3.1.4-1eg.png" alt="img" style="zoom:67%;" />
从以上查看情况看，DB0 中有 2 个 key，DB1 中没有 key，DB3 中有 1 个 key。

#### 3.1.5  `flushdb`删除当前库中数据

`flushdb` 命令仅仅删除的是当前数据库中的数据，不影响其它库。
<img src="..\MDImg\Redis7-动力节点\三\3.1.5-1eg.png" alt="img" style="zoom:67%;" />

#### 3.1.6 `flushall`删除所有库中数据命令 

`flushall` 命令可以删除所有库中的所有数据。所以该命令的使用一定要慎重。
<img src="..\MDImg\Redis7-动力节点\三\3.1.6-1eg.png" alt="img" style="zoom:67%;" />

#### 3.1.7 `exit` 或 `quit` 退出客户端命令

使用 `exit` 或 `quit` 命令均可退出 Redis 命令行客户端。
<img src="..\MDImg\Redis7-动力节点\三\3.1.7-1eg.png" alt="img" style="zoom:67%;" />

### 3.2 Key 操作命令

Redis 中存储的数据整体是一个 Map，其 key 为 String 类型，而 value 则可以是 String、Hash 表、List、Set 等类型

#### 3.2.1 `keys` 检索键

- 格式：`KEYS pattern`
- 功能：查找所有符合给定模式 pattern 的 key，**pattern 为正则表达式**。
- 说明：KEYS 的速度非常快，<mark>但在一个大的数据库中使用它可能会**阻塞**当前服务器的服务</mark>。所以生产环境中**一般不使用**该命令，而使用 scan 命令代替。

#### 3.2.2 `exists` 是否存在 key

- 格式：`EXISTS key`
- 功能：检查给定 key**是否存在**。
- 说明：若 key 存在，返回 1 ，否则返回 0 。

#### 3.2.3 `del` 删除key

- 格式：`DEL key [key ...]`
- 功能：删除给定的一个或多个 key 。不存在的 key 会被忽略。
- 说明：返回被删除 key 的数量。
- eg:`del key1 key2`

#### 3.2.4 `rename` 修改 key 名

- 格式：`RENAME key newkey`
- 功能：将 key 改名为 newkey。 
- 说明：
  - 当 key 和 newkey 相同，或者 key 不存在时，返回一个错误。
  - 当 newkey 已经存在时， RENAME 命令将覆盖旧值。改名成功时提示 OK ，失败时候返回一个错误。

#### 3.2.5 `move` 移动键所在库

- 格式：`MOVE key db`
- 功能：将当前数据库的 key 移动到给定的数据库 db 当中。
- 说明：如果当前数据库(源数据库)和给定数据库(目标数据库)有相同名字的给定 key ，或者 key 不存在于当前数据库，那么 MOVE 没有任何效果。移动成功返回 1 ，失败则返回 0 。

#### 3.2.6 `type` 检查键所对应值的类型

- 格式：`TYPE key`
- 功能：返回 key 所储存的**值的类型**。
- 说明：返回值有以下六种
  - none (key 不存在) 
  - string (字符串) 
  - list (列表) 
  - set (集合) 
  - zset (有序集) 
  - hash (哈希表)

#### 3.2.7 `expire` 与 `pexpire` 设置key生存时间

- 格式：`EXPIRE key seconds`
- 功能：为给定 key 设置生存时间。当 key 过期时(生存时间为 0)，它会被自动删除。
- 注意：
  - `expire `的时间单位为**秒**，`pexpire` 的时间单位为**毫秒**。
  - 在 Redis 中，带有生存时间的 key被称为“易失的”(volatile)。 
- 说明：生存时间设置成功返回 1。若 key 不存在时返回 0 。rename 操作不会改变 key的生存时间。

#### 3.2.8 `ttl` 与 `pttl` 返回 key 的生存时间

- 格式：`TTL key`
- 功能：TTL, time to live，**返回给定 key 的剩余生存时间。**
- 说明：其返回值存在三种可能：
  - 当 key 不存在时，返回 -2 。 
  - 当 key 存在但没有设置剩余生存时间时，返回 -1 。 
  - 否则，返回 key 的剩余生存时间。ttl 命令返回的时间单位为秒，而 pttl 命令返回的时间单位为毫秒。

#### 3.2.9 `persist` 持久化

+ 格式：`PERSIST key`
+ 功能：**去除给定 key 的生存时间**，将这个 key 从“易失的”转换成“持久的”。 
+ 说明：当生存时间移除成功时，返回 1；若 key 不存在或 key 没有设置生存时间，则返回 0。

#### 3.2.10 `randomkey` 返回随机key

- 格式：`RANDOMKEY`
- 功能：从当前数据库中随机返回(不删除)一个 key。 
- 说明：当数据库不为空时，返回一个 key。当数据库为空时，返回 nil。

`3.2.11 scan`

+ 格式：
  `SCAN cursor [MATCH pattern] [COUNT count] [TYPE type]`
  scan 开始游标 [match 匹配模式]  [count 返回个数] [type 返回value类型]

+ 功能：用于**迭代数据库中的数据库键**。
  其各个选项的意义为：

  - cursor：本次迭代**开始**的游标。
  - pattern ：本次迭代要**匹配**的 key 的**模式**。
  - count ：本次迭代要从数据集里返回**多少元素**，默认值为 10 。 
  - type：本次迭代**要返回的** value 的**类型**，默认为所有类型。

  `SCAN` 命令是一个基于游标 cursor 的迭代器：
  `SCAN` 命令每次被调用之后，都会向用户返回返回一个包含两个元素的数组， 
  第一个元素是用于进行下一次迭代的新游标,
  第二个元素则是一个数组， 这个数组中包含了所有被迭代的元素。用户在下次迭代时需要使用这个新游标作为 `SCAN` 命令的游标参数，以此来延续之前的迭代过程。
  当`SCAN` 命令的游标参数被设置为 0 时，服务器将开始一次新的迭代。如果新游标返回 0 表示迭代已结束。

  + 说明：使用间断的、负数、超出范围或者其他非正常的游标来执行增量式迭代不会造成服务器崩溃。
    当**数据量很大时**，**`count` 的数量的指定可能会不起作用**，Redis 会自动调整每次的遍历数目。由于 scan 命令每次执行都只会返回少量元素，所以该命令可以用于生产环境，而不会出现像 `KEYS` 命令带来的服务器阻塞问题。
    增量式迭代命令所使用的算法只保证在**数据集的大小有界的情况下迭代才会停止**，换句话说，如果被迭代数据集的大小不断地增长的话，增量式迭代命令可能永远也无法完成一次完整迭代。即当一个数据集不断地变大时，想要访问这个数据集中的所有元素就需要做越来越多的工作， **能否结束一个迭代取决于用户执行迭代的速度是否比数据集增长的速度更快**。

+ 相关命令：另外还有 3 个 scan 命令用于对三种类型的 value 进行遍历。

  - `hscan`：属于 Hash 型 Value 操作命令集合，用于遍历当前 db 中指定 Hash 表的所有 field-value 对。即数据库键值对中值是hash型的时候遍历这个hash型的值
  - `sscan`：属于 Set 型 Value 操作命令集合，用于遍历当前 db 中指定 set 集合的所有元素
  - `zscan`：属于 ZSet 型 Value 操作命令集合，用于遍历当前 db 中指定有序集合的所有元素（数值与元素值）

### 3.3 String 型 `Value` 操作命令

Redis 存储数据的 Value 可以是一个 String 类型数据。String 类型的 Value 是 Redis 中最基本，最常见的类型。String 类型的 Value 中可以存放任意数据，包括数值型，甚至是二进制的图片、音频、视频、序列化对象等。一个 String 类型的 Value 最大是 512M 大小。

#### 3.3.1 `set`

- 格式：`SET key value [EX seconds | PX milliseconds] [NX|XX]`
- 功能：`SET `除了可以**直接将 key 的值设为 value **外，还可以指定一些参数。
  - `EX seconds`：为当前 key 设置**过期时间**，单位**秒**。等价于` SETEX `命令。
  - `PX milliseconds`：为当前 key 设置**过期时间**，单位**毫秒**。等价于 `PSETEX` 命令。
  - `NX`：指定的 **key 不存在才会设置成功**，**用于添加**指定的 key。等价于` SETNX` 命令。
  - `XX`：指定的**key 必须存在才会设置成功**，**用于更新**指定 key 的 value。 
- 说明：如果 value 字符串中带有空格，则该字符串需要使用**双引号或单引号**引起来，否则会认为 set 命令的参数数量不正确，报错。

#### 3.3.2 `setex` 与 `psetex`设置值和生存时间

- 格式：`SETEX/PSETEX key seconds value`
- eg: `setex name 3 zhangsan`
- 功能：`set expire`，**其不仅为 key 指定了 value，还为其设置了生存时间**。`setex` 的单位为**秒**，`psetex` 的单位为**毫秒**。 
- 说明：如果 **key 已经存在**， 则**覆写旧值**。该命令类似于以下两个命令，不同之处是，SETEX 是一个原子性操作，关联值和设置生存时间两个动作会在同一时间内完成，该命令在 Redis 用作缓存时，非常实用。
  `SET key value`
  `EXPIRE key seconds # 设置生存时间`

#### 3.3.3 `setnx` 不存在时

- 格式：`SETNX key value`
- 功能：SET if Not eXists，**将 key 的值设为 value ，当且仅当 key 不存在。**
  若给定的 **key已经存在**，则 SETNX **不做任何动作**。成功，返回 1，否则，返回 0。
- 说明：该命令等价于 `set key value nx`

#### 3.3.4 `getset`设置值的同时返回旧值

- 格式：`GETSET key value`
- 功能：**将给定 key 的值设为 value** ，并**返回 **key 的**旧值**。
-  说明：当 key 存在但不是字符串类型时，返回一个错误；当 key 不存在时，返回 nil 。

#### 3.3.5 `mset `与 `msetnx`多个键值对

- 格式：`MSET/MSETNX key value [key value ...]`

- 功能：同时设置一个或多个 key-value 对。

- 说明：
  如果**某个给定 key 已经存在**，

  - **`MSET` **会用新值***覆盖*原来的旧值**，
  - **`MSETNX`** 命令：它只会在所有给定**key *都不存在*的情况下进行设置操作**。

  `MSET/MSETNX` 是一个原子性(atomic)操作，**所有给定 key 都会在同一时间内被设置**，某些给定 key 被更新而另一些给定 key 没有改变的情况不可能发生。该命令永不失败。

#### 3.3.6 `mget`获取多个

- 格式：`MGET key [key ...]`
- 功能：**返回所有(一个或多个)给定 key 的值。**
- 说明：如果给定的 key 里面，有某个 key 不存在，那么这个 key 返回特殊值 nil 。因此，该命令永不失败。

#### 3.3.7 `append`追加到值的末尾

- 格式：`APPEND key value`
- 功能：如果 key 已经存在并且是一个字符串， APPEND 命令将 value **追加到** key 原来的**值的末尾**。如果 key 不存在， APPEND 就简单地将给定 key 设为 value ，就像执行 SET key value 一样。
- 说明：追加 value 之后， key 中字符串的长度。

#### 3.3.8 `incr` 与 `decr` value数字自增自减1

- 格式：`INCR key 或 DECR key`
- 功能：
  **increment，自动递增。将 key 中存储的数字值增一。
  decrement，自动递减。将 key 中存储的数字值减一。**
- 说明：
  如果**key 不存在**，那么 key 的值会先被**初始化为 0**，然后**再执行增一/减一**操作。
  如果**值不能表示为数字**，那么**返回一个错误**提示。如果执行正确，则返回增一/减一后的值。

#### 3.3.9 `incrby` 与 `decrby`value数字整数自增自减n

- 格式：`INCRBY key increment 或 DECRBY key decrement`
- eg: `incrby age 3`：age的值一次加三
- 功能：**将 key 中存储的数字值增加/减少指定的数值，这个数值只能是整数，可以是负数，但不能是小数**。
- 说明：如果 key 不存在，那么 key 的值会先被初始化为 0，然后再执行增/减操作。如果值不能表示为数字，那么返回一个错误提示。如果执行正确，则返回增/减后的值。

#### 3.3.10` incrbyfloat`数字浮点数自增自减n

- 格式：`INCRBYFLOAT key increment`
- 功能：**为 key 中所储存的值加上浮点数增量 increment 。 **
- 说明：与之前的说明相同。没有 decrbyfloat 命令，但 **increment 为负数**可以**实现减**操作效果。

#### 3.3.11 `strlen` 返回value的长度

- 格式：`STRLEN key`
- 功能：返回 key 所储存的字符串值的长度。
- 说明：当 key 储存的不是字符串值时，返回一个错误；当 key 不存在时，返回 0 。

#### 3.3.12 `getrange`截取字符串

- 格式：`GETRANGE key start end`
  <img src="..\MDImg\Redis7-动力节点\三\3.3.13-1eg.png" alt="img" style="zoom:67%;" />
- 功能：返回 key 中字符串值的子字符串，字符串的**截取**范围由 start 和 end 两个偏移量决定，**包括 start 和 end 在内。**
- 说明：**end 必须要比 start 大**。支持**负数偏移量，表示从字符串最后开始计数**，-1 表示最后一个字符，-2 表示倒数第二个，以此类推。

#### 3.3.13 `setrange`从值的索引处开始替换值

- 格式：`SETRANGE key offset value`
- 功能：用 value 参数替换给定 key 所储存的字符串值 str，从偏移量 offset 开始。
- 说明：当 offset 值大于 str 长度时，中间使用零字节`\x00` 填充，即 0000 0000 字节填充；对于不存在的 key 当作空串处理。

#### 3.3.14 位操作命令

名称中包含 BIT 的命令，都是对二进制位的操作命令，例如，setbit、getbit、bitcount、

bittop、bitfield，这些命令不常用。

#### 3.3.15 典型应用场景

Value 为 String 类型的应用场景很多，这里仅举这种典型应用场景的例子： 

##### （1） 数据缓存

Redis **作为数据缓存层**，MySQL 作为数据存储层。应用服务器首先从 Redis 中获取数据，**如果缓存层中没有，则从 MySQL 中获取后先存入缓存层再返回给应用服务器。**

##### （2） 计数器

在 Redis 中写入一个 value 为数值型的 key 作为平台计数器、视频播放计数器等。每个有效客户端访问一次，或视频每播放一次，都是直接修改 Redis 中的计数器，然后再以异步方式持久化到其它数据源中，例如持久化到 MySQL。 

##### （3） 共享 Session

<img src="..\MDImg\Redis7-动力节点\三\3.3.15-1eg.png" alt="img" style="zoom:67%;" />

对于一个分布式应用系统，如果将类似用户登录信息这样的 Session 数据保存在提供登录服务的服务器中，那么如果用户再次提交像收藏、支付等请求时可能会出现问题：在提供收藏、支付等服务的服务器中并没有该用户的 Session 数据，从而导致该用户需要重新登录。对于用户来说，这是不能接受的。
此时，可以将系统中所有用户的 Session 数据全部保存到 Redis 中，用户在提交新的请求后，系统先从 Redis 中查找相应的 Session 数据，如果存在，则再进行相关操作，否则跳转到登录页面。这样就不会引发“重新登录”问题。

##### （4） 限速器

- 现在很多平台为了防止 DoS（Denial of Service，拒绝服务）攻击，**一般都会限制一个 IP不能在一秒内访问超过 n 次**。而 **Redis 可以可以结合 key 的过期时间与 incr 命令来完成限速功能，充当限速器。**
- 注意，其**无法防止 DDoS**（Distributed Denial of Service，分布式拒绝服务）**攻击。**

```java
// 客户端每提交一次请求，都会执行下面的代码
// 等价于 set 192.168.192.55 1 ex 60 nx
// 指定新 ip 作为 key 的缓存过期时间为 60 秒
Boolean isExists = redis.set(ip, 1, “EX 60”, “NX”);
//set失败会返回null
if(isExists != null || redis.incr(ip) <= 5) {
 // 通过
} else {
    // 限流
}
```

### 3.4 Hash 型 Value 操作命令

Redis 存储数据的 Value 可以是一个 Hash 类型。Hash 类型也称为 Hash 表、字典等。
Hash 表就是一个映射表 Map，也是由键-值对构成，为了与整体的 key 进行区分，这里的键称为 field，值称为 value。注意，Redis 的 Hash 表中的 field-value 对均为 String 类型。

#### 3.4.1 `hset` 

- 格式：`HSET key field value`
- 功能：将哈希表 **key 中的域 field 的值设为 value**
- 说明：如果 key 不存在，一个新的哈希表被创建并进行 HSET 操作。**如果域 **field **已经存在**于哈希表中，**旧值将被覆盖**。如果 field 是哈希表中的一个新建域，并且值设置成功，返回 1 。如果哈希表中域 field 已经存在且旧值已被新值覆盖，返回 0 。

#### 3.4.2 `hget` 

- 格式：`HGET key field`
- 功能：**返回**哈希表 **key 中给定域 field 的值。**
- 说明：当给定域不存在或是给定 key 不存在时，返回 nil 。

#### 3.4.3 `hmset`设置多个

- 格式：`HMSET key field value [field value ...]`
- 功能：**同时将多个 field-value (域-值)对设置到哈希表 key 中。**
-  说明：此命令会覆盖哈希表中已存在的域。如果 key 不存在，一个空哈希表被创建并执行 HMSET 操作。如果命令执行成功，返回 OK 。当 key 不是哈希表(hash)类型时，返回一个错误。

#### 3.4.4 `hmget`返回多个

- 格式：`HMGET key field [field ...]`
- 功能：**按照给出顺序返回哈希表 key 中一个或多个域的值。**
- 说明：如果给定的域不存在于哈希表，那么返回一个 nil 值。因为不存在的 key 被当作一个空哈希表来处理，所以对一个不存在的 key 进行 HMGET 操作将返回一个只带有 nil 值的表。

#### 3.4.5 `hgetall`返回所有(不建议)

- 格式：`HGETALL key`
- 功能：**返回哈希表 key 中所有的域和值。**
- 说明：在返回值里，紧跟每个域名(field name)之后是域的值(value)，所以返回值的长度是哈希表大小的两倍。若 key 不存在，返回空列表。若 key 中包含大量元素，则该命令可能会阻塞 Redis 服务。所以生产环境中**一般不使用该命令**，而使用 hscan 命令代替。

#### 3.4.6 `hsetnx`不存在时

- 格式：`HSETNX key field value`
- 功能：将哈希表**key 中的域 field 的值设置为 value ，当且仅当域 field 不存在。**
- 说明：若域 field 已经存在，该操作无效。如果 key 不存在，一个新哈希表被创建并执行 HSETNX 命令。

#### 3.4.7 `hdel` 删除多个

- 格式：`HDEL key field [field ...]`
- 功能：**删除哈希**表 key 中的一个或多个指定域，不存在的域将被忽略。
- 说明：返回被成功移除的域的数量，不包括被忽略的域。

#### 3.4.8 `hexits`是否存在

- 格式：`HEXISTS key field`
- 功能：**查看哈希表 key 中给定域 field 是否存在。**
- 说明：如果哈希表含有给定域，返回 1 。如果不含有给定域，或 key 不存在，返回 0 。

#### 3.4.9 `hincrby` 与 `hincrbyfloat`自增整数(浮点型)量

- 格式：`HINCRBY key field increment`
- 功能：**为哈希表 key 中的域 field 的值加上增量 increment 。*hincrby 命令只能增加整数值*，而 *hincrbyfloat 可以增加小数值*。**
- 说明：增量也可以为负数，相当于对给定域进行减法操作。如果 key 不存在，一个新的哈希表被创建并执行 HINCRBY 命令。如果域 field 不存在，那么在执行命令前，域的值被初始化为 0。对一个储存字符串值的域 field 执行 HINCRBY 命令将造成一个错误。

#### 3.4.10 `hkeys` 与 `hvals` 返回所有

- 格式：`HKEYS key` 或 `HVALS key`
- 功能：返回哈希表 key 中的所有域/值。 
- 说明：当 key 不存在时，返回一个空表。

#### 3.4.11 `hlen`获取key中field的数量

- 格式：`HLEN key`
- 功能：返回哈希表 key 中域的数量。
- 说明：当 key 不存在时，返回 0 。

#### 3.4.12 `hstrlen`获取关联值的长度

- 格式：`HSTRLEN key field`
- 功能：返回哈希表 key 中， 与给定域 field 相关联的值的字符串长度（string length）。
- 说明：如果给定的键或者域不存在， 那么命令返回 0 。

#### 3.4.13 应用场景

Hash 型 Value 非常**适合存储对象数据**。key 为对象名称，value 为描述对象属性的 Map，对对象属性的修改在 Redis 中就可直接完成。其不像 String 型 Value 存储对象，那个对象是序列化过的，例如序列化为 JSON 串，对对象属性值的修改需要先反序列化为对象后再修改，修改后再序列化为 JSON 串后写入到 Redis

### 3.5 List 型 Value 操作命令

Redis 存储数据的 Value 可以是一个 String 列表类型数据。即该列表中的每个元素均为String 类型数据。列表中的数据会**按照插入顺序进行排序**。不过，该列表的底层实际是一个**无头节点的双向链表**，所以对列表表头与表尾的操作性能较高，但**对中间元素的插入与删除**的操作**的性能相对较差**。

#### 3.5.1 `lpush/rpush`

- 格式：`LPUSH key value [value ...]`
  或 `RPUSH key value [value ...]`

- 功能：将**一个或多个值 **value **插入**到列表 key 的**表头/表尾**（表头在左表尾在右）

- 说明：
  如果有多个 value 值，

  - 对于 lpush 来说，各个 value 会按从左到右的顺序依次插入到表头；
  - 对于 rpush 来说，各个 value 会按从左到右的顺序依次插入到表尾。

  如果 key不存在，一个空列表会被创建并执行操作。当 key 存在但不是列表类型时，返回一个错误。执行成功时返回列表的长度。

#### 3.5.2 `llen`链表长度

- 格式：`LLEN key`
- 功能：返回**列表 key 的长度。**
- 说明：如果 key 不存在，则 key 被解释为一个空列表，返回 0 。如果 key 不是列表类型，返回一个错误。

#### 3.5.3` lindex`

- 格式：`LINDEX key index`
- 功能：**返回**列表 key 中，**下标为 index 的元素**。列表从 0 开始计数。
- 说明：如果 index 参数的值不在列表的区间范围内(out of range)，返回 nil 。

#### 3.5.4 `lset`

- 格式：`LSET key index value`
- 功能：将列表 key **下标为 index 的元素**的值**设置为 value** 。 
- 说明：当 index 参数超出范围，或对一个空列表(key 不存在）进行 LSET 时，返回一个错误。

#### 3.5.5 `lrange`

- 格式：`LRANGE key start stop`
- 功能：**返回**列表 key 中**指定区间[start, stop]内的元素**，即包含两个端点。 
- 说明：List 的下标从 0 开始，即以 0 表示列表的第一个元素，以 1 表示列表的第二个元素，以此类推。也可以使用负数下标，以 **-1 表示列表的最后一个元素**， -2 表示列表的倒数第二个元素，以此类推。超出范围的下标值不会引起错误。如果 start 下标比列表的最大下标 还要大，那么 LRANGE 返回一个空列表。如果 stop 下标比最大下标还要大，Redis 将 stop 的值设置为最大下标。

#### 3.5.6 `lpushx` 与 `rpushx`存在时

- 格式：`LPUSHX key value 或 RPUSHX key value`
- 功能：将值 value **插入**到列表 key 的**表头/表尾**，当且仅当 key 存在并且是一个列表。
- 说明：当 **key 不存在时，命令什么也不做**。若执行成功，则输出表的长度。

#### 3.5.7 `linsert`之前/后插入数据

- 格式：`LINSERT key BEFORE|AFTER pivot value`
- 功能：将值 value 插入到列表 key 当中，位于元素 pivot 之前或之后。
- 说明：当 pivot 元素不存在于列表中时，不执行任何操作，返回-1；当 key 不存在时，key 被视为空列表，不执行任何操作，返回 0；如果 key 不是列表类型，返回一个错误；如果命令执行成功，返回插入操作完成之后，列表的长度。

#### 3.5.8 `lpop / rpop`左/右移除n个元素

- 格式：`LPOP key [count] `
  `或 RPOP key [count]`
- 功能：**从列表 key 的表头/表尾移除 count 个元素**，并返回移除的元素。count 默认值 1 
- 说明：当 key 不存在时，返回 nil

#### 3.5.9 `blpop / brpop`阻塞式

- 格式：`BLPOP key [key ...] timeout 或 BRPOP key [key ...] timeout`
- 功能：BLPOP/BRPOP 是列表的阻塞式(blocking)弹出命令。它们是 LPOP/RPOP 命令的阻塞版本，当给定列表内没有任何元素可供弹出的时候，连接将被 BLPOP/BRPOP 命令阻塞，直到等待 timeout 超时或发现可弹出元素为止。**当给定多个 key 参数时，按参数 key的先后顺序依次检查各个列表，弹出第一个非空列表的头元素。**timeout 为阻塞时长，单位为秒，其值若为 0，则表示只要没有可弹出元素，则一直阻塞。
- 说明：假如在指定时间内没有任何元素被弹出，则返回一个 nil 和等待时长。反之，返回一个含有两个元素的列表，第一个元素是被弹出元素所属的 key ，第二个元素是被弹出元素的值。

#### 3.5.10` rpoplpush`

- 格式：`RPOPLPUSH source destination`
- 功能：命令 RPOPLPUSH 在一个原子时间内，执行以下两个动作：
  将列表 source 中的最后一个元素(尾元素)弹出，并返回给客户端。
  将 source 弹出的元素插入到列表 destination ，作为 destination 列表的的头元素。
  即**交换位置，将source最右边的元素弹出到destination最左边的元素**
  如果 source 不存在，值 nil 被返回，并且不执行其他动作。如果 source 和 destination相同，则列表中的表尾元素被移动到表头，并返回该元素，可以把这种特殊情况视作列表的旋转(rotation)操作。
-  <img src="..\MDImg\Redis7-动力节点\三\3.5.10-1eg.png" alt="img" style="zoom:100%;" />

#### 3.5.11 `brpoplpush`阻塞

- 格式：`BRPOPLPUSH source destination timeout`
- 功能：BRPOPLPUSH 是 RPOPLPUSH 的阻塞版本，当给定列表 source 不为空时，BRPOPLPUSH 的表现和 RPOPLPUSH 一样。当列表 source 为空时， BRPOPLPUSH 命令将阻塞连接，直到等待超时，或有另一个客户端对 source 执行 LPUSH 或 RPUSH 命令为止。timeout 为阻塞时长，单位为秒，其值若为 0，则表示只要没有可弹出元素，则一直阻塞。
- 说明：假如在指定时间内没有任何元素被弹出，则返回一个 nil 和等待时长。反之，返回一个含有两个元素的列表，第一个元素是被弹出元素的值，第二个元素是等待时长。

#### 3.5.12 `lrem`

- 格式：`LREM key count value`
- 功能：根据参数 count 的值，**移除列表中与参数 value 相等的元素**。count 的值可以是以下几种：
  - count **> 0** : 从**表头开始向表尾**搜索，移除与 value 相等的元素，数量为 count 。 
  - count **< 0** : 从**表尾开始向表头**搜索，移除与 value 相等的元素，数量为 count 的绝对值。
  - count **= 0 **: **移除表中所有**与 value 相等的值。
- 说明：返回被移除元素的数量。当 key 不存在时， LREM 命令返回 0 ，因为不存在的 key 被视作空表(empty list)。

#### 3.5.13 `ltrim`截取

- 格式：`LTRIM key start stop`
- 功能：对一个列表进行修剪(trim)，就是说，让列表**只保留指定区间内的元素**，不在指定区间之内的元素都将被删除。
- 说明：下标(index)参数 start 和 stop 都以 0 为底，也就是说，以 0 表示列表的第一个元素，以 1 表示列表的第二个元素，以此类推。也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此类推。当 key 不是列表类型时，返回一个错误。如果 start 下标比列表的最大下标 end ( LLEN list 减去 1 )还要大，或者 start > stop ， LTRIM 返回一个空列表，因为 LTRIM 已经将整个列表清空。如果 stop 下标比 end 下标还要大，Redis 将 stop 的值设置为 end 。

#### 3.5.14 应用场景

Value 为 List 类型的应用场景很多，主要是通过构建不同的数据结构来实现相应的业务功能。这里仅对这些数据结构的实现方式进行总结，不举具体的例子。

##### （1） 栈

通过** lpush + lpop **可以实现栈数据结构效果：先进后出。通过 lpush 从列表左侧插入数据，通过 lpop 从列表左侧取出数据。当然，通过** rpush + rpop** 也可以实现相同效果，只不过操作的是列表右侧。

##### （2） 队列

通过**lpush + rpop**可以实现队列数据结构效果：先进先出。通过 lpush 从列表左侧插入数据，通过 rpop 从列表右侧取出数据。当然，通过**rpush + lpop **也可以实现相同效果，只不过操作的方向正好相反。

##### （3） 阻塞式消息队列

通过**lpush + brpop** 可以实现阻塞式消息队列效果。作为消息生产者的客户端使用 lpush从列表左侧插入数据，作为消息消费者的多个客户端使用 brpop 阻塞式“抢占”列表尾部数据进行消费，保证了消费的负载均衡与高可用性。**brpop 的 timeout 设置为 0，表示只要没有数据可弹出，就永久阻塞**。

##### （4） 动态有限集合

通过 lpush + ltrim 可以实现有限集合。通过 lpush 从列表左侧向列表中添加数据，通过ltrim 保持集合的动态有限性。像企业的末位淘汰、学校的重点班等动态管理，都可通过这种动态有限集合来实现。当然，通过 rpush + ltrim 也可以实现相同效果，只不过操作的方向正好相反。

### 3.6 Set 型 Value 操作命令

Redis 存储数据的 Value 可以是一个 Set 集合，且集合中的每一个元素均 String 类型。Set与 List 非常相似，但不同之处是**Set 中的元素具有无序性与不可重复性**，而 List 则具有有序性与可重复性；**Set 底层是Map，而list底层是链表**

Redis 中的 Set 集合与 Java 中的 Set 集合的实现相似，其底层都是 value 为 null 的 hash表。也正因为此，才会引发无序性与不可重复性。

#### 3.6.1 `sadd`

-  格式：`SADD key member [member ...]`
-  功能：将**一个或多个** member 元素**加入**到集合 key 当中，**已经存在**于集合的 member 元素**将被忽略**。
-  说明：假如 key 不存在，则创建一个只包含 member 元素作成员的集合。当 key 不是集合类型时，返回一个错误。

#### 3.6.2 `smembers`

-  格式：`SMEMBERS key`
-  功能：**返回集合 key 中的所有成员。**
-  说明：不存在的 key 被视为空集合。若 key 中包含大量元素，则该命令可能会阻塞 Redis服务。所以生产环境中一般不使用该命令，而使用 sscan 命令代替。

#### 3.6.3 `scard`

- 格式：`SCARD key`
- 功能：返回 **Set 集合的长度**
- 说明：当 key 不存在时，返回 0 。

#### 3.6.`4 sismember`

- 格式：`SISMEMBER key member`
- 功能：**判断 member 元素是否是集合 key 的成员。**
- 说明：如果 member 元素是集合的成员，返回 1 。如果 member 元素不是集合的成员，或 key 不存在，返回 0 。

#### 3.6.5 `smove`

- 格式：`SMOVE source destination member`
- 功能：将 member 元素**从 source 集合移动到 destination 集合。**
- 说明：如果 source 集合不存在或不包含指定的 member 元素，则 SMOVE 命令不执行任何操作，仅返回 0 。否则， member 元素从 source 集合中被移除，并添加到destination 集合中去，返回 1。当 destination 集合已经包含 member 元素时， SMOVE命令只是简单地将 source 集合中的 member 元素删除。当 source 或 destination 不是集合类型时，返回一个错误。

#### 3.6.6` srem`

- 格式：`SREM key member [member ...]`
- 功能：**移除集合 key 中的一个或多个 member 元素**，不存在的 member 元素会被忽略，且返回成功移除的元素个数。
- 说明：当 key 不是集合类型，返回一个错误。

#### 3.6.7 `srandmember`

- 格式：`SRANDMEMBER key [count]`
- 功能：返回**集合中的 count 个随机元素**。count 默认值为 1。 
- 说明：**若 count 为<mark>正数</mark>，且小于集合长度，那么返回一个包含 count 个元素的数组，数组中的元素<mark>各不相同</mark>**。如果 count 大于等于集合长度，那么返回整个集合。**如果count 为<mark>负数</mark>，那么返回一个包含 count 绝对值个元素的数组，但数组中的元素<mark>可能会出现重复</mark>**。

#### 3.6.8 `spop`

- 格式：`SPOP key [count]`
- 功能：**移除并返回集合中的 count 个随机元素**。count **必须为正数**，且认值为 1。
- 说明：如果 count 大于等于集合长度，那么移除并返回整个集合。

#### 3.6.9 `sdiff` / `sdiffstore` 差集

- 格式：`SDIFF key [key ...] 或 SDIFFSTORE destination key [key ...]`
- 功能：返回第一个集合与其它集合之间的**差集**。差集，difference。 (差集指第一个集合减去其他集合)
- 不同：**sdiff 临时用，sdiffstore 可以存起来**
- 说明：这两个命令的不同之处在于，**sdiffstore 不仅能够显示差集，还能将差集存储到指定的集合 destination 中**。如果 destination 集合已经存在，则将其覆盖。不存在的 key 被视为空集。
-  <img src="..\MDImg\Redis7-动力节点\三\3.6.11-1eg.png" alt="img" style="zoom:100%;" />

#### 3.6.10 `sinter` / `sinterstore` 交集

- 格式：`SINTER key [key ...] 或 SINTERSTORE destination key [key ...]`
- 不同：**sinter 临时用，sinterstore 存起来**
- 功能：返回多个集合间的交集。**交集**，intersection。 
- 说明：这两个命令的不同之处在于，**sinterstore 不仅能够显示交集，还能将交集存储到指定的集合 destination 中**。如果 destination 集合已经存在，则将其覆盖。不存在的 key 被视为空集。

#### 3.6.11 `sunion` / `sunionstore` 并集

- 格式：SUNION key [key ...] 或 SUNIONSTORE destination key [key ...]
- 不同：**sunion 临时用，sunionstore 存起来**
- 功能：返回多个集合间的并集。**并集**，union。 
- 说明：这两个命令的不同之处在于，**sunionstore 不仅能够显示并集，还能将并集存储到指定的集合 destination 中**。如果 destination 集合已经存在，则将其覆盖。不存在的 key 被视为空集。

#### 3.6.12 应用场景

Value 为 Set 类型的应用场景很多，这里对这些场景仅进行总结。

##### （1） 动态黑白名单<img src="..\MDImg\Redis7-动力节点\三\3.6.12-1eg.png" alt="img" style="zoom:100%;" />

例如某服务器中要设置用于访问控制的黑名单。如果直接将黑名单写入服务器的配置文件，那么存在的问题是，无法动态修改黑名单。此时可以将黑名单直接写入 Redis，只要有客户端来访问服务器，服务器在获取到客户端 IP后先从 Redis的黑名单中查看是否存在该 IP，如果存在，则拒绝访问，否则访问通过。

##### （2） 有限随机数

有限随机数是指返回的随机数是基于某一集合范围内的随机数，例如抽奖、随机选人。通过 spop 或 srandmember 可以实现从指定集合中随机选出元素。

##### （3） 用户画像

社交平台、电商平台等各种需要用户注册登录的平台，会根据用户提供的资料与用户使用习惯，为每个用户进行画像，即为每个用户定义很多可以反映该用户特征的标签，这些标签就可以使用 sadd 添加到该用户对应的集合中。这些标签具有无序、不重复特征。同时平台还可以使用 sinter/sinterstore 根据用户画像间的交集进行好友推荐、商品推荐、客户推荐等。

### 3.7  有序 Set 型 Value 命令

Redis 存储数据的 Value 可以是一个有序 Set，这个有序 Set 中的每个元素均 String 类型。有序 Set 与 Set 的不同之处是，有序 Set 中的每一个元素都有一个分值 score，Redis 会根据score 的值对集合进行由小到大的排序。其与 Set 集合要求相同，元素不能重复，但元素的score 可以重复。由于该类型的所有命令均是字母 z 开头，所以该 Set 也称为 ZSet。

#### 3.7.1 `zadd`

- 格式：`ZADD key score member [[score member] [score member] ...]`
- <img src="..\MDImg\Redis7-动力节点\三\3.7.1-1eg.png" alt="img" style="zoom:100%;" />
- 功能：将一个或多个 member 元素及其 score 值加入到有序集 key 中的适当位置。
- 说明：score 值可以是整数值或双精度浮点数。如果 key 不存在，则创建一个空的有序集并执行 ZADD 操作。当 key 存在但不是有序集类型时，返回一个错误。如果命令执行成功，则返回被成功添加的新成员的数量，不包括那些被更新的、已经存在的成员。若写入的 member 值已经存在，但 score 值不同，则新的 score 值将覆盖老 score。

#### 3.7.2` zrange`与 `zrevrange`按递增/递减的顺序返回指定区间的成员

- 格式：`ZRANGE key start stop [WITHSCORES] 或 ZREVRANGE key start stop [WITHSCORES]`
- 功能：返回有序集 key 中，指定区间内的成员。**zrange 命令会按 score 值<mark>递增</mark>排序**，**zrevrange命令会按score<mark>递减</mark>排序**。具有相同 score 值的成员按字典序/逆字典序排列。可以通过**使用 WITHSCORES 选项，来让成员和它的 score 值一并返回**。
- 说明：下标参数从 0 开始，即 0 表示有序集第一个成员，以 1 表示有序集第二个成员，以此类推。也可以使用负数下标，**-1 表示最后一个成员**，-2 表示倒数第二个成员，以此类推。超出范围的下标并不会引起错误。例如，当 start 的值比有序集的最大下标还要大，或是 start > stop 时，ZRANGE 命令只是简单地返回一个空列表。再比如 stop 参数的值比有序集的最大下标还要大，那么 Redis 将 stop 当作最大下标来处理。若 key 中指定范围内包含大量元素，则该命令可能会阻塞 Redis 服务。所以生产环境中如果要查询有序集合中的所有元素，一般不使用该命令，而使用 zscan 命令代替。

#### 3.7.3 `zrangebyscore` 与 `zrevrangebyscore`

- 格式：`ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`
  `ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]`
- eg：`zrangebyscore cities -inf +inf withscores`：从正无穷到负无穷的都显示出来
  <img src="..\MDImg\Redis7-动力节点\三\3.7.3-1eg.png" alt="img" style="zoom:100%;" />
- 功能：返回有序集 key 中，**所有 score 值介于 min 和 max 之间(包括等于 min 或max )的成员**。有序集成员按 score 值递增/递减次序排列。具有相同 score 值的成员按字典序/逆字典序排列。
  可选的 `LIMIT`参数指定返回结果的数量及区间(就像 SQL 中的SELECT LIMIT offset, count )，注意当 offset 很大时，定位 offset 的操作可能需要遍历整个有序集，此过程效率可能会较低。**可选的 WITHSCORES 参数**决定结果集是单单返回有序集的成员，还是**将有序集成员及其 score 值一起返回。**
- 说明：min 和 max 的取值是正负无穷大的。**默认情况下**，**区间的取值使用闭区间** (小于等于或大于等于)，也**可以通过给参数前增加左括号“(”**来使用可选的**开区间** (小于或大于)。

#### 3.7.4 `zcard`返回集合长度

- 格式：`ZCARD key`
- 功能：返回**集合的长度**
- 说明：当 key 不存在时，返回 0 。

#### 3.7.5 zcount

- 格式：`ZCOUNT key min max`
- 功能：**返回**有序集 key 中，score 值在 min 和 max **之间**(默认包括 score 值等于 min 或 max )的成员的**数量**。

#### 3.7.6 zscore

- 格式：`ZSCORE key member`
- 功能：返回有序集 key 中，成员 member 的 score 值。
- 说明：如果 member 元素不是有序集 key 的成员，或 key 不存在，返回 nil 。

#### 3.7.7 `zincrby`增加 score 值

- 格式：`ZINCRBY key increment member`
- 功能：为有序集 key 的成员 member 的 **score 值加上增量 increment **。increment 值可以是整数值或双精度浮点数。
- 说明：可以通过传递一个负数值 increment ，让 score 减去相应的值。当 key 不存在，或 member 不是 key 的成员时， ZINCRBY key increment member 等同于 ZADD key increment member 。当 key 不是有序集类型时，返回一个错误。命令执行成功，则返回 member 成员的新 score 值。

#### 3.7.8 `zrank` 与 `zrevrank`返回排名

- 格式：`ZRANK key member 或 ZREVRANK key member`
- 功能：**返回**有序集 key 中成员 member 的**排名**。**zrank 命令会按 score 值递增排序 **，**zrevrank 命令会按 score 递减排序。**
- 说明：score 值最小的成员排名为 0 。如果 member 不是有序集 key 的成员，返回 nil 。

#### 3.7.9 `zrem`移除一个或多个

- 格式：`ZREM key member [member ...]`
- 功能：**移除有序集 key 中的一个或多个成员**，不存在的成员将被忽略。
- 说明：当 key 存在但不是有序集类型时，返回一个错误。执行成功，则返回被成功移除的成员的数量，不包括被忽略的成员。

#### 3.7.10 `zremrangebyrank`移除指定排名区间

- 格式：`ZREMRANGEBYRANK key start stop`
- 功能：**移除**有序集 key 中，**指定排名(rank)区间**内的所有成员。
- 说明：排名区间分别以下标参数 start 和 stop 指出，包含 start 和 stop 在内。**排名区间参数从 0 开始，即 0 表示排名第一的成员， 1 表示排名第二的成员**，以此类推。也可以使用负数表示，-1 表示最后一个成员，-2 表示倒数第二个成员，以此类推。命令执行成功，则返回被移除成员的数量。

#### 3.7.11 `zremrangebyscore`移除指定score区间

- 格式：`ZREMRANGEBYSCORE key min max`
- 功能：**移除**有序集 key 中，所有 **score 值介于 min 和 max 之间**(包括等于 min 或max )的成员。
- 说明：命令执行成功，则返回被移除成员的数量。

#### 3.7.12 `zrangebylex`按字母(字典)序返回成员

- 格式：`ZRANGEBYLEX key min max [LIMIT offset count]`
- 注：**字母序必须用"[" 或 "(" **
- 功能：该命令**仅适用于集合中所有成员都具有相同分值的情况**。当有序集合的所有成员都具有相同的分值时，有序集合的元素会根据成员的字典序（lexicographical ordering）来进行排序。即这个命令返回给定集合中元素值介于 min 和 max 之间的成员。如果有序集合里面的成员带有不同的分值， 那么命令的执行结果与 zrange key 效果相同。 
- 说明：合法的 min 和 max 参数必须包含左小括号“(”或左中括号“[”，其中左小括号“(”表示开区间， 而左中括号“[”则表示闭区间。**min 或 max 也可使用特殊字符“+”和“-”**，分别表示正无穷大与负无穷大。

#### 3.7.13 `zlexcount`按字母序返回数量

- 格式：`ZLEXCOUNT key min max`
- 功能：该命令**仅适用于集合中所有成员都具有相同分值的情况**。该命令返回该集合中元素值本身（而非 score 值）介于 min 和 max 范围内的元素数量。

#### 3.7.14 `zremrangebylex`移除字母范围的成员

- 格式：`ZREMRANGEBYLEX key min max`
- 功能：该命令**仅适用于集合中所有成员都具有相同分值的情况**。该命令会**移除**该集合中**元素值本身介于** min 和 max 范围内的所有**元素**

#### 3.7.15 应用场景

有序 Set 最为典型的应用场景就是**排行榜**，例如音乐、视频平台中根据播放量进行排序的排行榜；电商平台根据用户评价或销售量进行排序的排行榜等。将播放量作为 score，将作品 id 作为 member，将用户评价积分或销售量作为 score，将商家 id 作为 member。使用zincrby 增加排序 score，使用 zrevrange 获取 Top 前几名，使用 zrevrank 查询当前排名，使用zscore 查询当前排序 score 等。

### 3.8 benchmark 测试工具

#### 3.8.1 简介

在Redis安装完毕后会自动安装一个redis-benchmark测试工具，其是一个压力测试工具，用于测试 Redis 的性能。
<img src="..\MDImg\Redis7-动力节点\三\3.8.1-1eg.png" alt="img" style="zoom:100%;" />
通过 redis-benchmark –help 命令可以查看到其用法：
<img src="..\MDImg\Redis7-动力节点\三\3.8.1-2eg.png" alt="img" style="zoom:100%;" />
其选项 options 非常多，下面通过例子来学习常用的 options 的用法。

#### 3.8.2 测试 1

##### （1） 命令解析

<img src="..\MDImg\Redis7-动力节点\三\3.8.2-1eg.png" alt="img" style="zoom:100%;" />
以上命令中选项的意义：

- -h：指定要测试的 Redis 的 IP，若为本机，则可省略
- -p：指定要测试的 Redis 的 port，若为 6379，则可省略
-  -c：指定模拟有客户端的数量，默认值为 50
- -n：指定这些客户端发出的请求的总量，默认值为 100000
- -d：指定测试 get/set 命令时其操作的 value 的数据长度，单位字节，默认值为 3。在测试其它命令时该指定没有用处。

以上命令的意义是，使用 100 个客户端连接该 Redis，这些客户端总共会发起 100000个请求，set/get 的 value 为 8 字节数据。

##### （2） 测试结果分析

该命令会逐个测试所有 Redis 命令，每个命令都会给出一份测试报告，每个测试报告由四部分构成：

A、测试环境报告

首先就是测试环境：
<img src="..\MDImg\Redis7-动力节点\三\3.8.2-2eg.png" alt="img" style="zoom:100%;" />

B、 延迟百分比分布

这是按照百分比进行的统计报告：每完成一次剩余测试量的 50%就给出一个统计数据。
<img src="..\MDImg\Redis7-动力节点\三\3.8.2-3eg.png" alt="img" style="zoom:100%;" />

C、 延迟的累积分布

这是按照时间间隔统计的报告：基本是每 0.1 毫秒统计一次。
<img src="..\MDImg\Redis7-动力节点\三\3.8.2-4eg.png" alt="img" style="zoom:100%;" />

D、总述报告

这是总述性报告。
<img src="..\MDImg\Redis7-动力节点\三\3.8.2-5eg.png" alt="img" style="zoom:100%;" />

#### 3.8.3 测试 2

<img src="..\MDImg\Redis7-动力节点\三\3.8.3-1eg.png" alt="img" style="zoom:100%;" />
以上命令中选项的意义：

- -t：指定要测试的命令，多个命令使用逗号分隔，不能有空格
- -q：指定**仅给出总述性报告**

### 3.9 简单动态字符串 SDS

#### 3.9.1 SDS 简介

- 无论是 Redis 的 Key 还是 Value，其基础数据类型都是字符串。例如，Hash 型 Value 的field 与 value 的类型、List 型、Set 型、ZSet 型 Value 的元素的类型等都是字符串。虽然 Redis是使用标准 C 语言开发的，但并没有直接使用 C 语言中传统的字符串表示，而是自定义了一种字符串。这种字符串本身的结构比较简单，但功能却非常强大，称为简单动态字符串，Simple Dynamic String，简称 SDS。

- 注意，Redis 中的所有字符串并不都是 SDS，**也会出现 C 字符串。C 字符串只会出现在字符串“字面常量”中，并且该字符串不可能发生变更。**

- eg：`redisLog(REDIS_WARNNING, “sdfsfsafsafds”);`如果对redis作二次开发，这个sdfsfsafsafds就属于字面常量C字符串

- > 存的是SDS查的是C字符串

   <img src="..\MDImg\Redis7-动力节点\三\3.9.1-1eg.png" alt="img" style="zoom:100%;" />

#### 3.9.2 SDS 结构

SDS 不同于 C 字符串。C 字符串本身是一个以双引号括起来，**以空字符`’\0’`结尾的字符序列**。但 SDS 是一个结构体，定义在 Redis 安装目录下的 src/sds.h 中：
```java
struct sdshdr {
    // 字节数组，用于保存字符串
	char buf[];
	// buf[]中已使用字节数量，称为 SDS 的长度
	int len;
	// buf[]中尚未使用的字节数量
	int free;
}
```

例如执行 `SET country “China”`命令时，键 country 与值”China”都是 SDS 类型的，只不过一个是 SDS 的变量，一个是 SDS 的字面常量。”China”在内存中的结构如下：
<img src="..\MDImg\Redis7-动力节点\三\3.9.2-1eg.png" alt="img" style="zoom:100%;" />通过以上结构可以看出，SDS 的 buf 值实际是一个 C 字符串，包含空字符’\0’共占 6 个字节。但 SDS 的 len 是不包含空字符’\0’的。
<img src="..\MDImg\Redis7-动力节点\三\3.9.2-2eg.png" alt="img" style="zoom:100%;" />
该结构与前面不同的是，这里有 3 字节未使用空间。

