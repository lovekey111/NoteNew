# Maven使用总结

## 一、Maven的主要作用

* Maven 翻译为"专家"、"内行"，是 Apache 下的一个纯 Java 开发的开源项目
* Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤
*  Maven 是一个项目管理工具，可以对 Java 项目进行构建、依赖管理

## 二、Maven仓库

 Maven 仓库是项目中依赖的第三方库，这个库所在的位置叫做仓库。 

* 依赖搜索顺序：本地仓库 -> 中央仓库 -> 远程仓库

setting.xml文件配置
* Level配置
  * User Level：当前用户共享配置，一般在${user.home}/.m2/settings.xml下
  * Global Level：同一计算机上所用用户共享配置，一般在${maven.conf}/settings.xml下
  * 优先级：User Level > Global Level
  
 * localRepository：本地仓库位置
    * interactiveMode：是否需要和用户交互以获得输入，默认为true
    * offline：如果构建系统要在离线模式下工作，设置为true，默认为false
    
 * pluginGroups：添加多个pluginGroup子元素列表，内容包含一个groupId， 当使用插件并且命令行中没有提供groupId时，会搜索列表。该列表会自动包含 org.apache.maven.plugins 和 org.codehaus.mojo 

 * proxies：设置代理服务器 

    ```xml
        <proxies>
                <proxy>
                    <!-- 代理标识 -->
                    <id>testProxy</id>
                    <!-- 是否激活，默认是true -->
                    <active>true</active>
                    <!-- 代理协议 -->
                    <protocol>http</protocol>
                    <!-- 用户名 -->
                    <username>lky</username>
                    <!-- 密码 -->
                    <password>123</password>
                    <!-- 端口 -->
                    <port>8099</port>
                    <!-- 主机 -->
                    <host>proxy.xxx.com</host>
                    <!-- 不需要代理的主机 -->
				   <nonProxyHosts>*.xx1.com|*.xx2.com</nonProxyHosts>
                </proxy>
        </proxies>
    ```

    * servers：私服发布的用户名密码
    
      ```xml
          <servers>
                  <server>
                      <!-- 服务标识 -->
                      <id>server1</id>
                      <!-- 用户名 -->
                      <username>wang</username>
                      <!-- 密码 -->
                      <password>123</password>
                  </server>
                  <server>
                      <id>server2</id>
                      <privateKey>lky</username>
                      <passphrase>456</passphrase>
                  </server>
          </servers>
      ```
    
    * mirrors： 用于定义一系列的远程仓库的镜像
    
      ```xml
         <mirrors>
          <!-- 阿里云仓库 -->
          <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>https://maven.aliyun.com/nexus/content/repositories/central/</url>
          </mirror>
      
          <!-- 中央仓库1 -->
          <mirror>
            <id>repo1</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo1.maven.org/maven2/</url>
          </mirror>
      
          <!-- 中央仓库2 -->
          <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo2.maven.org/maven2/</url>
          </mirror>
        </mirrors>
      ```
    
    * profiles：settings.xml中的profile是pom.xml中的profile的简洁形式。它包含了激活(activation)，仓库(repositories)，插件仓库(pluginRepositories)和属性(properties)元素
    
      *  activation：当满足所有指定的条件时，就会激活该profile
      *  repositories：当profile被激活时定义在profile里的远程仓库将被使用
      *  pluginRepositories： 插件仓库， 结构与repositories类似
      *  properties：定义参数，可以在pom文件中通过${属性名}引用

### 1、本地仓库配置

* 默认位置： ${user.home}/.m2/repository
* 修改默认位置：在maven目录的conf/setting.xml文件中<localRepository> 节点修改位置

### 2、中央仓库配置

* 仓库由 Maven 社区管理，不需要进行配置，需要网络才能访问
* 中央仓库访问地址： http://search.maven.org/#browse

### 3、远程仓库配置

## 三、Pom.xml文件配置

* 常见Pom节点含义

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/maven-v4_0_0.xsd">
    <!--声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，但它仍然是必不可少的，这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。 -->
    <modelVersion>4.0.0</modelVersion>
    
     <!--项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 如com.mycompany.app生成的相对路径为：/com/mycompany/app -->
    <groupId>asia.banseon</groupId>
    <!-- 构件的标识符，它和group ID一起唯一标识一个构件。不能有两个不同的项目拥有同样的artifact ID和groupID；在某个特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven为项目产生的构件包括：JARs，源 码，二进制发布和WARs等。 -->
    <artifactId>banseon-maven2</artifactId>
    <!--项目的名称, Maven产生的文档用 -->
    <name>banseon-maven</name>
    <!--项目主页的URL, Maven产生的文档用 -->
    <url>http://www.baidu.com/banseon</url>
    <!-- 项目的详细描述, Maven 产生的文档用。 当这个元素能够用HTML格式描述时（例如，CDATA中的文本会被解析器忽略，就可以包含HTML标签），不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，你应该修改你自己的索引页文件，而不是调整这里的文档。 -->
    <description>A maven project to study maven.</description>
    
    <!-- 使用${标签名}来使用在<properties>标签里所定义的<标签>值</标签>可以作为全局变量来用,改变该全局变量的值,所有引用该全局变量的值也随着改变,方便维护 -->
     <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    
    <!-- 继承自该项目的所有子项目的默认依赖信息。这部分的依赖信息不会被立即解析,而是当子项目声明一个依赖（必须描述group ID和 artifact ID信息），如果group ID和artifact ID以外的一些信息没有描述，则通过group ID和artifact ID 匹配到这里的依赖，并使用这里的依赖信息。 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                ......
            </dependency>
        </dependencies>
    </dependencyManagement>
    
     <!--发现依赖和扩展的远程仓库列表。 -->
    <repositories>
        <!--包含需要连接到远程仓库的信息 -->
        <repository>
        </repository>
    </repositories>
    
    <!--发现插件的远程仓库列表，这些插件用于构建和报表 -->
    <pluginRepositories>
        <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素 -->
        <pluginRepository>
            ......
        </pluginRepository>
    </pluginRepositories>
```



## 四、Maven构建生命周期

| 阶段           | 处理     | 描述                                                     |
| -------------- | -------- | -------------------------------------------------------- |
| 验证  validate | 验证项目 | 验证项目是否正确且所有必须信息是可用的                   |
| 编译  compile  | 执行编译 | 源代码编译在此阶段完成                                   |
| 测试  Test     | 测试     | 使用适当的单元测试框架（例如JUnit）运行测试。            |
| 包装  package  | 打包     | 创建JAR/WAR包如在 pom.xml 中定义提及的包                 |
| 检查  verify   | 检查     | 对集成测试的结果进行检查，以保证质量达标                 |
| 安装 install   | 安装     | 安装打包的项目到本地仓库，以供其他项目使用               |
| 部署 deploy    | 部署     | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程 |

## 五、Snapshot 版本与 Release 版本

* Snapshot 版本代表不稳定、尚处于**开发中**的版本。Release 版本则代表**稳定**的版本

* SNAPSHOT使用场景：协同开发时，如果 A 依赖构件 B，由于 B 会更新，B 应该使用 SNAPSHOT 来标识自己