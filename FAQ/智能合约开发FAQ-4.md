## 第四课问题整理

### 如何进行第四课作业环境的布署？
* 对于Truffle本身的学习，我们的虚拟机镜像已经帮大家配置好了truffle， testrpc，npm等等版本，所以安装truffle，testrpc，npm，nvm的步骤都可以省略，请大家可以直接试运行truffle init，来产生标准的和视频讲解一样的metacoin工程 
* 对于react的前端整合部分，因为react box在视频录制之后更新，所以很多东西都有一些小的变化，为了方便大家，请用以下这些步骤运行那个simple storage的例子： 
1. 下载官方虚拟机镜像（版本号：5.1.22）
> 国外同学/ 能翻墙同学戳：https://www.dropbox.com/s/bikdtta0ecw1l1r/juicebox-desktop-1.3.0%20Clone.ova?dl=0
> 不能翻墙的同学戳：chinatechstory.com/juiceboxClone.ova
2. cp -r react-box payroll
3. cd payroll
4. npm install
5. 打开第二个terminal，启动testrpc
6. truffle migrate
7. npm run start
* 测试合约建议大家采用java script 因为写起来交互相对简单，Solidity本身的测试需要绕开各种问题，助教和老师讨论后，感觉并不是特别必要专门要求为了绕过这些东西而绕过，目前推荐大家使用javascript书写测试。 

【参考】 
* Windows环境下跑通Truffle开发环境 by 二期学员申龙斌 
http://mp.weixin.qq.com/s/lK9O0gEbDTJgcpyM3x2e9g 

* Mac环境下的Truffle课程环境配置 by 二期学员聪颖
https://github.com/Congying1112/LaoDongKeChengRemark/blob/master/TruffleEnvConfigForMac.md 

### GitHub上面的代码没法run start， 提示node_modules missing，怎么办？
* 需要执行npm install 

### 已经安装了truffle 4.0.5，想卸载，卸载的方法是什么？ 
* 执行命令sudo npm uninstall -g truffle可以卸载。 

### 为何参考了如上问题以及视频中的步骤，还是无法跑通整个流程？
* payroll.sol程序导入truffle工程时需要注意： 
1. 注意如果使用视频中的编译器版本时，public view这样的语法不被支持，编译时会报错。解决方法可以是把view字样删除，或者改成constant。 
2. truffle.js需要修改为如下内容：（注意端口号和网络名development，否则执行truffle test时会报错No network specified） 
```
module.exports = {		
  // See <http://truffleframework.com/docs/advanced/configuration>		
  // to customize your Truffle configuration!		
  networks: {		
    development: {		
      host: "localhost",		
      port: 8545,		
      network_id: "*" //Match any network id		
    }		
  }		
};
```
3. 修改utils里getWeb3.js的下面这句的端口号，使得该端口号和truffle.js中保持一致。并且务必开启testrpc。否则npm run start执行完并修改storage为10之后，刷新网页后不会有变化。 
```
var provider = new Web3.providers.HttpProvider('http://127.0.0.1:8545') 
```
4. 在浏览器中按F12，可以查看到相关的报错信息，进行调试来解决问题。JS中用console.log命令输出的内容也都会反映在其中。 

5. 在实在无法解决问题时，删除build目录，重新编译，重新migrate，有一定概率可以解决你碰到的问题。 

### pure, view这两个关键词有什么用途？ 
* view就是constant，在旧版本中叫constant，现在叫view 
* view和constant都是保证这个函数不会对合约的状态做修改 
* pure则保证不但不会做修改，而且连这个函数的状态都不会读取 
* 但是没有卵用，因为solidity不会enforce这些，cost太高，这些都只是作为一个程序当中的警示/标识，在团队开发的时候给其他人看的 
* compiler会报警，但老版本的compiler可以通过。所以这些都是意义大于实质。 

### 使用JS测试时如何在浏览器中输出相关信息以方便调试？
* 可以使用console.log命令。比如要输入account变量的内容的话： 
```
console.log("employeeId: ",account);
```

### truffle console中如何像remix界面里那样通过addFund来加钱，并且指定支付帐户？ 
* 可以在调函数的时候在花括号里设置参数，比如：  
```
payroll.addFund({from: accounts[0], value: 1});
```
* Web3中有提供函数把eth转成wei：web3.toWei()，所以上例中如果是要add 1eth的话需要写成： 
```
payroll.addFund({from: accounts[0], value: web3.toWei(1)});
```
* 同理，如果需要在执行getPaid函数时指定调用该命令的员工，可以执行： 
```
instance.getPaid({{from: employeeAddr});
```
* 调用合约里的函数相当于发起一个交易，调用时可以设定交易的一些参数，上例中的from是设定交易发起方，value是设定交易携带的金额。在下面的文档中可以查看到这个交易对象可以包含的内容： 
https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethsendtransaction 

