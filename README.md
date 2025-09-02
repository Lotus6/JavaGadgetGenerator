# JavaGadgetGenerator
* JavaGadgetGenerator 工具，支持 ysoserial，Hessian，字节码，Expr/SSTI，Shiro，JDBC 等 Gadget 生成，封装，混淆，出网延迟探测，内存马注入等...
* 迭代了很多版本，一直没发布，重新更新！！！
* (如果对您有帮助，感觉不错的话，请您给个大大的 ⭐️❗️)
<img width="884" height="641" alt="image" src="https://github.com/user-attachments/assets/88bc2fac-41b7-41b7-ae9a-8af15582cead" />


## 1.ysoserial 模块

### Ysoserial chain

| Ysoserial Gadget   | 实现 |
| ------------------ | ---- |
| cc 等链            | √    |
| cb 等链            | √    |
| Fastjson1          | √    |
| Fastjson2          | √    |
| SpringAOP（jdk17） | √    |
| …                  |      |

### 帆软 Gadget

| 帆软 Gadget       | 实现 |
| ----------------- | ---- |
| FrHibernate       | √    |
| JacksonSingObject | √    |
| HSQL              | √    |
| HSQLBypass        | √    |

### 功能模块

| 功能                                    | 实现 |
| --------------------------------------- | ---- |
| 脏数据 ARRAY/linkedList/TCRESET         | √    |
| UTF-8 混淆 yso（2bytes/3bytes）/hessian | √    |
| 压缩 compress                           | √    |
| MysqlPipe 不出网利用                    | √    |
| Abstrant 继承                       | √    |

### Gadget 模块说明

