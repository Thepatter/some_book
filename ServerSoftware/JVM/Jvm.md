### JVM

#### JDK

##### 安装

1. 官网下载

2. 解压到指定路径

3. 配置系统变量指定

   *~/.bash_profile*

   ```shell
   export JAVA_HOME=/path/to/jdk
   export PATH=jdk/bin:$PATH
   ```

4. 获取源码

   JDK 附带了一个 $jdk_home/lib/src.zip 的源代码

   ```shell
   mkdir javasrc
   cd javasrc
   jar xvf jdk_home/lib/src.zip
   ```

5. 检验安装

   ```
   javac --version
   ```

##### 命令行使用

1. 使用 javac 命令编译源文件为字节码 class 文件

2. 使用 Java 运行 class 文件，运行时类包名要与路径名匹配，否则会找不到主类，

   ```shell
   # 类名 jdbc.mysql.CityCurd
   java java/mysql/CityCurd
   ```

##### javac

Java 虚拟机启动时，可以指定不同的参数对运行模式进行选择。 比如，指定“-Xint”，就是告诉 JVM 只进行解释执行，不对代码进行编译，这种模式抛弃了 JIT 可能带来的性能优势。毕竟解释器（interpreter）是逐条读入，逐条解释运行的。与其相对应的，还有一个“-Xcomp”参数，这是告诉 JVM 关闭解释器，不要进行解释执行，或者叫作最大优化级别。那你可能会问这种模式是不是最高效啊？简单说，还真未必。“-Xcomp”会导致 JVM 启动变慢非常多，同时有些 JIT 编译器优化方式，比如分支预测，如果不进行 profiling，往往并不能进行有效优化

* 获取编译警告，设置编译选项，-Xline:unchecked

-Xlint 选项告诉编译器对一些普遍容易出现的代码问题进行检查

* -Xlint -Xlint:all，执行所有的检查
* -Xlint:deprecation，与 -deprecation 一样，检查废弃的方法
* -Xlint:fallthrough，检查 switch 语句中是否缺少 break 语句
* -Xlint:none，不执行任何检查
* -Xlint:path，检查类路径和源代码路径上的所有目录是否存在
* -Xlint:serial，警告没有 serialVersionUTD 的串行化类
* -Xlint:unchecked，对通用类型与原始类型之间的危险转换给予警告

反编译类

```shell
javap -c ClassName
```

##### 文本文件和字符集

###### 源文件的字符编码

作为程序员，要与 java 编译器交互，这种交互需要通过本地系统的工具来完成。例如，可以使用中文版的记事本来写 java 源代码文件，但这样写出来的源码不是随处可用的，因为它们使用的是本地的字符编码，只有编译后的 class 文件才能随处使用，它们会自动地使用 modified UTF-8 编码来处理标识符和字符串。即在程序编译和运行时，依然有3种字符编码参与其中：

* 源文件：本地编码
* 类文件：modified UTF-8
* 虚拟机：UTF-16

使用 -encoding 标记来设定源文件的字符编码

```shell
javac -encoding UTF-8 Myfile.java
```

为了使源文件能够到处使用，必须使用普通的 ASCII 编码。即，需要将所有非 ASCII 字符转换成等价的 Unicode 编码。JDK 包含一个工具---native2ascii，可以用它来将本地字符编码转换成普通的 ASCII。这个工具直接将输入中的每一个非 ASCII 字符替换为一个  \u 加 4 位十六进制数字的 Unicode 值。使用 native2ascii 时，需要提供输入和输出文件的名字

```shell
native2ascii Myfile.java Myfile.temp
```

用 -reverse 选项来进行逆向转换

```
native2ascii -reverse Myfile.temp Myfile.java
```

用 -encoding 选项指定另一种编码

```java
native2ascii -encoding UTF-8 Myfile.java Myfile.temp
```

##### java

* 运行属性设置

  ```
  # 设置编码
  java -Dfile.encoding=UTF-8
  # 配置日志文件
  java -Djava.util.logging.config.file=configFile
  ```

* 设置路径：-classpath -cp

* 启用或禁用断言：-enableassertions -ea

  ```shell
  # 在某个类或整个包中使用断言
  java -ea:MyClass -ea:com.mycompany.mylib... MyApp
  // 用选项 -disableassertions 或 -da 禁用某个特定类和包的断言
  ```

  有些类不是由类加载器加载，而是直接由虚拟机加载。可以使用这些开关有选择地启用或禁用那些类中的断言。启用和禁用所有断言的 -ea 和 -da 开关不能应用到那些没有类加载器的『系统类』 上。对于这些系统类，需要使用 -enablesystemassertions/-esa 开关启用断言。

* -Xprof 分析 10 版本之后移除

##### jconsole

