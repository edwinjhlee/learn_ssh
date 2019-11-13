# key与agent

相互相承
key提供了更安全的身份验证，agent来降低成本，提高可用性，甚至是安全性。

# ssh-key

ssh-key是一个远远要比password安全的登陆方案，我们的服务器管理策略里面日后将完全禁止password登陆

1. 生成key
    1. `ssh-keygen`
    2. `ssh-keygen -t rsa -b 2048`
        * `-t` 可选 `rsa, dsa, ecdsa, ed25519`
        * `-b` 根据不同算法含义不一样，请参考man
    3. 安全考虑：
        1. `https://security.stackexchange.com/questions/23383/ssh-key-type-rsa-dsa-ecdsa-are-there-easy-answers-for-which-to-choose-when`
        2. 默认rsa/2048足够安全
        3. ED25519可能长期是一个更安全的选择，但是支持也是个问题，在我们的case，不存在问题
    4. 生成过程要求：
        1. 增加密码，尽量strong
        2. 采用ssh-agent方式转发，尽量不要将ssh-key转移到不安全的临时环境
2. 连接时使用key
    1. 参数i：`ssh -i <file path for ssh private key> [other arguements]`
    2. 使用ssh-agent进行私钥代理
3. 使用ssh-agent

# `ssh-agent` 与 AgentForward

使用ssh-agent进行私钥代理

1. 优势：
    1. 避免频繁输入私钥密码
    2. 跨服务器的安全转发，这个特性称之为 `ForwardAgent`
2. 使用：
    1. 使用`ssh-keygen`生成key或者获取私钥
    2. 远端服务器配置: `ssh-copy-id -i <key的名称> <服务器>`
    3. 本地计算机：`ssh-add <file path for ssh private key>`
3. `ssh-agent` 管理
    1. 添加key：`ssh-add <file path for ssh private key>`
    2. 罗列key：`ssh-add -l`
    3. 删除key：`ssh-add -d <file path for ssh private key>`，删除所有 `ssh-add -D`
4. `ssh-agent-forward`
    1. 参数A：`ssh -A <Other argument>`
    2. 远程服务器输入`ssh-add -l`能看到引用的key
    3. 注意，要在安全的远端服务器上使用！一旦远端服务器被陷，默认agent的key所能访问的服务器都能被攻击者访问。
    4. 注意在windows的mingw，不会自动启动，需要手动触发`ssh-agent bash`
