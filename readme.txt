iec104
使用py3标准库写iec104的模拟主站（server）与从站（client）。
client端实现内容有：
1、单点，双点、带时标、分闸、合闸、执行或取消执行，以上组合的遥控。
2、发送S帧。
3、发送总召帧。
4、发送启动帧。
5、归一化值设定。
6、遥调设定。
7、实现多线程的并发。

server端实现的内容有：
1、自动回应遥控、归一化、遥调帧（均回应确认执行）。
2、回应总召帧，发送遥测、遥测帧。
3、实现多线程，回应多客户端的连接。

此软件上述功能与PMA软件连调成功，不保证规约的正确性。

遥测帧来源：
使用线程“initYcList”，自动生成化为的遥测帧数据部分，供iec104server类调用回复。

遥信帧来源：
使用线程“initYxList”，自动生成化为的遥信帧数据部分，供iec104server类调用回复。

存在的问题：
1、主站地址、子站地址没法修改，默认都为1。
2、公共地址无法修改，默认为1。
3、遥控时的传输原因，在前两帧选择：0600，0700（激活、激活确认）上没有问题。
在后两帧（取消时），各个软件平台处理不一致。
在取消遥控时，
有些软件处理方式为：0800，0900（停止激活、停止激活确认，公司软件的方式）。
有些软件处理方式为：0800、0a00（停止激活、激活终止，PMA的方式）。
有些软件处理方式为：0600、0a00（激活、激活终止）明显这个应该是错的。
此软件处理方式为PMA的处理方式。

使用方式：
通过参数调用脚本。控制运行在server端或client端。
其它参数可以通过--help参数查看帮助。

另外，client端行为，在iec104client类->run（）里面实现。
方法有：
1、sendZZao（）
2、sendSFrame（）
3、sendYaoKongActive（）
4、sendYaoTiao（）
5、sendGuiYi（）

server端行为，在iec104server类->run（）里面实现。
每接收到一帧数据，判断帧的类型，然后作相应的回复。