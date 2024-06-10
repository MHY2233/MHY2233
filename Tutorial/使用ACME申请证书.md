## 使用 ACME 申请安装证书

### 1. 安装 acme
> 使用 acme 命令行工具来申请安装证书
```
$ curl https://get.acme.sh | sh
```
### 2. 安装 socat
> socat 是一款 Linux 下的工具软件，可以在两个不同的数据流之间建立连接，实现数据传输、转换和处理等功能
> acme 依赖 socat, 所以安装:
```
$ apt install socat
```
### 3. 添加软连接
> 添加到 bin 下面, 可以直接使用 acme.sh 脚本而不需要加上路径
```
$ ln -s /root/.acme.sh/acme.sh /usr/local/bin/acme.sh
```
### 4. 注册账号
> 默认使用 ZeroSSL, 注册成功会提示
```
$ acme.sh -register-account -m my@example.com
```
### 5. 开放 80 端口
> 因为申请证书时候需要验证(使用 standalone 方式), 证书颁发机构会通过申请证书的域名来访问一个字符串, 用来测试你是否是域名的主人. 所以需要 80 端口.
```
$ ufw allow 80
```
### 6. 申请证书
> 申请过程中可以用浏览器打开域名看到有一传随机字符串, 申请结束后再访问域名就没有了, 这就是使用 acme 创建了一个服务器, 证书颁发机构用来验证的过程.
```
$ acme.sh -issue -d domain.com -standalone -k ec-256
```
### 7. 切换 CA
> 有时候排队很慢, 需要切换一下 CA
```
$ acme.sh -set-default-ca -server letsencrypt
```
> 然后再重新执行第 6 步.
> 申请完证书后会有几个文件:
> - cert 文件: 是申请的 SSL 证书，包含了网站的公钥信息以及证书持有者的身份信息, 一般用来安装到 Web 服务器, 用于加密传输数据.
> - cert.key 文件: 是证书的私钥文件, 用来和公钥一起对通信数据进行加密或解密等操作, 一般保密并妥善保存.
> - Intermediate CA cert: 中间证书, 用于构建证书链, 保证证书的可信度.
> - chain certs 文件(fullchain.cer): 由于 ACME 证书申请是需要经过中间 CA 的签发, 生成的证书文件中还包含了中间 CA 和根 CA 的证书信息, 这些证书也放在了该文件中. Web 服务器需要将该文件与证书配合使用, 以便构建完整的证书链条, 确保客户端能够正确识别证书的有效性.
### 8. 安装到 nginx
```
acme.sh --install-cert -d example.com \
--key-file       /path/to/keyfile/in/nginxconfig/cert.key  \
--fullchain-file /path/to/fullchain/nginxconfig/fullchain.cer \
--reloadcmd     "service nginx force-reload"
```
> 除此之外, 证书默认 90 天有效, 90 天后会过期, 如果使用 acme.sh 会自动帮我们每天 renew























