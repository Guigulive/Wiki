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
1. 修改getWeb3.js的下面这句的端口号，使得该端口号和truffle.js中保持一致。 
```
var provider = new Web3.providers.HttpProvider('http://127.0.0.1:8545') 
```
2. truffle.js需要修改为如下内容：（注意端口号） 
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

### 使用JS测试时如何在浏览器中输出相关信息以方便调试？
* 可以使用console.log命令。比如要输入account变量的内容的话： 
```
console.log("employeeId: ",account);
```

### truffle console中如何像remix界面里那样通过addFund来加钱？ 
* 可以在调函数的时候在花括号里设置参数，比如：  
```
payroll.addFund({value: xxxx})
```

### truffle console中如何像指定执行该命令的帐户？ 
* 同上一问，也可以在调函数的时候在花括号里设置参数，比如：  
```
payroll.addFund({from: xxxx})
```

### 用testrpc布署完合约之后，我如何才能知道合约的owner？ 
* 部署合约默认使用account[0],所以owner可以通过在testrpc命令执行完后查看account[0]得知 

### 如何自定义gasprice和gaslimit？ 
* 可以执行如下命令来修改： 
```
testrpc -g 30000000000 -l 200000 
```
* gasLimit的默认值相对较小，详细参数如下： 
> --gasPrice/-g <gas price> (default 20000000000)
> --gasLimit/-l <gas limit> (default 90000)

### 通过web3来调用solidity里面的方法都是需要添加回调函数吗？ 
* 是的，需要用JS里的promise来获取实际的结果，比如： 
```
MetaCoin.deployed().then(contract => {metacoin = contract})
```

---
上述问题整理自：
* 以太坊智能合约全栈课程第二期第四课课程内容 
* 以太坊智能合约全栈课程第二期第四课补充学习笔记 
https://github.com/linjie-1/guigulive-operation/wiki/lesson-4
* 以太坊智能合约全栈课程第二期硅谷Live github 每日优质内容复盘(1.17~1.20) 
https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98
