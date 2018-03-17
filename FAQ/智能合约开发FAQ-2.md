## 第二课问题整理

### 下面的代码中delete有意义吗？既然delete其实就是个clear操作，是不是可以认为不delete也没问题？
```
delete employees[index];
employees[index] = employees[employees.length - 1];
```
不进行delete操作也没问题。但注意，对于employees数组中最后一位需要通过delete方式删除。

参考：
http://solidity.readthedocs.io/en/develop/types.html
“It is important to note that delete a really behaves like an assignment to a, i.e. it stores a new object in a.”

### assert()和require()是不是在编译后是一样处理的？那我们在使用的时候什么时候用assert，什么时候用require呢？
回滚机制（reverting）是为了确保transaction的原子性，主要分为require样式和assert样式。
assert用于检查程序运行条件是否满足（例如数组访问是否越界），require用于检查用户输入或外部调用返回值。
一个重要区别是：触发assert()会消耗掉所有的gas，而触发require()会把剩余的gas返还。
异常一般会由子调用向上传递。但是send和底层调用call、delegatecall、callcode除外，它们只会返回false（注意：如果被调用账户不存在，call、delegatecall和callcode返回true）。

其它区别请参考：
https://ethereum.stackexchange.com/questions/15166/difference-between-require-and-assert-and-the-difference-between-revert-and-thro
https://medium.com/blockchannel/the-use-of-revert-assert-and-require-in-solidity-and-the-new-revert-opcode-in-the-evm-1a3a7990e06e

### employees动态数组声明好之后，用employees[0] = 赋值对象进行赋值为何会报错？
动态数组声明好之后，用索引的方式赋值是会报错的，要用push的方式进行赋值。
在增加了新元素之后，就能用索引的方法进行调用了。
注：array.push()会返回该数组的长度，由于数组的index从0开始标注，所以array.push() - 1的值即为数组的index。

### Solidity中默认的可视度是什么？	
在Solidity中，默认的function可视度为public，即默认为外部函数可以调用！
默认的状态变量的可视度为private，如果要从外部看到该状态变量，可以将其设为public。

### remix里的accoun数量只有5个，要怎么增加呀？
无法增加。如果要使用更多account来进行测试，只能在代码中自行定义或者在remix的参数输入界面中输入其它account来传入合约中。

注：
上述问题整理自：
* 以太坊智能合约全栈课程第二课课程内容		
	 以太坊智能合约全栈课程第二课补充学习笔记	https://github.com/linjie-1/guigulive-operation/wiki/Lesson-2-%E8%A1%A5%E5%85%85%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0
* 硅谷Live github 每日优质内容复盘(1.12~1.13) https://github.com/linjie-1/guigulive-operation/wiki/%E6%AF%8F%E6%97%A5%E4%BC%98%E8%B4%A8%E5%86%85%E5%AE%B9%E5%A4%8D%E7%9B%98