jdk 附带的 jconsole 是一个图像分析界面

##### 堆转储

可以使用 jmap 实用工具获得一个堆的转储，其中显示了堆中的每个对象

```shell
jmap -dump:format=b,file=dumpfileName processID
jhat dumpFileName
```

##### jar

###### jar 归档

java 归档（JAR）文件既可以包含类文件，也可以包含诸如图像和声音这些其他类型的文件。

*JAR程序选项*

* c

  创建一个新的或空的存档文件并加入文件。如果指定的文件名是目录，将递归处理

* C

  改变 classes 子目录，增加类文件

  ```shell
  jar cvf JARFileName.jar -C classes *.class
  ```

* e

  在清单文件中创建一个条目

* f

  将 jar 文件名指定为第二个命令行参数。如果没有这个参数，jar 命令会将结果写到标准输出上（在创建 jar 文件时）或者从标准输入中读取它（在解压或列出 jar 文件内容时）

* i

  建立索引文件，用于加快对大型归档的查找

* m

  将一个清单文件（manifest）添加到 jar 文件中，清单是对存档内容和来源的说明。每个归档有一个默认的清单文件。如果想验证归档文件的内容，可以提供自己的清单文件

  ```shell
  // 添加清单文件并打包
  jar cmf myJarFile.jar myManifestFile.mf *.class
  ```

* M

  不为条目创建清单文件

* t

  显示内容表

  ```
  jar tf myJarFile.jar
  ```

* u

  更新一个已有的 jar 文件

* v

  生成详细的输出结果

* x

  解压文件，如果提供一个或多个文件名，只解压这些文件，否则解压所有文件

* 0

  存储，不进行 zip 压缩

###### 清单文件

除了类文件，图像和其他资源外，每个 jar 文件还包含一个用于描述归档特征的清单文件。清单文件被命名为 MANIFEST.MF，它位于 jar 文件的一个特殊 META-INF 子目录中。最小的符合标准的清单文件是很简单的：

```manifest
Manifest-Version: 1.0
```

复杂的清单文件可能包含更多条目。这些清单条目被分成多个节。第一节被称为主节。它作用于整个 jar 文件。随后的条目用来指定已命名条目的属性，这些已命名的条目可以是某个文件、包或者 URL。它们都必须起始于名为 Name 的条目。节与节之间用空行分开。

```java
Manifest-Version: 1.0 	// 描述这个归档文件的行
Name: Woozle.class		// 描述这个文件的行
Name: com/company/mypkg/     // 描述这个包的行
```

创建一个包含清单的 JAR 文件

```java
// 将希望添加到清单文件中的行放到文本文件中，然后运行
jar cfm MyArchive.jar manifest.mf com/mycompany/mypkg/*.class
```

更新一个已有的 jar 文件的清单，需要将增加的部分放置到一个文本文件中，然后执行

```jar
jar ufm MyArchive.jar manifest-additions.mf
```

###### 可执行 jar 文件

可以使用 jar 命令中的 e 选项指定程序的入口点（通常需要在调用 java 程序加载器时指定的类）：

```jar
jar cvfe MyProgram.jar com.mycompany.mypkg.MainAppClass files to add
```

或者在清单中指定应用程序的主类，包括以下形式的语句：

```
// 不要将扩展名.class 添加到主类名中
Main-Class: com.mycompany.mypkg.MainAppClass   	
```

清单文件的最后一行必须以换行符结束。否则，清单文件将无法被正确地读取。

启动应用程序：

```
java -jar MyProgram.jar
```

* 在 Windows 平台中，java 运行时安装器将建立一个扩展名为 .jar 的文件与 javaw -jar 命令相关联来启动文件（与 java 命令不同，javaw 命令不打开shell窗口）
* 在  Solaris 平台中，操作系统能够识别 jar 文件的魔法数格式，并用 java -har 命令启动它
* 在 Mac OS X 平台中，操作系统能够识别 .jar 扩展名文件。当双击 jar 文件时就会执行 java 程序可以运行

###### 密封

可以将 java 包密封以保证不会有其他的类加入到其中。如果在代码中使用了包可见的类、方法和域，就可能希望密封包。如果不密封，其他类就有可能放在这个包中，进而访问包可见的特性

想要密封一个包，需要将包中的所有类放到一个 jar 文件中。在默认情况下，jar 文件中的包是没有密封的。可以在清单文件的主节中加入一行来改变全局的默认设定。

```
// 全局设定
Sealed: true 				
```

对于每个单独的包，可以通过在 JAR 文件的清单中增加一节，来指定是否想要密封这个包

```java
Name: com/mycompany/util/
Sealed: true					// 单个包设定

Name: com/mycompany/misc/
Sealed: false	 			// 单个包设定
```


