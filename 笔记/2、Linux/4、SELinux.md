主体：程序

目标：文档

政策：对主体的行为进行限制，主要分为两个

- targeted

  主机应用限制少，网络应用限制多（默认采用此政策）

- strict

  完整地SELinux限制，对各方面限制都和严格

安全性本文：类似于ACL，只有主体与安全性本文匹配成功后才能进行读写，由三个字段组成

- 身份标识

  类似于系统用户的区分，特定用户具有权限，常见有三种（root、system_u、user_u）

- 角色

  标识当前文件的角色，分为两种（object_r - 标识文档、system_r - 标识程序）

- 类型

  对不同的文件有不同的定义，在文档资源中称为type，在主体程序中称为domain

实际使用中更多关注与类型字段，一个程序是否对文件具有读写权限，由domain与type直接控制，属于某一domain的程序对某一type的文件具有读写权限，则当程序属于此domain时，才能对此type的文件进行读写



实践：

- 查看SELinux模式

  ```
  getenforce	（常用）
  sestatus
  ```

- SELinux的启动与关闭

  ```
  SELinux有三种模式：
  	enforcing	强制模式（严格限制domain/type）
  	permissive	宽容模式（仅具有警告信息，不实行规则校验）
  	disabled	关闭（SELinux不运行）
  在enforcing或permissive与disable进行状态切换时，需要重启
  在enforcing与permissive之间进行状态切换时，不需要重启
  
  setenforce [0|1]	更改SELinux状态为enf(1)或per(0)
  要想关闭SELinux，则必须修改配置文件/etc/selinux/config
  将其中的SELINUX字段改为disabled
  
  ```

- 重设SELinux安全性本文

  ```
  chcon
  
  参数：
  	-R	连同目录下的所有子目录一同修改
  	-t	后面接安全性本文的type字段
  	-u	后面接身份识别
  	-r	后面接角色
  	--reference	拿一个文件做范例来修改后续文件
  	
  	
  	
  restorecon
  
  参数：
  	-R	连同子目录一同修改
  	-V	将过程显示到屏幕
  ```

- 记录SELinux错误以便于修改维护

  ```
  setroubleshoot		(写入/var/log/messages)
  
  auditd				(写入/var/log/audit/audit.log)
  ```

- 政策查看

  ```
  SELinux预设的政策是targeted政策，想要查看该政策下的相关规则可通过一下命令进行：
  
  seinfo
  
  参数：
  	-A	列出SELinux的状态、规则布尔值、身份识别、角色、类别等所有信息
  	-t	列出SELinux的所有类别（type）种类
  	-r	列出SELinux的所有角色（role）种类
  	-u	列出SELinux的所有身份识别（user）种类
  	-b	列出SELinux在此政策下的统计状态
  	
  sesearch		（查看详细规则）
  
  参数：
  	-a	列出该类别或布尔值的所有相关信息
  	-t	后面还要接类别
  	-b	后面要接布尔值的规则
  例如：
  	sesearch -a -t httpd_sys_content_t
  	
  getsebool		（获取布尔值条目）
  
  参数：
  	-a	列出目前系统上面的所有布尔值条目
  	
  setsebool		（设置布尔值条目是否生效）
  
  参数：
  	-P	直接将设定值写入配置文件
  ```

- 自定义安全性本文内容

  ```
  semanage
  
  参数：
  	fcontext	主要用在安全性本文
  	-a			增加
  	-l			查询
  	-m			修改
  	-d			删除
  例如：
  	semanage fcontext -l
  	semanage fcontext -a -t public_content_t "/srv/sambsa(/.*)?"
  ```

  





























