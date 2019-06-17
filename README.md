# linux_command_record

**期间，有小伙伴想实现linux命令的实时记录功能，花了1小时时间，查阅了些资料，对Linux做少许改动，便可实现对Linux的命令实时记录，并配置syslog可以做到对操作命令的集中收集。

下面时配置步骤：

1、在/etc/profile下增加如下代码

```
# cat << EOF >>/etc/profile

export HISTTIMEFORMAT="`echo ${SSH_CONNECTION}`|#|`whoami`|#|%F %T|#|"

export PROMPT_COMMAND="history -a; history -c; history -r;"'echo $(history 1) >>/var/log/command.log'

EOF
```

2、创建/var/log/command.log文件，并将文件权限设置为662

```
# chmod 662 /var/log/command.log
```

3、开启syslog，实时将command内容发送到远程服务器。
