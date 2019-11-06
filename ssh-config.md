# 简化登陆命令：ssh-config

**sshd-config** located at place like `/etc/ssh/sshd.config`. 针对ssh daemon process

**ssh-config** 管理的是ssh客户端

typical ssh config file

```log
Host *.<root domain url>
  ControlMaster auto
  ControlPath <HOME>/.ssh/sockets/%r@%h-%p
  ControlPersist  600
Host dev*
  ForwardAgent yes
  User ubuntu
Host dev1
  SetEnv LHOSTNAME=dev1
  HostName dev1.aws.<root domain url>
Host dev2
  SetEnv LHOSTNAME=dev2
  HostName dev2.aws.<root domain url>
Host dev3
  SetEnv LHOSTNAME=dev3
  HostName dev3.aws.<root domain url>
```

1. 使用ControlMaster，来保证数据的安全
2. ControlMaster失灵情况
    1. `ssh -o dev1`查看进程
    2. kill该进程
