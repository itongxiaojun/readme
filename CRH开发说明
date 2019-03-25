CRH开发的核心是bigtop开发和ambari开发，目前ambari已集成到bigtop中，对ambari的修改以patch的方式集成到bigtop中。

这里分为两部分说明，分别是ambari开发和bigtop开发，每个部分会说明开发时涉及到的代码模块。

[TOC]

## bigtop开发

bigtop开发涉及到的模块如下：

| 模块            | 说明               |
| --------------- | ------------------ |
| bigtop.bom      | 控制组件及版本     |
| packages.gradle | 控制全局参数传递   |
| bigtop-packages | 设计软件包安装细节 |
| build.sh        | 自定义的编译入口   |

下面分别对这几部分进行说明。

### bigtop.bom

这个文件控制组件(例如hadoop)的下载地址以及版本信息。

需要注意的全局参数如下：

| 参数                | 说明                                                         | 示例                                          |
| ------------------- | ------------------------------------------------------------ | --------------------------------------------- |
| crh_version_with_bn | crh安装路径有一部分是版本号组成的，使用这个参数指定，格式参考示例或者代码。 | 6.1.2.6-1457                                  |
| crh_version_as_name | crh软件包名包含版本号，使用这个参数指定，格式参考示例或者代码。 | -6-1-2-6-1457                                 |
| crh_tag             | crh安装路径有一部分是stack名组成的，这里是crh，目前还未完善，不建议修改。 | crh                                           |
| APACHE_MIRROR       | 软件源码包下载路径前缀，和每个组件中指定的路径拼接成完成的源码包下载路径。 | http://compile.redoop.org/redoop/bigtop       |
| APACHE_ARCHIVE      | 软件源码包存档路径前缀，没有检验效果。                       | http://compile.redoop.org/redoop/archive/dist |

需要注意的组件参数如下，建议参考代码理解。

#### name

组件名。

#### version

指定一些版本信息。

base指定软件的版本号；

pkg指定安装包的版本号，一般和base一样；

release使用默认值即可，还没有深入研究。

#### tarball

指定源码包名的一些信息。

destination指定编译前的源码包名；

source指定要下载的源码包名

#### url

指定软件源码包的url信息。

download_path指定软件源码包的部分下载路径；

site拼接软件源码包的下载路径；

archive指定软件源码包的存档路径。

### packages.gradle

主要是传递一些全局参数，也有其他的功能。

这个一般不用修改，如果研究透了，可以尝试按需修改。

### bigtop-packages

这个目录的开发主要包含三块，分别是`common`、`deb`和`rpm`，都在src目录下。

#### common

一些通用的信息文件和脚本文件。

主要文件如下：

| 文件               | 说明                                                  |
| ------------------ | ----------------------------------------------------- |
| do-component-build | 源码的编译逻辑，比如一些mvn相关的命令                 |
| install_${name}.sh | 例如，install_hadoop.sh。主要用来设计软件包的安装细节 |

#### deb

有关deb格式软件包的封包逻辑在这里。

#### rpm

有关rpm格式软件包的封包逻辑在这里。这个和common是目前开发的重点。

主要文件是spec文件，这个文件一般在SPECS目录下，比如 hadoop.spec。

这个是标准的spec格式文件，是rpm包的核心文件，网上资料很多，可以自行学习，配合代码理解。

### build.sh

这个文件是我自定义的一个编译入口，是对bigtop编译命令的封装。



## ambari开发

ambari开发主要涉及三部分，分别是`web显示`、`CRH`和`service`。

主要代码都已封装进bigtop/bigtop-packages/common/ambari。在这里面还有一个licenseutils，提供ambari的license支持。

下面分别介绍。

### web显示

这部分主要是ambari登录页和样式的修改，请看custom-style目录。

### CRH

这部分是核心，是CRH在ambari中的核心代码，对应目录是stacks/CRH，其中的6.0这个目录是基本目录，用来和ambari交互，提供其他版本继承。是参照HDP开发的。

### service

这是ambari的一个概念，是一个可插拔的特性，每个组件是一个service，只要放到指定位置，ambari server就可以识别到，可以按需自定义service。开发规范参考ambari官方文档。





