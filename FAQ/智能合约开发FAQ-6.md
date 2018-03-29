## 第六课问题整理

### 怎么删除metamask上的account？ 
* 删account似乎没有，不过可以通过用助记词恢复钱包的方式。 

### Metamask中有些account会显示loose的状态，这个怎么恢复？ 
* Loose指的是通过私钥方式导进去的account。这种用助记词恢复后会没掉，得再自己导私钥进去恢复。 

### 为什么我addFund不用metamask能fire event，用了metamask就不行了，但fund都加成功了？  
* 这儿有个BUG，启动网页之后不要在metamask里切换网络，要先把metatask切到测试网络后，重启浏览器。 
> Finally, Metamask seems to have problems switching networks cleanly. After switching to the network you want to use, try closing all open Chrome windows (not just the window you're using Metamask in) and then open the browser again from scratch. 

【参考】 https://ethereum.stackexchange.com/questions/28747/cant-retrieve-event-logs-with-metamask-web3 

### 浏览器加载了前端页面之后总是显示loading，是啥原因？ 
* 首先打开console看看输出是什么，根据报错信息进行排查。 
* 其次，可能的问题有：
1. testrpc没打开 
2. truffle migrate需要带上--reset参数 
3. metamask有没有切换到private网络 

---
上述问题整理自：
* 以太坊智能合约全栈课程第二期第六课课程内容 
* 以太坊智能合约全栈课程第二期第六课补充学习笔记 
https://github.com/linjie-1/guigulive-operation/wiki/lesson-6 
* 以太坊智能合约全栈课程第二期硅谷Live github 每日优质内容复盘(1.25~1.28) 
https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98

