#### 生成随机密码
```
export PW=`pwgen -Bs 10 1`
echo $PW > password
```

#### 
```
# 创建一个别名为exampleca的证书，该证书存放在名为exampleca.jks的密钥库中，若exampleca.jks密钥库不存在则创建。
keytool -genkeypair -v \
  -alias exampleca \
  -dname "CN=exampleCA, OU=Example Org, O=Example Company, L=San Francisco, ST=California, C=US" \
  -keystore exampleca.jks \
  -keypass:env PW \
  -storepass:env PW \
  -keyalg RSA \
  -keysize 4096 \
  -ext KeyUsage:critical="keyCertSign" \
  -ext BasicConstraints:critical="ca:true" \
  -validity 9999

# 查看exampleca.jks这个密钥库里面的所有证书
keytool -list -keystore exampleca.jks

# 将exampleca.jks证书库中别名为exampleca的证书条目导出到证书文件exampleca.crt中
keytool -export -v \
  -alias exampleca \
  -file exampleca.crt \
  -keypass:env PW \
  -storepass:env PW \
  -keystore exampleca.jks \
  -rfc



```