# 同一台机器多个SSH Key配置

## 创建SSH Key

输入指令

```
ssh-keygen -t rsa -b 4096 -C "email@example.com"
```

之后出现如下提示

> Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]

如果不自定义路径，直接回车；自定义路径的情况下，要补充`.ssh`路径，或使用绝对路径

> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]

如果不想在每次git时都输入passphrase，那么直接回车使用空的passphrase

## SSH Key配置

如果要使用多个SSH Key，那么需要进行自定义配置

先在`.ssh`目录下创建`config`文件，按照格式填入内容

```
Host git-xxx
  HostName github.com
  User git
  IdentityFile /c/Users/xxx/.ssh/id_rsa
  IdentitiesOnly yes
```

`git-xxx`后面用于替换`HostName`；`IdentityFile`用于指定rsa文件路径

之后在`git clone`时，将`git@HostName:xxx/Docs.git`中的
HostName替换成配置文件中的`git-xxx`，即可指定SSK Key
