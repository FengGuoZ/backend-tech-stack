#### 01 window下链接socket网络库

window下编译含socket API的文件时，需加上`-lwsock32`，表示连接到wsock32动态库

```powershell
> g++ server.cpp -lwsock32 -o server
```



#### 02 chcp 65001

将powershell字符集设置为UTF8编码，否则在输出中文字符时会乱码



#### 03 window网络配置

![image-20220926231718216](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220926231718216.png)



#### 04 windows快捷键

|      快捷键      |         用途         |
| :--------------: | :------------------: |
|      Ctrl+C      |         复制         |
|      Ctrl+X      |         剪切         |
|      Ctrl+V      |         粘贴         |
|      Ctrl+Z      |         撤销         |
|      Ctrl+Y      |         重做         |
|      Win+D       |       显示桌面       |
|      Win+E       |    打开文件管理器    |
|   Ctrl+Alt+Del   |    打开任务管理器    |
|     Win+Tab      |  当前运行程序缩略图  |
| Shift+左右方向键 | 微软办公软件文字选中 |
|     Ctrl+End     |     跳转文章开头     |
|    Ctrl+Home     |     跳转文章结尾     |
|                  |                      |
|                  |                      |
|                  |                      |
|                  |                      |
|                  |                      |
|                  |                      |
|                  |                      |



#### 05 Windows环境变量

**相当于Linux下Path路径**

- 系统环境变量：作用于系统下所有用户
- 用户环境变量：仅作用于当前用户

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220927102514841.png" alt="image-20220927102514841" style="zoom: 67%;" />



#### 06 下位机程序烧写

- 用TCPUDPbg连接助手，连接192.168.1.30:5678，进入9秒倒计时。
- 运行tftpd32.exe，完成以下设置
  - 选择TCP Client
  - host = 192.168.1.30，port = 69
  - 正确加载文件路径
  - 选择512block size、点击put


<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20210521220907531.png" alt="image-20210521220907531" style="zoom:50%;" />



#### 07 iperf网络测速工具

软件下载及环境配置参见：https://iperf.fr/iperf-download.php	https://tsov.net/uupee/58977/

指令格式：

- 简单测速：iperf3 -c 192.168.1.100
- 持续测速：iperf3 -c 192.168.1.100 -t 1000

详细使用方法：https://www.cnblogs.com/Ph-one/p/10767962.html



