这里讨论CRH7的开发思路，以供参考。

**在开发前，请将现有代码备份或者release，或者可以新建一个分支进行开发。**

CRH的开发主要是ambari以及bigtop的开发，下面对两部分两部分分别讨论。

[TOC]

## ambari

开发一个新的版本实际上是在CRH这个stack里添加一个新的版本。

由于ambari已集成到bigtop中，所以需要在bigtop中开发ambari。

进入bigtop目录：`bigtop-packages/src/common/ambari/stacks/CRH`。

复制目录6.1，重命名为7.0。

进入目录7.0，按照下面的描述对相应目录文件进行修改。

### repos

这个目录下只有一个文件，是用来提供软件源信息的，编辑文件，将所有的`6.1`替换成`7.0`。如下：

```
<reposinfo>
  <os family="redhat-ppc7">
    <repo>
      <baseurl>http://baseurl</baseurl>
      <repoid>CRH-7.0</repoid>
      <reponame>CRH</reponame>
      <unique>true</unique>
    </repo>
    <repo>
      <baseurl>http://baseurl</baseurl>
      <repoid>CRH-UTILS-1.1.0.21</repoid>
      <reponame>CRH-UTILS</reponame>
      <unique>false</unique>
    </repo>
  </os>
  <os family="redhat7">
    <repo>
      <baseurl>http://baseurl</baseurl>
      <repoid>CRH-7.0</repoid>
      <reponame>CRH</reponame>
      <unique>true</unique>
    </repo>
    <repo>
      <baseurl>http://baseurl</baseurl>
      <repoid>CRH-UTILS-1.1.0.21</repoid>
      <reponame>CRH-UTILS</reponame>
      <unique>false</unique>
    </repo>
  </os>
</reposinfo>
```

### services

这里面有一个文件`stack_advisor.py`，用来继承上一个版本的一些设置，目前不需要做太大改动，编辑这个文件，如下：

```
import math
from resource_management.core.logger import Logger
import json
import re
from resource_management.libraries.functions import format
class CRH70StackAdvisor(CRH61StackAdvisor):
  
  pass
```

其他目录都是service，不用动。

### metainfo.xml

这个文件描述stack的继承关系，这里继承6.1。内容如下：

```
<metainfo>
  <versions>
    <active>true</active>
  </versions>
  <extends>6.1</extends>
</metainfo>
```

至此，可以将7.0这个目录放在测试环境中进行调试了，然后根据出现的问题进行针对性的修改。



## bigtop

bigtop这部分升级CRH版本，只涉及一个文件，`bigtop.bom`，这个文件在bigtop工程的顶级目录下。

如果要修改成7.0版本，编辑文件，修改如下：

```
crh_version_with_bn = "7.0.2.6-1457"
crh_version_as_name = "-7-0-2-6-1457"
```

两个参数的版本内容需要保持一致，格式必须按照这样写，下面是参数解释：

| 参数                | 说明                   | 参数值说明                                                |
| ------------------- | ---------------------- | --------------------------------------------------------- |
| crh_version_with_bn | 控制软件的安装目录     | 7.0代表CRH版本。2.6代表对应的HDP版本。1457是是自定义的。  |
| crh_version_as_name | 控制软件包名的版本字段 | 参数内容是参照`crh_version_with_bn`的，只是需要注意格式。 |

完成之后，就可以进行软件编译了。



当两部分都开发完成后，就可以在测试环境进行调试了，然后根据出现的问题进行针对性的修改。