---
title: acme-sh tutorial
excerpt: \"acme.sh\" is a shell script to get and install the cerification automatically, use ACME protocal.
date: 2023-02-19
---

# ACME.SH 教程

项目地址：https://github.com/acmesh-official/

常见参考：

- [ZeroSSL.com CA 证书申请 - ACME.sh](https://github.com/acmesh-official/acme.sh/wiki/ZeroSSL.com-CA)
- [ACME-SH DNS API - ACME.sh](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)

## 安装

```sh
curl https://get.acme.sh | sh -s email=my@example.com
```

## ZeroSSL 申请证书流程

1. 注册CA

    这个注册并非账号的注册，该邮箱为申请证书必填项，仅用于证书办法机构向我们指定邮箱发送通知和更新，可以通过下述命令来指定邮箱：

    ```sh
    acme.sh  --register-account -m my@example.com --server zerossl
    ```

2. 申请证书

    申请证书需要验证域名所有权，根据不同的CA（证书颁发机构）其方式可能不同，可以通过HTTP、DNS、邮箱等方式；这里推荐使用DNS方式验证，这里以 cloudflare 举例：

    ```sh
    export CF_Key="generated_key"
    export CF_Email="test@example.com"
    ```

    `CF_Key` 可以在 [api-token - cloudflare](https://dash.cloudflare.com/profile/api-tokens) 链接下创建；

    `CF_Zone_Key` 在每个域名的 overview 界面的右下角。

    这个是`acme.sh`官方可参考的DNS验证文档：。

3. 生成证书

    `-d `选项的参数是要申请证书的域名，但请根据上述设置的验证授权的环境来设置正确的DNS域名解析服务商，在`--dns`选项中指定参数，不同域名解析服务商的代号不同：

    - Cloudflare: dns_cf
    - Ali: dns_ali

    具体可以参考[dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)

    有需求可直接生成一个通配符域名证书：

    ```sh
    acme.sh --issue --dns dns_ali -d '*.example.com'
    ```

    默认情况下，生成的证书的目录`.acme.sh`目录下。

4. 执行定时任务。


证书生成完成之后 **acme.sh** 会自动记保存API ID 和 API key，会保存到 `~/.acme.sh/account.conf`，下次再使用阿里云API的时候不需要再指定阿里云的 AccessKey了。

## 三、安装证书

前面生成的证书默认都会生成到`~/.acme.sh/`目录下面。

以 Nginx 为例安装证书

```sh
acme.sh --installcert -d '*.example.com' \
--key-file       /etc/ssl/private/'*.example.com.key'  \
--fullchain-file /etc/ssl/certs/'*.example.com.pem' \
--reloadcmd     "sudo service nginx force-reload"
```

以上命令会将证书拷贝复制到 `/etc/nginx/cert/`目录下，并重启 Nginx 。

上面指定的所有参数都会被自动记录下来，并在将来证书自动更新以后， 会被再次自动调用。

需要注意到是，上面的`--installcert`命令后面跟的 `sudo service nginx force-reload` 参数，要成功执行的前提需要将Nginx 添加到系统的服务中并设置相关script，更多关于添加到服务中的设置查看 [Red Hat NGINX Init Script](https://link.segmentfault.com/?url=https%3A%2F%2Fwww.nginx.com%2Fresources%2Fwiki%2Fstart%2Ftopics%2Fexamples%2Fredhatnginxinit%2F)。

安装证书并配置完成后访问 [SSL Server Test](https://link.segmentfault.com/?url=https%3A%2F%2Fwww.ssllabs.com%2Fssltest%2Findex.html)，输入域名测试证书。

## 四、更新证书

更新证书不需要做任何操作， **acme.sh** 会自动创建 cronjob，每天 0:00 点自动检测所有的证书，如果证书快过期了，则会自动更新证书。

### 参考资料

- [An ACME Shell script: acme.sh](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Facmesh-official%2Facme.sh)
- [acme.sh 使用说明](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Facmesh-official%2Facme.sh%2Fwiki%2F%E8%AF%B4%E6%98%8E)
- [使用 acme.sh 给 Nginx 安装 Let’ s Encrypt 提供的免费 SSL 证书](https://link.segmentfault.com/?url=https%3A%2F%2Fruby-china.org%2Ftopics%2F31983)

---

## Setup via Docker

https://github.com/acmesh-official/acme.sh/wiki/Run-acme.sh-in-docker

1. 拉取镜像

    ```sh
    docker pull neilpang/acme.sh
    ```

2. 登录/注册CA

    ```sh
    docker run --rm \
        -it \
        -v ~/.acme.sh:/acme.sh \
        --net=host \
        neilpang/acme.sh \
        --register-account -m 'test@example.com'
    ```

    `docker run -v`  为持久化挂载，防止容器删除后配置消失。

3. 申请证书

    因为使用DNS的方式进行申请，需要设置环境变量，这里以Cloudflare为参考：

    ```sh
    docker run --rm  -it  \
      -e CF_Key="sdfsdfsdfljlbjkljlkjsdfoiwje" \
      -e CF_Email="test@example.com" \
      -v ~/.acme.sh:/acme.sh  \
      --net=host \
      neilpang/acme.sh  --issue --dns dns_cf -d example.com
    ```

4. 安装证书

    ```sh
    docker run --rm  -it  \
      -v ~/.acme.sh:/acme.sh  \
      --net=host \
      neilpang/acme.sh --installcert -d '*.example.com' \
        --key-file       /etc/ssl/private/'*.example.com.key'  \
        --fullchain-file /etc/ssl/certs/'*.example.com.pem' \
        --reloadcmd     "sudo service nginx force-reload"
    ```

## ACME.SH & Docker

接下来用Docker来配置Nginx。

1. 将`acme.sh`以守护进程方式启动：

    ```sh
    docker run --restart always -itd  \
      -v ~/docker-volumes/acme.sh:/acme.sh  \
      --net=host \
      --name=acme.sh \
      -v /var/run/docker.sock:/var/run/docker.sock \
      neilpang/acme.sh daemon
    ```

2. 获取证书

   ```sh
   docker exec \
     -e CF_Email=my@example.com \
     -e CF_Key=xxxxxxx \
     acme.sh --issue -d "*.hihusky.com" --dns dns_cf
   ```

3. 启动其他启动

   ```sh
   docker run --restart always -itd \
     --label sh.acme.autoload.domain=*.hihusky.com \
     -v ~/docker-volumes/nginx:/etc/nginx \
     --net=host \
     --name=nginx \
     nginx
   ```

   在这里设置了label，用于后续acme.sh钩子操作重启nginx。

4. 升级`acme.sh`

   如果不升级在接下来的步骤中不出现`can't work with curl 8.0.1`错误

   ```sh
   docker exec acme.sh --upgrade -b dev
   ```

5. 安装证书

   ```sh
   docker exec \
       -e DEPLOY_DOCKER_CONTAINER_LABEL=sh.acme.autoload.domain=*.hihusky.com \
       -e DEPLOY_DOCKER_CONTAINER_KEY_FILE=/etc/nginx/ssl/*.hihusky.com_ecc/*.hihusky.com.key \
       -e DEPLOY_DOCKER_CONTAINER_CERT_FILE="/etc/nginx/ssl/*.hihusky.com_ecc/*.hihusky.com.cer" \
       -e DEPLOY_DOCKER_CONTAINER_CA_FILE="/etc/nginx/ssl/*.hihusky.com_ecc/ca.cer" \
       -e DEPLOY_DOCKER_CONTAINER_FULLCHAIN_FILE="/etc/nginx/ssl/*.hihusky.com_ecc/fullchain.cer" \
       -e DEPLOY_DOCKER_CONTAINER_RELOAD_CMD="service nginx force-reload" \
       acme.sh --deploy -d *.hihusky.com  --deploy-hook docker --ecc
   ```

   该操作会将上述申请的证书，从`acme.sh`的容器中拷贝到`nginx`的容器中，同时会在`nginx`容器中执行`service nginx force-reload`，在证书路径没问题的情况下，就完成了nginx的证书的更新。