### 在执行addFund时报错说钱不够，应该如何处理？ 
```
truffle(development)> pr.addFund({value:web3.toWei(100)})
Error: Error: sender doesn't have enough funds to send tx. The upfront cost is: 100471238800000000000 and the senders account only has: 99830579499999958350
```
* 测试网络默认每个账户有100 eth，这边addFund传的value是100 eth，再加上gas超过了账户的余额。所以少加点钱就可以了。 

### 用testrpc布署完合约之后，我如何才能知道合约的owner？ 
* 部署合约默认使用account[0],所以owner可以通过在testrpc命令执行完后查看account[0]得知。 

### 在truffle migration时出错：Error: exceeds block gas limit。如何自定义gasprice和gaslimit？ 
* 在启动testrpc时自定义下gasprice  gaslimit，改大点。比如可以执行如下命令来修改：（具体改成多少可以自行尝试一下，每个人的情况都不太一样。） 
```
testrpc -g 30000000000 -l 200000 
```
* gasLimit的默认值相对较小，详细参数如下： 
> --gasPrice/-g <gas price> (default 20000000000)
> --gasLimit/-l <gas limit> (default 90000)

### 在执行addEmployee时提示gas不足该怎么办？ 
* 执行addEmployee等复杂的操作时需要比较高的gas，那么需要在调用addEmployee的时候把gas作为参数传进去，设高一些，比如： 
```
payroll.addEmployee(address, salary, {
  from: account,
  gas: 4000000
}).then( ... ) /*以下省略*/
```

### 通过web3来调用solidity里面的方法都是需要添加回调函数吗？ 
* 是的，这个可以算是Node.js的特性之一，因为Node.js调用Solidity这些方法是异步的，所以需要设置回调函数，在调用完成后会触发回调函数进行后续操作。比如： 
```
MetaCoin.deployed().then(contract => {metacoin = contract})
```

### 为何在执地函数名.call()时状态变量未被修改？ 
* 函数名.call()是本地执行，不会发送transaction到链上，并且只返回true/false。正确的方法是直接调用函数名()，这将返回一个交易信息。 

### juicebox中执行某些步骤需要密码怎么办？
* 密码就是juicebox。 

### 何时需要执行migration --reset？ 
* 如果之前已经运行过truffle migrate，并且migrations文件下没有新加一个migration文件而是重用并修改之前的2-deploy...js文件，那么修改后migrate要加参数--reset： 
```
truffle migration --reset
```
【参考】 
http://truffleframework.com/docs/getting_started/migrations 

### 执行函数.call()之后的几个返回值不太明白，具体是什么意思？ 
```
truffle(development)> pr.calculateRunaway.call()
{ [String: '10000000000000000000'] s: 1, e: 19, c: [ 100000 ] }
```
* 这个返回的结果是bigNumber格式的值。加.valueOf()之后可以转换成数字。JS里面对非常大的数字支持不好，所以可以使用bignumber这个JS库进行对大数的支持，所以web3在内部使用了bignumber。valueOf是bignumber的一个方法，就是取出对象里面的值。 
【参考】 
http://mikemcl.github.io/bignumber.js/

### Timestamp的修改方式是什么？ 
* testrpc提供了一个方法evm_increaseTime，可以用来修改timestamp。 
```
evm_increaseTime : Jump forward in time. Takes one parameter, which is the amount of time to increase in seconds. Returns the total time adjustment, in seconds.
```
* 参考的调用方式如下： 
```
//挖一个区块
web3.currentProvider.send({jsonrpc: "2.0", method: "evm_mine", params: [], id: 0})
/增加11s
web3.currentProvider.send({jsonrpc: "2.0", method: "evm_increaseTime", params: [11], id: 0})
```
【参考】 
https://github.com/ethereumjs/testrpc#implemented-methods

---
上述问题整理自：
* 以太坊智能合约全栈课程第二期第四课课程内容 
* 以太坊智能合约全栈课程第二期第四课补充学习笔记 
https://github.com/linjie-1/guigulive-operation/wiki/lesson-4
* 以太坊智能合约全栈课程第二期硅谷Live github 每日优质内容复盘(1.17~1.24) 
https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98
