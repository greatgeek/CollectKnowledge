#### UTF-8和GBK编码概述

**UTF-8（8-bit Unicode TRansformation Format）**是一种针对Unicode的可变长字符编码，又称万国码，它包含全世界所有国家需要用到的字符，是国际编码，通用性强，是用以解决国际上字符的一种多字节编码。由Ken Thompson 于1992年创建。UTF-8用1到4个字节编码UNICODE字符，它对英文**1**个字节，对中文使用**3**个字节来编码。用在网页上可以同一页面显示 中文简体繁体及其他语言（如日文，韩文）。

**GBK（GBK即“国标”、“扩展”汉语的第一个字母）（Chinese Internal Code Specification）**是汉字编码标准之一，全称《汉字内码扩展规范》。

GBK是国家标准GB2312基础上扩容后兼容GB2312的标准。GBK的文字编码是用双字节来表示，即不论中、英文字符均使用双字节来表示（注意，GB系列编码是利用了字节中的最高位和ASCII编码区分的，可以和ASCII码混用。所以全角模式下英文是2字节，半角模式下英文还是1字节）。为了区分中文，将其最高位都设定成1。GBK包含全部中文字符，是国家编码，通用性比UTF-8差，不过UTF-8占用的数据库比GBK大。

#### 简单概况

UTF-8英文1字节，中文3字节，在编码效率和编码安全性之间做了平衡，适合网络传输，是理想的中文编码方式。

GBK英文1字节（半角1字节，全角2字节），中文2字节，GBK的范围比GB2312广，GBK兼容GB2312。

#### Note

Windows 在中国使用GBK编码，在其他地区使用UTF-8编码。这会造成一些使用UTF-8输出日志的软件会输出乱码。例如Tomcat

**解决办法：**

1. 找到Tomcat目录下的**conf/logging.properties**配置文件，打开并搜索 **java.util.logging.ConsoleHandler.encoding**. 将UTF-8改为GBK，即可解决问题
2. 保存后，重启Tomcat

#### 参考来源

[UTF-8和GBK编码概述](https://www.cnblogs.com/sochishun/p/7026762.html)

[启动tomcat时出现乱码——淇℃伅](https://blog.csdn.net/weixin_42443070/article/details/88085762)

