# 安装说明
 ## 一 安装需求
 ### 1. java8及以上的版本
 ### 2. github 帐号一个 
 github上需要有一个帐号，同时将自己本机的id_rsa.pub文件中的key放到github上
 ### 3. coding.net 帐号一个
 项目源代码保管在coding.net上，如果你要加入进来，请准备一个帐号。
 ### 4. mysql或者mariaDB
 
 启动项目前需要导入一个数据库文件。
 为了防止以后的真实数据导入失败，最好能够自己手动的在命令行进行导入数据。
 如果不会命令，请自学。
 
 导入：
  ```
  mysql -u root -p password;
  use databaseName;
  source file.sql;
  ```
  ### 5.  git 基本命令
   ``` 
  git clone XXX
  git pull origin(仓库名字) master(分支名字)
  git add XXXX  (将当前目录中所有修改加入本地仓库进行追踪)
  git commit -m XXX
  git push origin master
  git merge （合并分支） 
  git submodule 查看有多少个附加模块
  git submodule init 初始化附加模块
  git submodule update 更新附加模块
  
  ```
  基本命令就这么几个，如果这几个命令不熟悉也没关系。去看一下相关的文档。网上关于git的操作文档很多。
  
  如果有不懂的地方，可以向团队其他成员提出你的疑问。
  
  欢迎你的提问，咱们共同进步！
  
  ## 二 安装步骤
  ### 1 拉（pull）代码
  * 确定你已经有相应项目以及各个组件的访问权限后，将源代码clone到本地。
  * clone完后，进入主目录app-XXXXX .执行 git submodule ,查看所有的附加组件
  * 你会看到类似 pylon，addon-gateway, addon-totoro ，这样的文件夹。 删除这些文件夹。
  * 删除完这些组件后，执行 git submodule init 进行初始化
  * 初始化完成，执行  git submodule update 更新代码
  ### 2 修改配置
   在pylon 的同级目录中，你会看到app-XXXX文件夹, 然后找到以下文件，修改相应的配置
   ```
   app-XXXXX/src/main/resources/application.yaml 
 
        url: jdbc:mysql://127.0.0.1:3306/yummy（数据库的名字）?useUnicode=true&characterEncoding=utf8
        username: root （用户名）
        password: 123456（密码）
        
        
   ```
  ### 3 启动，下载。 
   * 去到项目根目录下执行
   ```
  ./startup.sh
  ```
  * 当出现要求你下载gradle的包的时候，取消下载。
  * 如果你是linux，macOS，以下配置对你会有很大帮助。如果是windows，你可以试试。
   在家目录下，找到隐藏文件夹".gradle"
   进入文件夹,创建一个文件 init.gradle
   将以下代码写入：
   ```
   allprojects{
       repositories {
           def REPOSITORY_URL = 'http://maven.aliyun.com/nexus/content/groups/public'
           all { ArtifactRepository repo ->
               if(repo instanceof MavenArtifactRepository){
                   def url = repo.url.toString()
                   if (url.startsWith('https://repo1.maven.org/maven2') || url.startsWith('https://jcenter.bintray.com/')) {
                       project.logger.lifecycle "Repository ${repo.url} replaced by $REPOSITORY_URL."
                       remove repo
                   }
               }
           }
           maven {
               url REPOSITORY_URL
           }
       }
   }
   
```
  * 重新进入项目根目录。执行.startup.sh 。 
  * 如果你发现下载gradle的速度特别慢，想一想这是中国，就不会觉得奇怪了。当然如果你会科学上网的话，自己想办法下载的快一点吧。
 
 ## 注意事项
 1 发现类似 NOT FOUND 类似的报错，别着急。 应该是你的submodule 没有处理好。
 git submodule update 试试
 2 每次提交代码之前，pull一下线上的代码，进行合并。
 3 每次启动的时候，如果发现启动不了，报错，记得更新最新的代码。
   不仅仅是 git pull origin XXX(分支名字)
   同时还要进行 git submodule update (更新附加组件源代码)
 
  
  
  
 