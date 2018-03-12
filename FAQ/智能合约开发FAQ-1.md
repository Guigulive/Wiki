## 第一课问题整理

### 有没有关于以太坊的比较基础的介绍？

一个基础的以太坊介绍：	https://zhuanlan.zhihu.com/p/24012669	

实际上更准确的资料仍然是官网。

以太坊官网：	https://www.ethereum.org/	

以太坊官方github：https://github.com/ethereum/	

以太坊mist客户端：	https://github.com/ethereum/mist	

查看以太坊区块链：https://etherscan.io/	


### Solidity中有没有浮点类型？
Solidity中没有浮点数类型。所有的非整数都是不支持的。并且如果被赋予的值超过申明类型的大小，还会存在溢出问题。（最后一课智能合约安全中会讲到）
推荐两个网站：

http://www.tryblockchain.org 

https://solidity.readthedocs.io/en/develop/types.html#value-types 


### constant有什么作用？	

constant修饰的常成员函数，意在表示不能更改内部成员变量。但是在目前的版本中，constant的修饰是无效的，只是一个视觉上的警视。

constant在修饰变量时是有效的，修饰的变量不能被修改。


### address.transfer和address.send有什么区别？

执行address.transfer(value)调用之后，调用后的异常将会被返回。

执行address.send(value)调用之后，调用后的异常将不会被返回，只会返回一个false。	


### 以太坊地址（Address）中的账户地址与合约地址有什么区别？

在比特币中，有一个概念叫做地址，比特币存储在这里--就像是一串比特币的银行账户数字。这个在以太坊中一般被称作账户（Address），账户有两种类型：

类型1：只存储ETH的账户。（账户地址）该类型的账户可以参考如下示例：	

https://etherscan.io/address/0x2d7c76202834a11a99576acf2ca95a7e66928ba0	

类型2：不仅存储 ETH，同时也有可以运行的代码。（智能合约）这些智能合约可以通过一个交易发送ETH的到账户里。一旦智能合约被上传，它就在那里等待被激活。该类型的账户可以参考如下示例：	

https://etherscan.io/address/0xcbe1060ee68bc0fed3c00f13d6f110b7eb6434f6	



### solidity变量的作用域与其它语言有何区别？

与Javascript很像，和C++，JAVA不同。只要在函数中定义了局部变量，作用域为整个函数。	

比如，在if内定义一个变量，在if内肯定有效，但在if外面也有效。
				
### 为什么课上老董反复强调：一个函数中，一定要先把内部变量修改完成之后，再转到外部钱，不能更改顺序？（课程中的示例代码中，要先进行lastPayday的更新，再transfer value。）	

涉及到以太坊智能合约的re-entry攻击的问题。（最后一课智能合约安全中会更次讲到）	
​				

### 能否自己修改合约的balance？

合约的balance不能直接接改，而是自动计算的。比如下面的代码是不能够通过编译的。	
```
function addFund(uint amount) returns (uint) {	
	this.balance = this.balance + amount;
	return this.balance;
}
```
正确的做法是在调用带payable的函数时附带上value给合约打钱，合约的balance会自动变化。	
```
function addFund() payable returns (uint) {	
	return this.balance;
}	
```


### Contract布署是什么意思？	

Smart Contract可以简单的理解为一段可执行的程序片段，具体的代码经过solidity编写之后，发布到区块链上。	

而以太坊的智能合约也可以理解为一个特殊的交易（包括可执行代码的），被发送出去后会被矿工打包记录在某一个区块中。	

				
### Contract布署之后如何调用？

Contract部署到链上后会有一个合约地址，通过合约地址可以调用其中的方法，调用的形式相当于向合约地址发起一个transaction。	

每个合法的address都可以发起调用，普通的用户地址可以调用合约，同时合约之间也是可以相互调用的。	

				
### 以太坊中的Gas有什么作用？

