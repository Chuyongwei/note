

### 命令

|命令|操作|
|--|--|
| mvn compile | 编译maven工程 |
| mvn install | 打包并安装到本地仓库 |
| mvn package | 编译并打包工程 |
| mvn deploy | 打包并安装到远程仓库 |
| mvn clean | 清除target目录 |

### 元素

`groupId`：表示项目所属的组

`artifactId`：项目唯一标识

`packageing`：项目类型

`version`：项目版本号

`modelVersion`：代表pom文件的maven版本

`dependencies`：此元素下包含多个depenency，声明依赖

`dependency`：声明项目依赖

`scope`:此类库与项目的关系

`build`：辅助项目的构建