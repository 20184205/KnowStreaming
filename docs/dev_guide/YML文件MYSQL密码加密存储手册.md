## YML文件MYSQL密码加密存储手册

### 1、本地部署加密

**第一步：生成密文**

在本地仓库中找到jasypt-1.9.3.jar，默认在org/jasypt/jasypt/1.9.3中，使用`java -cp`生成密文

```
java -cp jasypt-1.9.3.jar org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input=mysql密码 password=加密的salt algorithm=PBEWithMD5AndDES
```

得到密文，例如DYbVDLg5D0WRcJSCUGWjiw==

**第二步：配置密文**

使用密文替换[application.yml](https://github.com/didi/KnowStreaming/blob/master/km-rest/src/main/resources/application.yml)中mysql的明文密码为ENC(密文），例如

```
know-streaming: 
  username: root
  password: ENC(DYbVDLg5D0WRcJSCUGWjiw==)
```

**第三步：配置加密的salt（选择其一）**

- 配置在[application.yml](https://github.com/didi/KnowStreaming/blob/master/km-rest/src/main/resources/application.yml)中（不推荐）

```
jasypt:
  encryptor:
    password: salt
    algorithm: PBEWithMD5AndDES
    iv-generator-classname: org.jasypt.iv.NoIvGenerator
```

- 配置程序启动时的命令行参数

```
java -jar xxx.jar --jasypt.encryptor.password=salt
```

- 配置程序启动时的环境变量

```
export JASYPT_PASSWORD = salt
```

```
java -jar xxx.jar --jasypt.encryptor.password=${JASYPT_PASSWORD}
```

  



