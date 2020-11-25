### 解决Ubuntu(20.04)开机、关机、重启慢，有光标闪烁问题



#### 1. 问题描述

开关机，或者重启时，等待时间很长，大约1分30秒，且有光标闪烁。



#### 2. 问题解析

- 等待时间长，可能时由于开关机时后台要打开或关闭某些程序，这些程序花费的时间是有系统设定的默认时间的，大约90秒，只有到了90秒系统才能打开或是关闭。
- 光标闪烁就是后台一系列活动的简化，它表示后台有一系列活动在进行，只是我们看不到。也因此让我们觉得它像是卡住了。

如果我们在终端输入：

```
sudo gedit /etc/default/grub
```

在打开的文件中，找到以下内容：

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
```

将其改为：

```
GRUB_CMDLINE_LINUX_DEFAULT=""
```

并保存，重启。我们就会看到开关机时后台的动作，如果我们不想看到，再把删掉的内容填上就行。

再次开关机时，我们会发现后台有这样一个动作：

```
A stop job is running for Snappy daemon (1min 16s / 1min 30s)
```

正是它导致了关机慢。

#### 3. 问题解决

打开终端，输入：

```
sudo su # 成为root用户
vim /etc/systemd/system.conf

#ubuntu默认没有开启root权限，我们需要以下操作：
sudo passwd root
#然后设置密码就行
```

修改以下内容：

```
#DefaultTimeoutStartSec=90s
#DefaultTimeoutStopSec=90s
```

改为：

```
DefaultTimeoutStartSec=3s	# 将#去掉，90改为3
DefaultTimeoutStopSec=3s
```

然后，加载修改的配置：

```
systemctl daemon-reload
```



如果对vim不熟悉的话，可以在文件夹中进入  `/etc/systemd` 文件夹, 在文件夹中打开终端，输入：

```
sudo chmod 777 system.conf		# 修改system.conf只读文件为读写文件
```

再在文件夹中打开`system.conf`文件进行编辑，编辑完保存。

再打开终端，将文件设置为只读文件：

```
sudo chmod 644 system.conf		# 修改system.conf读写文件为只读文件
```

最后，仍需要加载修改的配置：

```
systemctl daemon-reload
```



通过以上操作之后，开关机慢的问题应该就解决了。