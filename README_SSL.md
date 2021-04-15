# 证书生成（开启ssl时参考此文档）

## 生成步骤
各参数含义：
  * -genkeypair 生成密钥
  * -keyalg 指定密钥算法，这时指定RSA,
  * -keysize 指定密钥长度，默认是1024位,这里指定2048，长一点，我让你破解不了（哈哈...）,
  * -siglag 指定数字签名算法，这里指定为SHA1withRSA算法
  * -validity 指定证书有效期，这里指定36500天，也就是100年，我想我的应用用不到那么长时间
  * -alias 指定别名，这里是cas.server.com
  * -keystore 指定密钥库存储位置，这里存在d盘
  * -dname 指定用户信息，不用一个一个回答它的问题了；

注意：CN=域名，我们采用`sso.baicaowei.cn`
1.
windows:
```cmd
keytool -genkeypair -keyalg RSA -keysize 2048 -sigalg SHA1withRSA -validity 36500 -alias sso.baicaowei.cn -keystore C:\etc\cas\thekeystore -keypass changeit -storepass changeit -dname "CN=sso.baicaowei.cn,OU=baicaowei,O=IT-wms,L=HangZhou,ST=ZheJiang,C=CN"
```
linux:
```bash
keytool -genkeypair -keyalg RSA -keysize 2048 -sigalg SHA1withRSA -validity 36500 -alias sso.baicaowei.cn -keystore /etc/cas/thekeystore -keypass changeit -storepass changeit -dname "CN=sso.baicaowei.cn,OU=baicaowei,O=IT-wms,L=HangZhou,ST=ZheJiang,C=CN"
```
2.导出数字证书
windows  在cmd下输入如下命令：
```cmd
keytool -exportcert -alias sso.baicaowei.cn -storepass changeit -keystore C:\etc\cas\thekeystore -file C:\etc\cas\cas.cer -rfc
```
linux:
```bash
keytool -exportcert -alias sso.baicaowei.cn -storepass changeit -keystore /etc/cas/thekeystore -file /etc/cas/cas.cer -rfc
```

3.将服务端的证书tomcat.cer导入到客户端java的cacerts证书库中
cmd到 `${JAVA_HOME}jre\lib\security`

因为Windows下的jdk安装目录有空格，如果直接执行导入命令，会报错：
```
非法选项:  Files\Java\jdk1.8.0_251\jre\lib\security\cacerts
```
为了避免该问题，我们可以先定位到 `security` 目录：
```cmd
cd %JAVA_HOME%\jre\lib\security\
```
之后，windows 再运行如下命令：
```cmd
keytool -import -alias sso.baicaowei.cn -keystore cacerts -keypass changeit  -storepass changeit  -file c:/work/cas.cer -trustcacerts
# 密码为 changeit
```


4.检查是否导入成功
```cmd
keytool -list -keystore "cacerts" | findstr/i server

keytool -list -keystore "%JAVA_HOME%\jre\lib\security\cacerts" | findstr/i server
```
有东西输出代表成功