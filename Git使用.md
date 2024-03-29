#### git init 初始化本地仓库

<img src="https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614142344604.png" alt="image-20220614142344604" style="zoom:33%;" />



#### git branch 查看分支信息

![image-20220717010008829](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220717010008829.png)



#### git clone 拷贝远程仓库

```bash
$ git clone https://github.com/FengGuoZ/FengGuoSZ.git
```

![image-20220614142004569](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614142004569.png)



#### git remot -v 查看远程仓库信息

![image-20220614142052988](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614142052988.png)





#### git status 查看仓库状态，显示变更

![image-20220614162150151](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614162150151.png)



#### git add . 添加所有文件到暂存区

![image-20220614162628591](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614162628591.png)



#### git commit 提交暂存区到本地仓库

```bash
$ git commit -m "添加文件 千年文脉.md"
```

![image-20220614162823341](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614162823341.png)



#### git log 查看当前提交记录

![image-20220614162910104](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614162910104.png)



#### git push 上传代码至远程

```bash
$ git push origin main # 将本地main分支上传至远程main分支
```

![image-20220614163252596](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614163252596.png)



#### git config 查看配置信息

```bash
$ git config user.name
$ git config user.email
```

![image-20220614142201571](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220614142201571.png)



#### git config 设置配置

git采用**三级配置结构**

- 仓库级(--local) > 用户级(--global) > 系统级别(--sys)
- 每一级都有各自的.config文件

```bash
git config --global user.email "1541441163@qq.com"		# 将用户邮箱设置为1541441163@qq.com，配置结果写入用户级.config文件中
git config --global core.quotepath false				# 解决git status显示中文文件名乱码的问题
git config --global http.proxy http://127.0.0.1:33210	
git config --global https.proxy https://127.0.0.1:33210	# 设置git的https代理，解决挂上vpn后git由于不走代理无法push的问题，33210为代理端口
```

![image-20220716162628082](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220716162628082.png)

![image-20220718235318287](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220718235318287.png)

![image-20220718235609499](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220718235609499.png)



#### git config --list 查看当前配置

![image-20220719000229155](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220719000229155.png)



#### git commit --amend 修改最后一次提交的注释

![image-20220811003323182](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220811003323182.png)

![image-20220811003345660](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20220811003345660.png)



#### git rm --cached -r 移除文件的版本控制

不会删除文件本身

![image-20221209144441259](https://figure-bed-zwd.oss-cn-hangzhou.aliyuncs.com/img_for_markdown/image-20221209144441259.png)



#### .gitignore 配置不进行版本管理的文件

支持通配符

```
# 不需要加入git管理的文件类型
*.mat
*.l
```

