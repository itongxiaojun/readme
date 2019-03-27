mpack是ambari的一个特性，其本身是一些service的集合，ambari通过安装mpack包，可以将mpack中的service添加到ambari中，然后就可以使用这些service安装相应的服务。

在CRH中，一个mpack是一类service的集合，并已集成进bigtop，其独立于ambari，并且以系统安装包的形式安装。

这里会对集成进bigtop的mpack机制做一个说明，并用`crh-DW-mpack`进行说明来帮助理解。

## mpack机制

集成进bigtop中mpack机制分为两部分，一部分是mpack源码，另一部分是生成系统安装包的描述文件。

这些代码都在目录`bigtop-packages/src/`下。

### mpack源码

mpack源码主要在`common/ambari-mpacks`目录下，主要内容描述如下：

| 目录             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| management-packs | mpack的源码，其中的每个目录是一个mpack包，`pom.xml`是总的描述文件。每个mpack目录内部的`pom.xml`是单独的描述文件。crh-select的源码 |
| selector         | `crh-select`的源码，用来描述软链，一般不用动。               |

### spec描述

目前bigtop中的ambari只支持rpm包，deb包需要再对deb部分进行开发。

因此ambari的mpack目前也只支持rpm包，deb包需要再对deb部分进行开发。

rpm包的描述文件是`rpm/ambari-mpacks/SPECS/ambari-mpacks.spec`。

至于spec文件，网上有很多资料，可以自己学习理解。

下面我会以`crh-DW-mpack`为例进行说明。



## crh-DW-mpack

上面说了，在CRH中，一个mpack是一类service的集合，而`crh-DW-mapck`就是和数据仓库相关的service的集合。

开发一个`crh-DW-mpack`，按照mpack机制，分为两部分开发。

### crh-DW-mpack源码

进入目录`bigtop-packages/src/common/ambari-mpacks/management-packs`。

编辑`pom.xml`文件，添加`crh-DW-mpack`信息，请参考源码，共有两处。

新建文件夹`crh-DW-mpack`，并进入。

新建`pom.xml`文件，这个文件的内容可以从其他mpack复制过来，然后改动如下两处即可：

```
<artifactId>crh-DW-mpack</artifactId>
<name>CRH Data Warehouse Management Pack</name>
```

这两个标签用来指明mpack的名字和描述。

创建`src/main`目录，并进入。

创建`assemblies/crh-DW-mpack.xml`目录和文件，这个文件直接从其他mpack复制过来即可，不需要做任何修改。

退`src/main`目录，创建`resources`目录，这个目录下，`stacks/CRH`这个目录用来存放service，按照service规范开发即可。`mpack.json`这个文件也是mpack的描述文件，其内容可以从其他mpack复制过来，然后改动如下两处即可：

```
"name" : "crh-DW-mpack",
"description" : "CRH Data Warehouse Management Pack",
```

至此，`crh-DW-mpack`的源码完成。

下面看rpm包的描述文件，也就是spec文件。

### spec描述

spec是rpm包的描述文件，mpack的rpm包的描述文件是`bigtop-packages/src/rpm/ambari-mpacks/SPECS/ambari-mpacks.spec`，所有的mpack的描述信息都在这个文件内。

下面添加`crh-DW-mpack`的描述信息，主要内容如下：

```
......
%__cp -ra crh-DW-mpack/target/crh-DW-mpack-*.tar.gz ${RPM_BUILD_ROOT}/var/lib/ambari-mpacks/
......
%package crh-DW
Summary: CRH Data Warehouse Mpack
Group: Development/Libraries
Requires: ambari-server
AutoProv: no
AutoReqProv: no
%description crh-DW
Redoop Ambari CRH Data Warehouse Mpack
......
%service_macro crh-DW
```

具体内容请参考源码。



到这里，`crh-DW-mpack`基础开发完成，接下来就可以进行编译和调试了。