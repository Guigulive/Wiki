## 第七课&白帽黑客比赛问题整理

### 关于回退函数(fallback function)的机制理解得不太透彻，有没有详细的说明资料？ 
* 【参考】http://www.tryblockchain.org/14825685263030.html 

### 下面这个函数的最后为何还要加上一个()？ 
```
msg.sender.call.value(amount)();
```
* value(amount), gas(xxx), gasprice(xxx)等是指定交易的参数。他们可以加到任何一个方法的自身函数参数之前。后边的括号才是方法自身的参数。 
* 另外可以参考第四课的FAQ：“truffle console中如何像remix界面里那样通过addFund来加钱，并且指定支付帐户？”　

### web3调用addEmployee函数并返回一个promise，这个promise的resolve（.then调用了）代表什么含义呢？比如：transaction广播出去了，还是transaction至少被一个full node接受了，还是transaction至少被一个full node执行了，还是transaction已经写到区块链上了，得到了n个确认，还是说有其他解释？ 
* 指的是transaction is mined。transaction已经被写到区块链了，有没有得到确认还不一定。
* 注意：由于关于mined的定义还不是十分清晰，该QA的回答的准确性未被充分确认，仅供参考。 

### 利用infuro部署时出现exceeds block gas limit的error，需要进行什么样的修改？ 
* 可以在truffle-config.js中把gas设大点，比如： 
```
ropsten: {
    provider: function() {
		return new HDWalletProvider(mnemonic, "https://ropsten.infura.io/gYQcfCK5nmcxAwEoRvGV", 5)
    },
    gas: 2706583,
    network_id: 3
}, 
```

### 关于白帽黑客比赛的注意点有哪些？ 
1. 黑客大赛是通过破解老董部署在以太坊上Ropsten测试网络的一个智能合约，让大家认识到以及充分实践重要的智能合约安全知识，同时也是课程的一个结业考试。 
2. 参加比赛的地址，需要自己能够控制这个地址的私钥。imtoken、metamask、mytherwallet生成的地址都可以。但是绝对不要用交易所的地址去参加比赛！！！这个不是用来领最后奖金的地址，而是比赛用的地址。 
3. 为了避免不必要的麻烦，建议用metamask新生成一个地址。 
4. 如果不幸还是有组员用交易所的地址参加了比赛……那么，别灰心！塞翁失马，焉知非福！（不是凭空说这句话的，而是根据二期学员的实际经验总结出的这句话）同学，加油！ 
5. 比赛需要每一位参与者的配合，但是没有必要每一位参与者都一直在线。因此需要组长合理分工。 
6. 在比赛最后需要组长总结出本组的闪光点，比如在什么时候发现了什么漏洞，什么时候利用了什么漏洞，并且需要提交证据（微信截图，或者是testnet transaction记录），以帮助老董裁决最终名次。因此请做好比赛过程记录。 
7. 原则上，老董除了部署合约和重置合约之外不会做任何额外操作。 
8. 最后，希望同学能够积极报名参加比赛，并在享受比赛的过程的同时努力争取比赛的结果。没拿到名次也别灰心，因为这个比赛的过程带来的收获可能远远大于前面六节课所有的收获。 

### 上完“以太坊智能合约全栈课程”之后可以有什么进一步的学习内容的推荐或者项目的参与机会？ 
* 老董会为了自己将来的项目招兵买马的，详情请耐心等待。 
* 鼓励大家报名申请课程助教，帮助第四期、第五期……等今后的学员进行学习，努力充当一位以太坊社区的知识布道者！同时，教是最好的学，教别的人同时自己学习到的知识也能够进一步地巩固。 

---
上述问题整理自：
* 以太坊智能合约全栈课程第二期第七课课程内容 
* 以太坊智能合约全栈课程第二期白帽黑客比赛经验 
* 以太坊智能合约全栈课程第二期硅谷Live github 每日优质内容复盘(1.29~2.3) 
https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98

