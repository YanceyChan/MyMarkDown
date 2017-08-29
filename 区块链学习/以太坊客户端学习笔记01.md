# 以太坊客户端学习笔记01

这篇主要记录下以太坊Go-Ethereum 客户端的安装以及基本的使用。
以下如果描述的不正确的地方，请指出~谢谢。

以太坊，简单来说就是区块链+智能合约。比特币是区块链1.0，以太坊就是区块链2.0。

不了解的同学可以上网查查资料，这里不再展开（因为我也不是很懂~~~~(>_<)~~~~）。

## 如何安装
Geth的GitHub地址[https://github.com/ethereum/go-ethereum](https://github.com/ethereum/go-ethereum)

我们以Mac OS X 系统为例，其他的安装方法，可以查看geth的wiki。

```
brew tap ethereum/ethereum
brew install ethereum
```

安装成功后，输入
```
geth -h
```
这时候应该会列出geth的帮助文档。

现在，我们可以开始尝试使用Geth了。Geth 使用的是go语言，[查看Geth的APIs](https://github.com/ethereum/go-ethereum/wiki/Management-APIs)

### 选择不同的环境
* 生产环境  直接使用 ```geth ``` 命令就会连接到以太坊的主链上。
* 测试环境TestNet  也是以太坊提供的环境，节点也是全球化的。

 ```
 --testnet		Ropsten network: pre-configured proof-of-work test network
 --rinkeby		Rinkeby network: pre-configured proof-of-authority test network
 ```
	
* 开发模式  开发模式下，只有一个节点，不会同步公有链上的区块（我同步下来了差不多60G..太大了，而且慢）（注意：测试和开发环境参数不用同时使用）
	
 ```
 --dev		Developer mode: pre-configured private network with several debugging flags
 ```
	
* 私有网络PrivateNetWork 就是由用户自己通过Geth创建的私有网络，是一个非常适合开发、调试和测试的网络

#### 这次我们先用开发模式来学习geth。搭建如何私有链下次再写。
	
```
//--dev : 选择开发模式
//2>>geth.log : 是让控制台输出到geth.log 文件中去。会在当前文件夹下面创建，而不是在geth的文件下创建的
geth --dev console 2>>geth.log
	
//打开新的终端，查看控制台输出。注意：要在之前的文件夹下
tail -f geth.log
	
//或者直接 这样控制台会直接在同一个地方输出
geth --dev console  
```

正常启动之后，我们应该看见成功进入控制台，如下：

```
$ geth --dev console
WARN [08-27|20:56:45] No etherbase set and no accounts found as default
INFO [08-27|20:56:45] Starting peer-to-peer node               instance=Geth/v1.6.7-stable-ab5646c5/darwin-amd64/go1.8.3
INFO [08-27|20:56:45] Allocated cache and file handles         database=/var/folders/hk/zwvdvp_s4kz_6zklbf_4whmw0000gn/T/ethereum_dev_mode/geth/chaindata cache=128 handles=1024
INFO [08-27|20:56:45] Initialised chain configuration          config="{ChainID: 1337 Homestead: 0 DAO: <nil> DAOSupport: false EIP150: 0 EIP155: 0 EIP158: 0 Metropolis: 9223372036854775807 Engine: ethash}"
WARN [08-27|20:56:45] Ethash used in test mode
INFO [08-27|20:56:45] Initialising Ethereum protocol           versions="[63 62]" network=1
INFO [08-27|20:56:45] Loaded most recent local header          number=0 hash=e5be92…38f3bc td=131072
INFO [08-27|20:56:45] Loaded most recent local full block      number=0 hash=e5be92…38f3bc td=131072
INFO [08-27|20:56:45] Loaded most recent local fast block      number=0 hash=e5be92…38f3bc td=131072
INFO [08-27|20:56:45] Starting P2P networking
INFO [08-27|20:56:45] started whisper v.5.0
INFO [08-27|20:56:45] RLPx listener up                         self="enode://5d2862923598cd341db0d7340ee1114d86027d3b260bc4bd76bb221a369a608fc53272c4c14d154ec2df6c4a0483283e2633dfab13ecf3bdedad98a6f0c55847@[::]:49309?discport=0"
INFO [08-27|20:56:45] IPC endpoint opened: /var/folders/hk/zwvdvp_s4kz_6zklbf_4whmw0000gn/T/ethereum_dev_mode/geth.ipc
INFO [08-27|20:56:45] Mapped network port                      proto=tcp extport=49309 intport=49309 interface=NAT-PMP(192.168.0.1)
Welcome to the Geth JavaScript console!

instance: Geth/v1.6.7-stable-ab5646c5/darwin-amd64/go1.8.3
 modules: admin:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 shh:1.0 txpool:1.0 web3:1.0

>
```

其中，database（/var/folders/hk/zwvdvp_s4kz_6zklbf_4whmw0000gn/T/ethereum_dev_mode/geth/chaindata），是开发模式下数据存放的地方。很多其他教程在用开发模式下要指定目录，以避免和生产数据产生混淆，但是2个的文件夹不是在同一个地方（生产geth文件夹的是在/Users/用户名/Library/Ethereum/geth)，不知道这样指定的意义？而且我来回切换过2个模式，也没发现生产模式下会有什么异常，所有我这里没有在生产模式下指定文件夹地址。

下面给出生产环境下的，和上面开发模式的对比下

```
$ geth
INFO [08-27|21:00:38] Starting peer-to-peer node               instance=Geth/v1.6.7-stable-ab5646c5/darwin-amd64/go1.8.3
INFO [08-27|21:00:38] Allocated cache and file handles         database=/Users/用户名/Library/Ethereum/geth/chaindata cache=128 handles=1024
INFO [08-27|21:00:38] Initialised chain configuration          config="{ChainID: 1 Homestead: 1150000 DAO: 1920000 DAOSupport: true EIP150: 2463000 EIP155: 2675000 EIP158: 2675000 Metropolis: 9223372036854775807 Engine: ethash}"
INFO [08-27|21:00:38] Disk storage enabled for ethash caches   dir=/Users/用户名/Library/Ethereum/geth/ethash count=3
INFO [08-27|21:00:38] Disk storage enabled for ethash DAGs     dir=/Users/用户名/.ethash                      count=2
INFO [08-27|21:00:38] Initialising Ethereum protocol           versions="[63 62]" network=1
INFO [08-27|21:00:38] Loaded most recent local header          number=4208484 hash=000c95…be8739 td=783241965759849246391
INFO [08-27|21:00:38] Loaded most recent local full block      number=4208484 hash=000c95…be8739 td=783241965759849246391
INFO [08-27|21:00:38] Loaded most recent local fast block      number=4208484 hash=000c95…be8739 td=783241965759849246391
WARN [08-27|21:00:39] Blockchain not empty, fast sync disabled
INFO [08-27|21:00:39] Starting P2P networking
INFO [08-27|21:00:39] Mapped network port                      proto=udp extport=30303 intport=30303 interface=NAT-PMP(192.168.0.1)
INFO [08-27|21:00:39] UDP listener up                          self=enode://5f9fae1a0f4f066dbfbc5d85f62f06923a9ed0a389c6cce9a79901e9dae953cd19c3d6a049dbcc54e62759961d4bee000ee3aaccc207bf706ae955bf9d2388e5@192.168.1.2:30303
INFO [08-27|21:00:39] RLPx listener up                         self=enode://5f9fae1a0f4f066dbfbc5d85f62f06923a9ed0a389c6cce9a79901e9dae953cd19c3d6a049dbcc54e62759961d4bee000ee3aaccc207bf706ae955bf9d2388e5@192.168.1.2:30303
INFO [08-27|21:00:39] IPC endpoint opened: /Users/用户名/Library/Ethereum/geth.ipc
INFO [08-27|21:00:39] Mapped network port                      proto=tcp extport=30303 intport=30303 interface=NAT-PMP(192.168.0.1)
```
	
	
### 创建新的账户
进入控制台后，我们可以查看我们当前环境下的账户信息

```
> eth.accounts
[]
```
返回的是一个空数组，表示当前环境下还没有创建账户。
接着，我们新建2个账户

```
> personal.newAccount('12345678')
INFO [08-27|21:07:38] New wallet appeared                      url=keystore:///var/folders/hk/zwvd… status=Locked
"0xf38c69c8e9487139746e6b7df2f6bf1162a58b60"
> personal.newAccount('12345678')
INFO [08-27|21:07:53] New wallet appeared                      url=keystore:///var/folders/hk/zwvd… status=Locked
"0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71"
> eth.accounts
["0xf38c69c8e9487139746e6b7df2f6bf1162a58b60", "0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71"]
```

12345678 这个是我们创建账户时对应的密码~请记住哦，下面还要用。当然你可以自己填写自己的密码。
"0xf38c69c8e9487139746e6b7df2f6bf1162a58b60", "0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71"这2串数字，就是我们创建的账户地址了，以后的转账、查询账户资料，都会用到这个地址。别人给你转钱，你只要告诉对方这个地址，别人就能给你转钱了。

	

### 查看账户

```
> eth.getBalance("0xf38c69c8e9487139746e6b7df2f6bf1162a58b60")
0
> eth.getBalance("0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71")
0
```

使用eth.getBalance(）方法，传入我们要查的账户地址，就能获取当前的账户有多少以太币了。当然现在我们的2个账户还是0。

### 挖矿
怎么才能获取以太币？最简单的就是挖矿了。当然，现实生活下。。还是去交易所买吧。一般的机器，估计也挖不了。

```
> miner.start()
INFO [08-27|21:15:10] Updated mining threads                   threads=0
INFO [08-27|21:15:10] Transaction pool price threshold updated price=0
INFO [08-27|21:15:10] Starting mining operation
null
> INFO [08-27|21:15:10] Commit new mining work                   number=1 txs=0 uncles=0 elapsed=162.015µs
INFO [08-27|21:15:10] Generating DAG in progress               epoch=0 percentage=0 elapsed=45.721µs
...
INFO [08-27|21:15:10] Generating DAG in progress               epoch=1 percentage=13 elapsed=1.536ms
elapsed=10.499ms
INFO [08-27|21:15:10] Successfully sealed new block            number=1 hash=30195b…67dd8f
INFO [08-27|21:15:10] 🔨 mined potential block                  number=1 hash=30195b…67dd8f
INFO [08-27|21:15:10] Commit new mining work                   number=2 txs=0 uncles=0 elapsed=199.485µs
INFO [08-27|21:15:10] Successfully sealed new block            number=2 hash=742668…9dad4e
INFO [08-27|21:15:10] 🔨 mined potential block                  number=2 hash=742668…9dad4e
INFO [08-27|21:15:10] Mining too far in the future             wait=2s
INFO [08-27|21:15:12] Commit new mining work                   number=3 txs=0 uncles=0 elapsed=2.005s
INFO [08-27|21:15:13] Successfully sealed new block            number=3 hash=66f660…123c80
INFO [08-27|21:15:13] 🔨 mined potential block                  number=3 hash=66f660…123c80
INFO [08-27|21:15:13] Commit new mining work                   number=4 txs=0 uncles=0 elapsed=154.482µs
INFO [08-27|21:15:13] Successfully sealed new block            number=4 hash=d2f582…561915
INFO [08-27|21:15:13] 🔨 mined potential block                  number=4 hash=d2f582…561915
INFO [08-27|21:15:13] Commit new mining work                   number=5 txs=0 uncles=0 elapsed=222.227µs
INFO [08-27|21:15:13] Successfully sealed new block            number=5 hash=a0b92a…5b8c2f
INFO [08-27|21:15:13] 🔨 mined potential block                  number=5 hash=a0b92a…5b8c2f
INFO [08-27|21:15:13] Mining too far in the future             wait=2s
> mINFO [08-27|21:15:15] Commit new mining work                   number=6 txs=0 uncles=0 elapsed=2.000s
> minINFO [08-27|21:15:15] Successfully sealed new block            number=6 hash=51ab7a…d8b9c7
INFO [08-27|21:15:15] 🔗 block reached canonical chain          number=1 hash=30195b…67dd8f
INFO [08-27|21:15:15] 🔨 mined potential block                  number=6 hash=51ab7a…d8b9c7
INFO [08-27|21:15:15] Commit new mining work                   number=7 txs=0 uncles=0 elapsed=162.129µs
INFO [08-27|21:15:15] Successfully sealed new block            number=7 hash=8cb4b6…ddcd7d
INFO [08-27|21:15:15] 🔗 block reached canonical chain          number=2 hash=742668…9dad4e
INFO [08-27|21:15:15] 🔨 mined potential block                  number=7 hash=8cb4b6…ddcd7d
INFO [08-27|21:15:15] Mining too far in the future             wait=2s
> miner.stopINFO [08-27|21:15:17] Commit new mining work                   number=8 txs=0 uncles=0 elapsed=2.001s
> miner.stop()
true
```

miner.start() 开始挖矿

miner.stop() 停止挖矿

如果在生产环境下，要挖矿是需要先同步主链上数据的。
这时候，我们看看我们之前的创建的2个账户。

```
> user1 = eth.accounts[0]
"0xf38c69c8e9487139746e6b7df2f6bf1162a58b60"
> user2 = eth.accounts[1]
"0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71"
> eth.getBalance(user1)
35000000000000000000
> eth.getBalance(user2)
0
```

挖矿是默认将我们第一次创建的账号地址作为挖矿收益存放的地址的。所以，第二个账号没有获得以太币


### 转账

如果我们先要第二个账户也有以太币，那可以用第一个账户转账给第二个账户。
假如我想从第一个账户中转1以太币给第二个账户

```
> amount = web3.toWei(1.0)
"1000000000000000000"
> eth.sendTransaction({from:user1, to:user2, value:amount})
Error: authentication needed: password or unlock
    at web3.js:3104:20
    at web3.js:6191:15
    at web3.js:5004:36
    at <anonymous>:1:1

```
为什么出现error？因为这是以太坊的一个保护机制，账户每隔一段时间就会自动锁定，这个时候任何以太币在账户之间的转换都会被拒绝，除非把账户解锁

```
> personal.unlockAccount(user1)
Unlock account 0xf38c69c8e9487139746e6b7df2f6bf1162a58b60
Passphrase:
true
```
Passphrase: 为当时我们创建账户时输入的密码。继续尝试交易。

```
> eth.sendTransaction({from:user1, to:user2, value:amount})
INFO [08-27|21:30:22] Submitted transaction                    fullhash=0xbd701ebf132c8d52dc1163c0112eefc36959e6b1e5fde83c1195caa0096983ca recipient=0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71
"0xbd701ebf132c8d52dc1163c0112eefc36959e6b1e5fde83c1195caa0096983ca"
```
看过区块链基本概念，应该知道，交易的成功是需要挖矿的，所以，我们又要开始挖矿

```
> miner.start()
INFO [08-27|21:31:00] Updated mining threads                   threads=0
INFO [08-27|21:31:00] Transaction pool price threshold updated price=0
INFO [08-27|21:31:00] Starting mining operation
null
> INFO [08-27|21:31:00] Commit new mining work                   number=8 txs=1 uncles=0 elapsed=336.998µs
INFO [08-27|21:31:00] Successfully sealed new block            number=8 hash=0c8f73…0bffce
INFO [08-27|21:31:00] 🔗 block reached canonical chain          number=3 hash=66f660…123c80
INFO [08-27|21:31:00] 🔨 mined potential block                  number=8 hash=0c8f73…0bffce
INFO [08-27|21:31:00] Commit new mining work                   number=9 txs=0 uncles=0 elapsed=197.989µs
INFO [08-27|21:31:00] Successfully sealed new block            number=9 hash=8435a8…3f52e3
INFO [08-27|21:31:00] 🔗 block reached canonical chain          number=4 hash=d2f582…561915
INFO [08-27|21:31:00] 🔨 mined potential block                  number=9 hash=8435a8…3f52e3
INFO [08-27|21:31:00] Mining too far in the future             wait=2s
> minerINFO [08-27|21:31:02] Commit new mining work                   number=10 txs=0 uncles=0 elapsed=2.001s
INFO [08-27|21:31:02] Successfully sealed new block            number=10 hash=bccee0…d9154c
INFO [08-27|21:31:02] 🔗 block reached canonical chain          number=5  hash=a0b92a…5b8c2f
INFO [08-27|21:31:02] 🔨 mined potential block                  number=10 hash=bccee0…d9154c
INFO [08-27|21:31:02] Commit new mining work                   number=11 txs=0 uncles=0 elapsed=355.605µs
> miner.stoINFO [08-27|21:31:03] Successfully sealed new block            number=11 hash=d517bb…a0c5ff
INFO [08-27|21:31:03] 🔗 block reached canonical chain          number=6  hash=51ab7a…d8b9c7
INFO [08-27|21:31:03] 🔨 mined potential block                  number=11 hash=d517bb…a0c5ff
INFO [08-27|21:31:03] Commit new mining work                   number=12 txs=0 uncles=0 elapsed=171.33µs
> miner.stopINFO [08-27|21:31:04] Successfully sealed new block            number=12 hash=96f208…6f8ed1
INFO [08-27|21:31:04] 🔗 block reached canonical chain          number=7  hash=8cb4b6…ddcd7d
INFO [08-27|21:31:04] 🔨 mined potential block                  number=12 hash=96f208…6f8ed1
INFO [08-27|21:31:04] Commit new mining work                   number=13 txs=0 uncles=0 elapsed=228.932µs
INFO [08-27|21:31:04] Successfully sealed new block            number=13 hash=218e08…5fadbb
INFO [08-27|21:31:04] 🔗 block reached canonical chain          number=8  hash=0c8f73…0bffce
INFO [08-27|21:31:04] 🔨 mined potential block                  number=13 hash=218e08…5fadbb
INFO [08-27|21:31:04] Mining too far in the future             wait=2s
> miner.stop()
INFO [08-27|21:31:06] Commit new mining work                   number=14 txs=0 uncles=0 elapsed=2.005s
true
> eth.getBalance(user2)
1000000000000000000
```

最后，user2账户里面也有了1000000000000000000 Wei了，也就是1个以太币。
这笔交易，我们可以从上面的信息获取到是记录在第8个block里面。我们看看这个区块的内容

```
> eth.getBlock(8)
{
  difficulty: 131072,
  extraData: "0xd883010607846765746887676f312e382e338664617277696e",
  gasLimit: 4712388,
  gasUsed: 21000,
  hash: "0x0c8f73d3b772c6b08a62a55d67ddd428ba86b4a5f68e1fcf5804296c750bffce",
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  miner: "0xf38c69c8e9487139746e6b7df2f6bf1162a58b60",
  mixHash: "0x2a34f3fdffebd553a0e7080d5b3de7b34df4709638897b24b73c9a7362280bc8",
  nonce: "0x5ebec41ea909e99a",
  number: 8,
  parentHash: "0x8cb4b611a65dd9a3e29708582e910173a5eb2563d96d7dac2b44b7b732ddcd7d",
  receiptsRoot: "0x9a119cca1bcbf34249fefa8e713cb53177cc0665468e020a703a6bd465a3d77b",
  sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  size: 647,
  stateRoot: "0xa52c7c2fba6eabe5337ee3055d119da5fa2a6bb7b51fd1dfb8fc8c03f505c4d3",
  timestamp: 1503840660,
  totalDifficulty: 1180992,
  transactions: ["0xbd701ebf132c8d52dc1163c0112eefc36959e6b1e5fde83c1195caa0096983ca"],
  transactionsRoot: "0x9565de8884cf728b336340a3008e0cd9e3d0e2b6cb441155358a2fd86c1e9b77",
  uncles: []
}

> eth.getTransaction("0xbd701ebf132c8d52dc1163c0112eefc36959e6b1e5fde83c1195caa0096983ca")
{
  blockHash: "0x0c8f73d3b772c6b08a62a55d67ddd428ba86b4a5f68e1fcf5804296c750bffce",
  blockNumber: 8,
  from: "0xf38c69c8e9487139746e6b7df2f6bf1162a58b60",
  gas: 90000,
  gasPrice: 0,
  hash: "0xbd701ebf132c8d52dc1163c0112eefc36959e6b1e5fde83c1195caa0096983ca",
  input: "0x",
  nonce: 0,
  r: "0xdf292a060c795957bf261988a75b1aceb24d246919f2f73617fff1c32be5dcd0",
  s: "0x2ecc541ab90d9711520d21b7faa72dbd16b05ca0f2336fb79cab984c2558d809",
  to: "0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71",
  transactionIndex: 0,
  v: "0xa96",
  value: 1000000000000000000
}

> user1 = eth.accounts[0]
"0xf38c69c8e9487139746e6b7df2f6bf1162a58b60"
> user2 = eth.accounts[1]
"0xc1aa13886addcc31f5c42b06c363d6f2b3a52c71"
```
看出什么端倪了么~~嘻嘻。本节完~






