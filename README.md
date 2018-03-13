实践操作:

在两台不同的虚拟机上：（10.45.59.181和10.45.59.191)

1、安装pip3.6 （参考https://www.cnblogs.com/zhouxinfei/p/8353562.html ）

2、pip install Flask==0.12.2 requests==2.18.4

3、git clone https://github.com/fjibj/blockchain

4、cd blockchain

5、python blockchain.py -p 56111 (端口任意，但需要避开556,6000等专有端口，以免chrome等浏览器无法使用）

操作：

1、注册：

curl -X POST -H "Content-Type: application/json" -d '{
 "nodes": ["http://10.45.59.191:56111"]
}' "http://10.45.59.182:56111/nodes/register"


2、查看chain:

curl http://10.45.59.182:56111/chain

3、挖矿：

curl http://10.45.59.182:56111/mine

4、交易：

curl -X POST -H "Content-Type: application/json" -d '{
 "sender": "d4ee26eee15148ee92c6cd394edd974e",
 "recipient": "someone-other-address",
 "amount": 5
}' "http://10.45.59.182:56111/transactions/new"

5、共识机制：即解决冲突，寻找最长有效链

curl http://10.45.59.182:56111/nodes/resolve

注

只要算力够强，即使后加入的节点，只要算得比较快，当链条达到最长时就可以通吃，输家可能整个链都废了，但赢家其实也不稳定，输家如果提高算力后拼命挖矿使得链条长度超过其他家，也可以再次实现反超，这种竞争似乎永无止境。

交易对挖矿似乎没什么影响，除了会影响hash值，有些块可以不承载交易（除了挖矿获得1个币这个自含交易）

猜想，有待验证：

1、 比特币等真实区块链应用可能会定时确定最长链，并在确定最长链之后可能会将其他分支链消除（否则其他链仍有反超可能）

2、 如果分支链被取消，其上所承载的交易（包括挖矿奖励交易）也将被取消，这种情况下要不要作反向回滚呢？


此只为一个简单示例，尚缺乏双花劫持、用户签名等安全控制，仅可做实验之用。

-------------------------以下是原文参考--------------------------------------------\

# 用Python从零开始创建区块链

本文是博客：[用Python从零开始创建区块链](http://learnblockchain.cn/2017/10/27/build_blockchain_by_python/) 的源码. 
翻译自[Building a Blockchain](https://medium.com/p/117428612f46)

[博客地址](http://learnblockchain.cn/2017/10/27/build_blockchain_by_python/)| [英文README](https://github.com/xilibi2003/blockchain/blob/master/README-en.md) 

## 安装

1. 安装 [Python 3.6+](https://www.python.org/downloads/) is installed. 
2. 安装 [pipenv](https://github.com/kennethreitz/pipenv). 

```
$ pip install pipenv 
```

3. 创建virtual env. 

```
$ pipenv --python=python3.6
```

4. 安装依赖.  

```
$ pipenv install 
``` 

5. 运行节点:
    * `$ pipenv run python blockchain.py` 
    * `$ pipenv run python blockchain.py -p 5001`
    * `$ pipenv run python blockchain.py --port 5002`
    
## Docker运行

另一种方式是使用Docker运行：

1. 克隆库
2. 构建docker容器

```
$ docker build -t blockchain .
```

3. 运行

```
$ docker run --rm -p 80:5000 blockchain
```

4. 添加多个节点:

```
$ docker run --rm -p 81:5000 blockchain
$ docker run --rm -p 82:5000 blockchain
$ docker run --rm -p 83:5000 blockchain
```

## 贡献
[深入浅出区块链](http://learnblockchain.cn/) 想做好的区块链学习博客。
[博客地址](https://github.com/xilibi2003/learnblockchain) 欢迎大家一起参与贡献，一起推动区块链技术发展。




