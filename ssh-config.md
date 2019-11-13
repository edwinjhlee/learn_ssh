# 简化登陆命令：ssh-config

**sshd-config** located at place like `/etc/ssh/sshd.config`. 针对ssh daemon process

**ssh-config** 管理的是ssh客户端

typical ssh config file

```log
Host *.<root domain url>
  ControlMaster auto
  ControlPath <HOME>/.ssh/sockets/%r@%h-%p
  ControlPersist  0
Host dev*
  ForwardAgent yes
  User ubuntu
Host dev1
  SetEnv LHOSTNAME=dev1
  HostName dev1.aws.<root domain url>
Host dev2
  SetEnv LHOSTNAME=dev2
  HostName dev2.aws.<root domain url>
```

1. 使用ControlMaster，来保证数据的安全
    1. 长链接，避免再次断开连接登陆
    2. ControlMaster失灵情况
        1. `ssh -o dev1`查看进程
        2. kill该进程
    3. ControlPersist: 保持连接的时间，0表示`forever`，正数以秒为单位
2. SetEnv：登陆时设定环境变量，较新的sshd才支持
3. 其余参数：ForwardAgent，HostName

## 团队模式

下载最新的config，放在`$HOME/.ssh/dy.config"`位置上

将该行放在`$HOME/.bashrc`内

```bash
alias ssh.dy="ssh -F $HOME/.ssh/dy.config"
```

或者

```bash
ssh.dy(){ ssh -F $HOME/.ssh/dy.config $@; }
```
