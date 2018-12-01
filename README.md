# maven
maven
maven

打包方式：jar war pom

maven功能包含项目构建功能、依赖管理、项目信息管理

使用maven的原因（好处）：

	解决jar冲突和jar依赖、制定jar依赖范围、不需要在每个项目中保存，只需要放在仓库即可
  
配置环境变量

	MAVEN_HOME；path
  
构建配置文件：

	项目级pom文件			pom.xml 当前项目下使用
	用户级user settings		setting.xml 当前系统的当前用户使用
	全局级global settings	setting.xml 当前系统下的所有用户使用
	构建配置文件激活：
		在具体项目下cmd 
			命令行参数：
				mvn test -P具体profile id 例如mvn test -Pnexus 指定使用nexus.xml文件，使用文件中指定的私服网址下载jar包

依赖

	指定依赖的使用范围 <scope></scope>
		compile（默认）
		test（测试）
		provide（设置成后，在打包时不会打包该jar包，在tomcat中有，避免了冲突）
		runtime（运行）
		system（外部依赖，自己开发的jar包）
		import（导入外部pom.xml）
	直接以来和间接依赖，若发生冲突以直接依赖为准
		间接依赖被覆盖，使用直接依赖omitted for conflict with x.x 

	如果本地仓库中有jar包，则不需要下载，直接引用本地的
		本地仓库->私服仓库->远程仓库

插件

构建生命周期：

	clean:pre-clean,clean,post-clean
	dafault(build)
		build周期的各个阶段
			验证validate
			编译compile
			测试test
			打包package
			检查verify
			安装install 将打包得到的文件复制到仓库的指定位置
			部署deploy 生成war包复制到servlet容器指定目录下
				可在命令行运行参数：
				mvn clean 将项目中的.class文件删除,对项目重新编译，将target文件夹下的文件全部删除，
				mvn package 将项目打成jar包
				mvn compile 编译类文件
				mvn install 包含mvn compile，mvn package，然后上传到本地仓库
				mvn deploy 包含mvn install,然后上传到私服
				可在项目右击runas->maven build...Goals写参数
	site(不常用)
	生命周期和阶段有关联，阶段和插件有关联，插件和目标有关联。
	
	命令行参数（使用maven插件）：mvn help:describe -Dplugin=help \ clean \ compiler \ archetype...
	查看help插件的描述	mvn hepl:describe -Dplugin=help
  
继承和聚合

		继承：目的：消除重复的配置
			父工程:打包模式为pom,依赖管理，插件管理，不知道自己的子工程
			子工程：知道自己的父工程
      
		聚合：目的：为了方便快速构建项目
			聚合模块：打包方式为pom,依赖管理，插件管理，知道自己的被聚合模块
			被聚合模块：不知道自己的聚合模块
      
命令行参数

使用cmd创建maven项目

	1.mvn archetype:generate
	Define value for property 'groupId': 			cn.com.taiji
	Define value for property 'artifactId': 			Test03
	Define value for property 'version' 1.0-SNAPSHOT: : 确认
	Define value for property 'package' cn.com.taiji: :	确认

	2.mvn archetype:generate -DgroupId=cn.com.taiji -DartifactId=Test02 -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
分析当前项目的依赖：mvn dependency:resolve  打印出已解决依赖的列表 
					mvn -Pnexus dependency:resolve	将当前依赖通过私服下载打印
