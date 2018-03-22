## 第四课问题整理

### 如何进行第四课作业环境的布署？
* 对于Truffle本身的学习，我们的虚拟机镜像已经帮大家配置好了truffle， testrpc，npm等等版本，所以安装truffle，testrpc，npm，nvm的步骤都可以省略，请大家可以直接试运行truffle init，来产生标准的和视频讲解一样的metacoin工程 
* 对于react的前端整合部分，因为react box在视频录制之后更新，所以很多东西都有一些小的变化，为了方便大家，请用以下这些步骤运行那个simple storage的例子： 
1. 下载官方虚拟机镜像（版本号：5.1.22）
2. cp -r react-box payroll
3. cd payroll
4. npm install
5. 打开第二个terminal，启动testrpc
6. truffle migrate
7. npm run start
* 测试合约建议大家采用java script 因为写起来交互相对简单，Solidity本身的测试需要绕开各种问题，助教和老师讨论后，感觉并不是特别必要专门要求为了绕过这些东西而绕过，目前推荐大家使用javascript书写测试。 
【参考】 
* Windows环境下跑通Truffle开发环境 

http://mp.weixin.qq.com/s/lK9O0gEbDTJgcpyM3x2e9g 

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
3. 修改utils里getWeb3.js的下面这句的端口号，使得该端口号和truffle.js中保持一致。否则npm run start执行完并修改storage为10之后，刷新网页后不会有变化。 
```
var provider = new Web3.providers.HttpProvider('http://127.0.0.1:8545') 
```
4. 在实在无法解决问题时，删除build目录，重新编译，重新migrate，有一定概率可以解决你碰到的问题。 

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

### 用testrpc布署完合约之后，我如何才能知道合约的owner？ 
* 部署合约默认使用account[0],所以owner可以通过在testrpc命令执行完后查看account[0]得知 

### 在truffle migration时出错：Error: exceeds block gas limit。如何自定义gasprice和gaslimit？ 
* 在启动testrpc时自定义下gasprice  gaslimit，改大点。比如可以执行如下命令来修改： 
```
testrpc -g 30000000000 -l 200000 
```
* gasLimit的默认值相对较小，详细参数如下： 
> --gasPrice/-g <gas price> (default 20000000000)
> --gasLimit/-l <gas limit> (default 90000)

### 通过web3来调用solidity里面的方法都是需要添加回调函数吗？ 
* 是的，这个可以算是Node.js的特性之一，因为Node.js调用Solidity这些方法是异步的，所以需要设置回调函数，在调用完成后会触发回调函数进行后续操作。比如： 
```
MetaCoin.deployed().then(contract => {metacoin = contract})
```

### 为何在执地函数名.call()时状态变量未被修改？ 
* 函数名.call()是本地执行，不会发送transaction到链上，并且只返回true/false。正确的方法是直接调用函数名()，这将返回一个交易信息。 

---
上述问题整理自：
* 以太坊智能合约全栈课程第二期第四课课程内容 
* 以太坊智能合约全栈课程第二期第四课补充学习笔记 
https://github.com/linjie-1/guigulive-operation/wiki/lesson-4
* 以太坊智能合约全栈课程第二期硅谷Live github 每日优质内容复盘(1.17~1.20) 
https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98
