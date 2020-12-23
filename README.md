# 项目说明

本项目用于自助更改LDAP用户密码，是基于docker-LTB-self-service-password镜像更改而来，适用于国内网络环境的构建源码

配置文件存放于 assets/config.inc.php中

## LDAP配置
```
# LDAP
$ldap_url = "ldap://LdapDomain:389";
$ldap_starttls = false;
$ldap_binddn = "cn=root,dc=com,dc=cn";#链接账号
$ldap_bindpw = "*****";#管理账号密码
$ldap_base = "dc=com,dc=cn";# 搜索base结构
$ldap_login_attribute = "uid";
$ldap_fullname_attribute = "cn";
$ldap_filter = "(&(objectClass=person)($ldap_login_attribute={login}))";
#加密方式需要与Ldap里的密码设置一致
$hash = "MD5";

#配置邮箱


# keyphrase必须把默认的进行修改，否则项目启动会提示“Token encryption requires a random string in keyphrase setting”

$keyphrase = "goodluck";
```

## 项目构建与运行

### build

```
docker build -t="youname/ltb-self-service-password" .
```


### 运行

```
docker run -d --restart=always --name=ldap-ssp  -p 8765:80 youname/ltb-self-service-password
# 查看日志
 docker exec -ti $(docker ps | grep 'ltb-self-service-password' | awk '{print $1}') tail /var/log/apache2/error.log
```