1. Dnslog，dnslog 出网探测，参数：xxx.dnslog.cn
2. Httplog，Http 出网探测，参数：http://x.x.x.x
3. Sleep，延时探测，参数：10(约等于 10 秒)
4. SleepCheckFile，延时探测文件是否存在，参数 5(约等于 5 秒)：，额外参数 /tmp/test.txt
5. ClassLoaderFromBase64File，ClassLoader define 加载目标本地 base64 字节码文件，参数：/tmp/EvilBase64
6. ClassLoaderFromBase64Code，ClassLoader define 加载 base64 字节码，参数：Base64Str
7. UnsafeDefineAmFromBase64Code，Unsafe define 加载 base64 字节码，参数：Base64Str，
8. UrlClassLoaderFromJar，URLClassLoader 加载目标本地恶意 JAR，参数：jar 的路径(file:///tmp/Evil.jar) ，额外参数： jar 的 ClassName 
9. WebShell，随机生成 webshell 配置后，直接生成 gadget 即可
10. TomcatCmdEcho，Tomcat 通用回显：header 头 par1m：whoami，无参数
11. LoadClass，加载本地恶意 Class 文件内容，参数：/Users/xxx/Desktop/Evil.class
12. LoadClassBase64，加载恶意 base64 Class 文件内容，参数：base64Str 
13. MemorySystemSetProperty:分段设置 SystemSetProperty，ClassLoader 加载，参数处传入 base64 自定义字节码
14. SplitChunkWriteFile:分段写入文件到指定目录，ClassLoader 加载，参数处传入 base64 自定义字节码，额外参数传入路径 /tmp/EvilBase64
15. SplitChunkWriteThread:分段写入到 Tomcat ThreadName，ClassLoader 加载，参数处传入 base64 自定义字节码
16. SplitChunkWriteProperty:分段写入到 System.property，ClassLoader 加载，参数处传入 base64 自定义字节码
17. CodeFile，代码如：java.lang.Runtime.getRuntime().exec("open -a Calculator", 参数：/Users/xxx/Desktop/Code.txt
18. CodeBase64，base64 后想执行的 java 代码，参数：base64Str
19. Bcel，BCELClassloader 加载恶意字节码，参数：$$BCEL$$...
20. BcelClassFile，BCELClassloader 加载恶意字节码，参数：/Users/xxx/Desktop/Evil.class
21. ScriptBase64，js.eval() 方式执行，参数：base64Str（js eval code）
22. ScriptFile，js.eval() 方式执行，参数：/Users/xxx/Desktop/jsEvalCodeFile
23. JNDI，lookup JNDI 地址，参数：ldap://xxx.xxx.xxx.xxx
24. UploadFile，读取本地文件写入到目标机器路径，参数：本地文件路径，额外参数：目标路径
25. UploadFileBase64，本地文件 Base64 内容，参数：本地 base64 文件内容，额外参数：目标路径
26. MozillaClassLoader，Runtime，Template 被禁用情况下用 org.mozilla.javascript.DefiningClassLoader defineClass 加载字节码，参数：本地 class 文件路径，额外参数：ClassName
27. LoadRemoteClass，加载远程恶意 class，参数：http://127.0.0.1:8000/，额外参数：Txxxxx（恶意类名）
28. LoadRemoteJar，加载远程恶意 jar，参数：http://127.0.0.1:8000/evil.jar，额外参数：Txxxxx（恶意类名）
29. LoadRemoteSQL，加载远程恶意 sql，参数：http://127.0.0.1:8000/evil.sql


## 2.bytes 模块

### 封装模块

| 封装               | 实现方式                               | 利用                                                         |
| ------------------ | -------------------------------------- | ------------------------------------------------------------ |
| JDK_Abstrant       | Tmplate 链继承的父类                   | JDK 原生内置                                                 |
| XALAN_Abstrant     | Template 链继承父类                    | 外部 Apache Xalan 库的版本                                   |
| SnakeYaml          | ScriptEngineFactory 实现类             | Snakeyaml loadJar 时需要实现 ScriptEngineFactory 接口        |
| FastjsonGroovy     | @GroovyASTTransformation 注解封装      | Fastjson Groovy 1.2.80 利用                                  |
| FastjsonAutoGroovy | AutoCloseable 实现类                   | Fastjson 1.2.68 利用                                         |
| JavaSerialize      | Serializable 实现类                    | 反序列化利用                                                 |
| Java_Main          | 恶意方法封装到 _main 函数内            | Hessian 反序列化利用链 BCEL 需要（JavaWrapper#_main）        |
| JavaMain           | main 函数                              | 双击运行 jar                                                 |
| JavaCharset        | CharsetProvider 实现类                 | 写 jre/lib 目录时利用                                        |
| SvgJar 输出        | MANIFEST.MF SVG-Handler-Class:xxx 入口 | svg setter RCE 时利用                                        |
| ScriptSPIJar 输出  | SnakeYaml jar 包封装                   | Snakeyaml loadJar 时利用                                     |
| ClassName          | 自定义类名，不传入参数则随机生成类名   | 自定义类名，DerbyRemoteJarLoader 加载 jar 时执行 jar 包静态方法。 |


## 3.Expr/SSTI 模块

### Gadget 模块

| 表达式     | 命令执行 | 命令执行回显 | DNSLOG | Sleep | Base64Code | JNDI | LoadJar |
| ---------- | -------- | ------------ | ------ | ----- | ---------- | ---- | ------- |
| javaxEL    | √        | √            | √      | √     | √          | √    | x       |
| Spel       | √        | √            | √      | √     | √          | √    | x       |
| Ognl       | √        | √            | √      | √     | √          | √    | x       |
| Aviator    | √        | √            | √      | √     | √          | √    | x       |
| Enjoy      | √        | √            | √      | √     | √          | √    | x       |
| Jelly      | √        | √            | √      | √     | √          | √    | x       |
| Freemarker | √        | √            | √      | √     | √          | √    | x       |
| Velocity   | √        | √            | √      | √     | √          | √    | x       |
| SpringCPX  | √        | √            | √      | √     | √          | √    | √       |
| ScriptJs   | √        | √            | √      | √     | √          | √    | x       |
| Groovy     | √        | √            | √      | √     | √          | √    | x       |

### 字节码模块

* jsScript jdk 11被移除，unsafe.define <11 ，defineAnonymousClass <jdk17
* bcel 时恶意类不能继承 Abstrant

| Expr/SSTI  | 原生                     | spel-defineClass           | spel-bcel         | spel-bypass-jdk17 | spel-deserialize          | js-usafeAnom                   | js-unSafe | js-base64 | js-bcel           | js-bigInter |
| ---------- | ------------------------ | -------------------------- | ----------------- | ----------------- | ------------------------- | ------------------------------ | --------- | --------- | ----------------- | ----------- |
| Spel       | spel                     | √                          | √                 | √                 | √                         | √                              | √         | √         | √                 | √           |
| ScriptJs   | js                       | x                          | x                 | x                 | x                         | √                              | √         | √         | √                 | √           |
| Groovy     | define √<br>defineAnom √ | √                          | x($ 被groovy解析) | √                 | √                         | √                              | x         | x         | x($ 被groovy解析) | √           |
| Ognl       | Bcel √<br>defineAnom √   | x(ognl不支持 spel T()语法) | x                 | x                 | x                         | √                              | √         | √         | √                 | √           |
| Aviator    | Bcel √                   | √                          | x                 | √                 | x 序列化数据 EOF 长度过长 | public static  限制，不支持 js | x         | x         | x                 | x           |
| JavaxEL    | x                        | x（不支持spel）            | x                 | x                 | x                         | √                              | √         | √         | √                 | √           |
| Enjoy      | x                        | √                          | √                 | √                 | √                         | √                              | √         | √         | √                 | √           |
| FreeMarker | x                        | √                          | √                 | √                 | √                         | √                              | √         | √         | √                 | √           |
| Jelly      | x                        | √                          | √                 | √                 | √                         | √                              | √         | √         | √                 | √           |
| Velocity   | define √<br>defineAnom √ | √                          | √                 | √                 | √                         | √                              | √         | √         | √                 | √           |
| SpringCPX  | x                        | √                          | √                 | √                 | √                         | √                              | √         | √         | √                 | √           |


## 4.hession 模块

### Gadget 模块

| Hessian Gadget                         | 实现 | 利用方式                         |
| -------------------------------------- | ---- | -------------------------------- |
| XSTL                                   | √    | xslt 结合 bytegadget base64 生成 |
| PKCS9AttributesLazyValue               | √    | swing可以/proxy报错              |
| MimeLazyValue                          | √    | Swing/Proxy 可以                 |
| RomeJNDI                               | √    | JNDI                             |
| Resin                                  | √    | 远程 class                       |
| XBean                                  | √    | 远程 class                       |
| HashMapLazyValue                       | √    | Swing/Proxy 可以                 |
| SpringPartiallyComparableAdvisorHolder | √    | JNDI                             |

### Hessian 序列化模块

- [x] hessian1
- [x] hessian2

### Wrapper

- [x] SwingLazyValue（仅触发rt.jar中的静态方法）
- [x] ProxyLazyValue（无限制包的静态方法）


## 5.shrio 模块

* 使用时选择 YsoserialGadgetGenerate 对应 gadgetchain ，编码为 ShiroAES，然后在 shiro 模块输入密钥生成即可

### 功能模块

* 非 base64 字符混淆

### GadgetChain 模块

* 分离 tomcat(需要 x-www-form-urlencoded，且 post 进行字符进行 url 编码)
* spring 分块写文件，thread，property


## 6.JDBC 模块

* 工具集成常见的​JdbcAttack Payload，这些都是原始的连接串。下面是些注意事项：
  * 如果它们需要被嵌套在Fastjson中等进行利用，需要进行关键字符转义操作
  * 某些JDBC连接串因为自身语法必须添加换行符等

### H2Databse

* H2DataBase 数据库的打法是通过执行SQL语句来RCE的，而H2数据库建立连接时INIT参数配置正好支持执行多条SQL语句。

| 连接串         | 参数                                                         | 功能                                                         | 备注                                                         |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| H2CreateAlias  | JGG 生成的 java 代码                                         | 因为是Java代码执行所以支持漏洞利用参数中的Java代码的漏洞利用效果参数 | [如果攻防中INIT参数被过滤可以使用￼`ı`￼​大小写转换绕过](%5Bhttps%3A//rce.moe/2023/07/28/Metabase-CVE-2023-38646/%5D%28https://rce.moe/2023/07/28/Metabase-CVE-2023-38646/%29) |
| H2RunScript    | [http://127.0.0.1:2333/update](http://127.0.0.1:2333/update) | 远程加载 sql 文件执行多组 sql 语句                           | 关于.sql文件名后缀可以没有                                   |
| H2Groovy       | JGG 生成的字节码，用 js 加载                                 | 调用Groovy 引擎执行 JS 表达式再执行Java代码                  | H2支持调用 Groovy 引擎                                       |
| H2JavaScript   | JGG 生成的字节码，用js 加载                                  | 调用JavaScript引擎执行Java代码                               | H2支持调用 JavaScript 引擎                                   |
| H2StaticMethod | ldap://xxx.com                                               | 只支持静态方法 JNDI                                          | H2支持调用 静态方法                                          |

### DerbyDatabase

| 连接串               | 参数                                                         | 功能                                            | 备注 |
| -------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ---- |
| DerbyCodeLoader      | 正常 GadgetModule                                            | 利用 Spring ReflectUtils.defineClass 执行字节码 |      |
| DerbyRemoteJarLoader | 参数：远程 jar 包url（http://xx.com/xxx.jar）， jar名字需要和类名一样，因为内部需要 className，我默认截取的 jar 名字<br>额外参数：执行的静态函数名 | 远程加载 jar，执行 jar 的静态函数               |      |









**感谢**

https://github.com/frohoff/ysoserial

https://github.com/woodpecker-framework/ysoserial-for-woodpecker

https://github.com/pen4uin/java-memshell-generator

https://github.com/Y4er/ysoserial

https://github.com/xi3w3n/ysoserial

https://github.com/B0T1eR/ysoSimple

https://github.com/4ra1n/java-swing-gui-stater

https://github.com/woodpecker-appstore/jexpr-encoder-utils

https://github.com/kezibei/Urldns

https://github.com/unam4/yso-mysqlpipe

**免责声明**

本工具仅能在取得足够合法授权的企业安全建设中使用，在使用本工具过程中，您应确保自己所有行为符合当地的法律法规。

如您在使用本工具的过程中存在任何非法行为，您将自行承担所有后果，本工具所有开发者和所有贡献者不承担任何法律及连带责任。
