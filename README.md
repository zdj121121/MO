# MO
学校通信软件应用实践（短消息篇）实验一作业

用户的MO短信由两部分构成：发送号码和发送内容，再加上业务申请时设置的匹配模式，这三项共同构成了指令匹配的依据。

当一条MO短信到MISC进行鉴权时，MISC首先对发送号码（长号码）进行匹配，按照最大匹配+精确匹配的原则进行，如果有匹配成功的，则取出对应业务代码和指令类型；在上一步匹配出来的列表中，再对指令内容进行匹配，也是按照最大匹配+精确匹配的原则进行。

如果有匹配上的结果，则取出对应的业务代码和指令类型；如果没有能对应上的匹配结果，则取列表中的最后一条的ServiceID作为匹配出来的ServiceID，同时通知短信网关将此条短信当做普通MO向SP转发。

MO指令匹配算法示例

seq |	AccessNO |	FeatureStr |	ANCheckFlag |	FSCheckFlag

1	   |   10628888	   |  xw	    |      1	   |      0

2	   |   1062888801	 |  xw	    |      0     |    	0

3	   |   1062888801	 |  xw1	    |      0     |    	1

4	   |   10628888	   |  01xw	  |      1     |    	1

5	   |   10628888	   |  (null)	|      0	   |      0

[备注]AccessNO表示MO的发送号码、FeatureStr表示指令内容、ANCheckFlag表示对AccessNO是否使用精确匹配（1代表精确匹配）、FSCheckFlag表示对指令内容是否使用精确匹配（0代表模糊匹配）。

针对上表的设置：

用户发送 xw1到10628888011我们匹配第3条记录

用户发送 xw01到1062888801我们将匹配到第2条记录(先最长匹配接入号)

用户发送01xw到1062888802我们匹配到第5条记录（不会匹配到第4条记录，因为第4条记录的AccessNO是精确匹配）

用户发送01xw到10628888我们匹配到第4条记录

用户发送 xw01到10628888我们匹配第1条记录

用户发送 A到10628888我们匹配第5条记录