当你激活一个智能合约的时候，你在要求整个网络内的每个矿工个体分别执行里面的运算。这会花费他们的时间和精力，Gas是你为这项服务向矿工们支付的机制。	
报酬是小额的以太币，想要运行智能合约的人的需要支付报酬来使合约工作。付款款项（单位以太币）＝ Gas数量（单位Gas） x Gas price（单位以太币／Gas）	
智能合约越复杂（计算步骤的数量和类型，占用的内存等），用来完成运行就需要越多Gas。

让智能合约花费Gas/以太币/钱可以防止人们随意激活合约，解决了垃圾交易以及相关问题，如果运行智能合约免费，此类问题会发生。	
​	


### Gas price和Gas有什么区别？

任何特定的合约所需的运行合约的Gas数量是固定的，由合约的复杂度决定，而Gas价格由想运行合约的人规定，在他们提交运行合约请求的时候（有点类似于比特币的交易费）。	

每个矿工会根据Gas的价格的高低来决定他们是否想作为区块的一部分去运行此合约。如果你希望矿工运行你的合约，你最好提供高一点的Gas价格。	

在某种程度是这是一场基于合约运行有多愿意付费驱动下的竞价。	

​				
### Contract调用时的Gas的支付方是谁？

Gas是由一开始发起transaction的地址进行支付。	

比如frank.transfer这个操作是调用者（不一定是frank）支付gas。	

再比如"合约A调用合约B再调用合约C"这个过程中所产生的所有计算步骤所消耗的Gas也都是由调用合约A的人来承担的。	

详细可以参见此处：http://ethfans.org/posts/797	
​

### Transaction中Gas limit的含义？

如果交易所产生的所有计算步骤（包含原始消息和可能被触发的所有其他消息）所需要的Gas的总量小于等于交易中指定的Gas Limit，交易会被处理。

如果实际消耗的Gas的总量超过交易中Gas Limit，所有的变动都会被revert，除了交易仍然有效，且费用仍可以被矿工收取。

​				
### 关于transaction cost和execution cost的区别是什么？

Execution cost包括存储全局变量以及方法调用相关的运行环境的开销。同一个函数，每次调用时的execution cost有可能是不同的。（比如全局变量发生了变化导致）	

而transaction cost和编译后的合约代码长度相关，也和execution cost相关。同一个合约，每次执行时transaction cost - execution cost的值应该是不变的。 	
详细可以参见此处：https://ethereum.stackexchange.com/questions/5812/what-is-the-difference-between-transaction-cost-and-execution-cost-in-browser-so（注意贴子要看到底哦！）	
​				


### Remix在线编译器的网址？

Remix是一个非常方便的solidity集成开发环境，省去了许多安装操作，非常方便上手。

http://remix.ethereum.org/	
​	


### 账户（Address）的双引号，什么时候要加？什么时候不需要？	

在solidity源代码中，地址就是一串十六进制数，不需要加双引号。

但在Remix的对话界面中输入address时，务必在address两边加上双引号（""），否则会报错。而且报错的消息非常诡异，务必小心。

remix对于不打引号的值会作为javascript中的int处理，地址明显越过最大整数的范围，加引号才算bignumber，也就是说输入一个超大int256数也要加引号。


### 向contract充值后怎么数值不对？

remix的界面有默认单位的选择，其余的地方都没有单位的选择。传入金额数值的默认单位为wei。

如果你要传入1 eth，则在数值之后加上ether。例如：

``` salary = salary * 1 ether;```


注：
上述问题整理自：
* 以太坊智能合约全栈课程第一课课程内容		
	 以太坊智能合约全栈课程第一课补充学习笔记	https://github.com/linjie-1/guigulive-operation/wiki/Lesson-1-%E8%A1%A5%E5%85%85%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0		
* 硅谷Live github 每日优质内容复盘(1.7~1.11) https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98
* 硅谷Live github issues https://github.com/linjie-1/guigulive-operation/issue
