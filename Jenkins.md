# Jenkins相关问题记录

Windows系统

## 执行系统自带SVN失败

在目标系统新建一个bat，内容是svn操作，直接在系统双击执行正常，在Jenkins执行出错

这种情况可能是权限问题。如果是用服务启动的，到服务中右键Jenkins服务，
**属性**->**登录**->**此账户**填写帐户信息；如果是通过任务计划启动的，
右键Jenkins任务，**属性**->**常规**->**安全选项**修改运行时用户。
之后重启Jenkins即可

---

## 运行带GUI的应用失败

起初怀疑有2个可能，一是无法启动带GUI的应用；
另一个是Java内存限制，因为在**ProcessExplorer**中看到大概在500M的时候**Game_x64h.exe**就退出了。

之后再次搜索，最终发现问题所在，导致原因可查看此
[博客](https://blog.csdn.net/anlegor/article/details/24329237)最后的**补充**内容 （额外附上[Windows官方文档](https://learn.microsoft.com/en-us/previous-versions/windows/hardware/design/dn653293(v=vs.85)?redirectedfrom=MSDN)）

解决方案是把Jenkins从服务中去掉，改成cmd启动。从服务中去掉可执行`jenkins_slave.exe uninstall`，
之后再按照[官方文档](https://wiki.jenkins.io/display/JENKINS/Launch+Java+Web+Start+slave+agent+via+Windows+Scheduler)将Jenkins加入计划任务中即可
