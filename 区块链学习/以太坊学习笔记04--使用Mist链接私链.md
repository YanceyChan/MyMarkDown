# 使用Mist连接私链

```
//环境
$ npm -v
4.1.2
$ node -v
v7.7.3

$ npm list -g --depth 0
/Users/xxxxxx/.nvm/versions/node/v7.7.3/lib
├── electron@1.4.15
├── gulp@3.9.1
├── iron-node@3.0.19
├── npm@4.1.2
├── react-native-cli@2.0.1
├── supervisor@0.12.0
├── typescript@2.4.2
└── yarn@0.27.5
```

上面是我搭建成功之后，查的安装环境。（中间出现太多问题，花费的时间也比较久）


## 安装Mist
Mist 是以太坊官方提供的浏览器，通过Mist我们可以很方便的连接上我们的私有网络，从而更好的开发、调试、测试我们的智能合约

Mist 的GitHub地址[https://github.com/ethereum/mist](https://github.com/ethereum/mist)

其实按照GitHub的指示安装，就差不多ok了。不过在我安装的过程中，出现各种奇怪的问题，Google也没类似的解决办法，于是我在不同的node版本下多次尝试安装。最后终于成功连接上自己搭建的私链。



#### 遇到的问题1
如果meteor --no-release-check 一直断续，可以按照[https://stackoverflow.com/questions/37185894/meteor-is-stuck-at-downloading-meteor-tool1-3-2-4](https://stackoverflow.com/questions/37185894/meteor-is-stuck-at-downloading-meteor-tool1-3-2-4)里面的方法，将 ```54.192.225.217 warehouse.meteor.com``` 加进我们的 ```/etc/hosts```文件中。


#### 遇到的问题2
```
=> Started proxy.
=> A patch (Meteor 1.4.2.7) for your current release is available!
   Update this project now with 'meteor update --patch'.
=> Errors prevented startup:

   While building the application:
   /Users/yancey_chan/Documents/Git仓库/mist/interface/i18n/mist.sq.i18n.json:
   Can't load
   `/Users/yancey_chan/Documents/Git仓库/mist/interface/i18n/mist.sq.i18n.json'
   JSON
   packages/tap-i18n-compiler/lib/plugin/helpers/load_json.coffee:25:19: Can't
   load
   `/Users/yancey_chan/Documents/Git仓库/mist/interface/i18n/mist.sq.i18n.json'
   JSON (compiling i18n/mist.sq.i18n.json)
   at _.extend.loadJSON
   (packages/tap-i18n-compiler/lib/plugin/helpers/load_json.coffee:25:19)
   at
   packages/tap-i18n-compiler/lib/plugin/compilers/i18n.generic_compiler.coffee:24:18


=> Your application has errors. Waiting for file change.
=> Started MongoDB.
```

