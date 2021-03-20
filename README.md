# SNORT 第一次笔记

## 基本语法：规则头+规则选项

### 规则头

* 规则动作：规则被激发的时候要做什么
  * alert：生成警报并记录数据包
  * log：记录数据包
  * pass：忽略数据包

* 协议：规则针对什么报文
* 源IP地址、源端口
* 一个小箭头：->
* 目的IP地址、目的端口

### 规则选项

* content：数据包中是否含有此内容
* msg：规则触发之后在控制台显示该提醒消息

### 示例

alert tcp any any -> 192.168.1.0/24 143 (content: "|90C8 C0FF FFFF|/bin/sh"; msg: "IMAP buffer overflow!";)

遇到下列情况时产生警报：

使用TCP协议，源自任意IP地址任意端口，目的地址为192.168.1.0/24的143端口

报文中含有内容：|90C8 C0FF FFFF|/bin/sh

警报内容：IMAP报文的栈溢出攻击！

## 本次作业内容

* 创建GitHub仓库："idshwk1"
* 添加snort规则："test.rules"，此规则用于检测如下报文：
  * 目标端口是8080
  * TCP的ACK类型flag
  * 内含字符串："I am IDS Homework I"，位于第100~第200字节
  * 警报内容："TEST ALERT"

## 作业作答

### 规则头

alert tcp $EXTERNAL_NET any -> $HOME_NET 8080

即：对如下TCP报文发出警报：来自外部网络任意端口，访问本地8080端口

### 规则选项

|  属性   |         内容          |                  解释                   |
| :-----: | :-------------------: | :-------------------------------------: |
|   msg   |     "TEST ALERT"      |                    -                    |
|  flags  |           A           |               检测ACK类型               |
| content | "I am IDS Homework I" |                    -                    |
| offset  |          100          | content选项的修饰符，设定开始搜索的位置 |
|  depth  |          100          | content选项的修饰符，设定搜索的最大深度 |
|   sid   |        114514         |       snort规则id，自己设一个试试       |

综上，答案应该是：

alert tcp $EXTERNAL_NET any -> $HOME_NET 8080 (msg: "TEST ALERT"; flags: A; content: "I am IDS Homework I"; offset: 100; depth: 100; sid: 114514)


