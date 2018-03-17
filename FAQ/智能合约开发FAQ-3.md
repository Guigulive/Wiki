## 第三课问题整理

### 关于mapping是否有更详细的介绍？为何说mapping不能够简单地遍历？
* mapping只能作为成员变量，不能作为合约的局部变量。 
* 当key不存在时，通过value = employees[key]返回的是这个类型的默认值，不会抛出异常。判断mapping里面是否存在某个key的方法，只能通过查看这个mapping里面对应的key的值是不是那个值的类的默认值来决定。 
* 无法遍历整个mapping。mapping是在EVM中的storage中出现的数据结构，storage天生就是Key-Value实现的，所以mapping直接基于storage，将key映射到一个内存地址。mapping中所有可能的键已被虚拟化的创建，被映射到一个默认值，这也导致mapping没有长度，是不可遍历的。 

【参考】 
* 官方文档中关于mapping的说明 
http://solidity.readthedocs.io/en/latest/types.html#mappings
* Store data in mapping vs. array 
https://ethereum.stackexchange.com/questions/2592/store-data-in-mapping-vs-array
* mapping和hash table的区别 
https://ethereum.stackexchange.com/a/17410
* how-does-mapping-work 
https://ethereum.stackexchange.com/questions/7713/how-does-mapping-work/17410#17410


### 为什么课程中说合约的所有成员变量都是肉眼可见的？
* 隐私在公链上面是不存在的，因为都可以在storage里面看到，除非使用ZKSNARK／STARK等方法。 
* 即使声明为private，但并不意味着其他人不能读取它的内容，它只是意味着它只能在合约中被访问，但实际上，由于整个区块链存储在许多计算机上，所以存储在变量中的信息总是可以被其他人看到，这是你应该记住的第一个安全考虑。访问权限只是阻止了其它合约访问函数或修改数据。 

### 为何外部能够访问到public的状态变量？
* 编译器会自动为公共变量创建getter 函数。为了使其他的合约和用户能够更改公共变量的值，你还需要创建一个setter函数。 

### this.function()究竟是external调用还是internal调用？	
* 是external调用。我们可以在合约的调用函数前加this.来强制以external方式的调用。需要注意的是这里的this的用法与大多数语言的都不一致。 
* external调用时，实际是向目标合约发送一个消息调用。消息中的函数定义部分是一个24字节大小的消息体，20字节为地址，4字节为函数签名。 

【参考】 
* 区块链编程语言Solidity语言函数可见性深入详解 
https://www.jianshu.com/p/c3e3ccb466ec

---
上述问题整理自：
* 以太坊智能合约全栈课程第二期第三课课程内容 
* 以太坊智能合约全栈课程第二期第三课补充学习笔记 
https://github.com/linjie-1/guigulive-operation/wiki/lesson-3
* 以太坊智能合约全栈课程第二期硅谷Live github 每日优质内容复盘(1.14~1.16) 
https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98
