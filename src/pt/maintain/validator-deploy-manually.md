## 手动部署验证节点

手动部署应用链的验证节点，可以选择自己喜欢的 VPS 供应商，一般使用 **Ubuntu**操作系统。 需完成以下步骤：

1. 安装 Rust
2. 安装依赖项
3. 获取应用链的节点二进制文件
4. 启动应用链的节点

### 1.安装 Rust

如果服务器的系统没有安装 Rust，则首先要进行安装。

下面这个命令可以下载安装最新版本的 Rust。

```bash
# Install
curl https://sh.rustup.rs -sSf | sh
# Configure
source ~/.cargo/env
```

如果已经安装了 Rust，则配置 Rust 工具链默认为最新的稳定版本，添加 nightly 和 nightly wasm 编译目标。

```bash
rustup default stable
rustup update
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

### 2. 安装依赖项

首先安装一些必要的依赖项，以便编译和运行应用链的节点，运行下面的命令。

```bash
sudo apt update
# May prompt for location information
sudo apt install make clang pkg-config libssl-dev build-essential
```

### 3. 获取应用链的节点二进制文件

可以从应用链的源代码仓库中，编译源码生成应用链的节点二进制文件。

```bash
git clone <Appchain GitHub Repo>
cd <Appchain>
cargo build --release
```

> `<Appchain GitHub Repo>`，在应用链页面的`Github`图标获取
> `<Appchain>`，应用链仓库的名称

编译完成后，在目录 `./target/release/` 下会生成应用链的节点二进制文件。这一步骤将需要一段时间，大约为10~40分钟，取决于服务器的硬件配置。

> 备注：如果遇到编译错误，可能需要将 Rust 切换到一个较新的 nightly 版本。

### 4. 启动应用链的节点

启动应用链节点的参考命令如下：

```bash
./target/release/<Appchain node> \
--base-path ./chain_data \
--chain octopus-mainnet \
--name <Your validator name> \
--validator \
--telemetry-url "wss://telemetry.mainnet.octopus.network/submit 0" \  
--enable-offchain-indexing true
```

> `<Appchain node>`，上一步编译生成的节点二进制文件
> `<Your validator name>`，验证节点的名称

检查节点是否已完成链数据的同步，查看节点的日志，是否有类似的输出如下：

```bash
2021-09-21 00:12:09 ✨ Imported #54411 (0x3566…3b0e)
2021-09-21 00:12:12 ✨ Imported #54412 (0xdf36…2c87)
2021-09-21 00:12:12 [54412] 🐙 Current block: 54412 (parent hash: 0x9cc7f31a20793f50cf885835de0e3977a1e080431ebc002469aa176046ba094a)
......
2021-09-21 00:13:18 ✨ Imported #54434 (0xba36…ee68)
2021-09-21 00:13:18 [54434] 🐙 Current block: 54434 (parent hash: 0x84aa3d1b6455859f9503d6ecc70b50b183141fe08f5b0695357e00fe1d24d915)
2021-09-21 00:13:18 💤 Idle (6 peers), best: #54434 (0xba36…ee68), finalized #54431 (0xd194…b319), ⬇ 22.0kiB/s ⬆ 21.9kiB/s
```

等待节点同步完成后，此时需要[生成验证节点 Session Key](validator-set-session-keys.md)，以供下一步[注册验证人](./validator-register.md)使用。

### 升级验证节点

应用链客户端节点发布新的版本后，验证人需要升级其验证节点。请定期关注章鱼网络 Discord 的**validators-delegators**频道，章鱼网络团队会发布应用链节点新版本的信息，验证人需要：

1. 编译新版本的应用链源码，生成新的应用链客户端；
2. 停掉升级前的验证节点；
3. 用新的应用链客户端启动验证节点。