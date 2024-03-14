# siteproxy 2.0
<a href="https://github.com/netptop/siteproxy/blob/master/README_english.md"><strong>English</strong></a>
<br />
<br />
Siteproxy 2.0 使用了service worker, 使得代理更加稳定, 可以代理了的网站更多。
同时使用hono替代express，速度提高4倍。 支持cloudflare worker部署。
反向代理, 免翻墙访问youtube/google, 支持github和telegram web登录(请注意不要通过不信任的代理登录)。
纯web页面的在线代理， 客户端无需任何配置，反向代理到internet。 

```
                                                 +----> google/youtube
                             +----------------+  |
                             |                |  |
user browser +-------------->+ siteproxy      +-------> wikipedia
                             |                |  |
                             +----------------+  |
                                                 +----> chinese forums
```
请勿将本项目用于非法用途，否则后果自负。

## 目录

- [特点](#特点)
- [使用技巧](#使用技巧)
- [部署到cloudflare_worker](#部署到cloudflare_worker)
- [部署到vps或者云服务器](#部署到vps或者云服务器)
- [docker部署](#docker部署)
- [联系方式](#联系方式)

### 特点
- 使用hono替代express，速度提高4倍。 
- 支持cloudflare worker部署。
- 支持密码控制代理，知道密码才能访问代理。
- 不需要客户端的任何配置，访问代理网址即可访问全世界。
- 支持github和telegram web登录。
- 输入部署siteproxy的代理网址，就可以访问全世界，并隐藏你的IP。
- 客户端不需要任何软件安装，客户浏览器也不需要任何配置。 

### 使用技巧
1. 可以通过部署的siteproxy进行git clone，方法:
```
git clone https://your-proxy-domain.name/user-your-password/https/github.com/the-repo-to-clone
```

### 部署到cloudflare_worker
```
1. 假设你的域名已经管理在cloudflare名下;
2. git clone本项目，并使用文本编辑器打开build/worker.js (不用git clone,直接下载这个文件也可以)
3. 搜索http://localhost:5006字符串，将它替换为你的代理网站域名，比如https://your-proxy-domain.name
   同时搜索user22334455,将其修改为你自己想设置的密码。
4. 创建一个worker，并编辑worker，将上一步编辑过的worker.js拷贝粘贴到worker里面，保存部署。
5. 在Workers & Pages页面，打开刚才保存的worker，点顶部的'触发器', 之后'添加自定义域', 设置为你的代理域名。
6. 现在可以直接访问https://your-proxy-domain.name/user-your-password/, 就可以了。注意这里的域名和密码替换为你自己的域名和密码。
```

### 部署到vps或者云服务器
```
1. 创建一个ssl website(使用certbot and nginx, google下用法), 配置nginx,
   /etc/nginx/sites-enabled/default 需要包含以下内容:
   ...
   server {
      server_name your-proxy.domain.name
      location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass       http://localhost:5006;
      }
   }
2. 执行:sudo systecmctl restart nginx
3. 用户环境下执行下列命令安装node环境, 如果你已经有node环境, 忽略这一步
   (1)curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
   (2)source ~/.bashrc
   (3)nvm install v18
4. 执行:git clone https://github.com/netptop/siteproxy.git;
5. 执行:cd siteproxy;
6. 打开并修改保存config.json文件:
   {
      "proxy_url": "https://your-proxy.domain.name", // 这个是你申请到的代理服务器域名
      "token_prefix": "/user-SetYourPasswordHere/",  // 这个实际上是你的网站密码，用来防止非法访问,注意保留首尾的斜杠。
      "description": "注意:token_prefix相当于网站密码，请谨慎设置。 proxy_url和token_prefix合起来就是访问网址。"
   }
7. 执行:nohup node bundle.js &
8. 现在就可以在浏览器中访问你的域名了, 网址就是前面的proxy_url加上token_prefix.
9. 如果想套CloudFlare加速, 可以参考CloudFlare说明
```
### docker部署
```
1. 配置域名对应的ssl证书和nginx，指向本地5006端口。
2. git clone本项目.
3. 打开并修改保存config.json文件:
   {
      "proxy_url": "https://your-proxy.domain.name", // 这个是你申请到的代理服务器域名
      "token_prefix": "/user-SetYourPasswordHere/",  // 这个实际上是你的网站密码，用来防止非法访问,注意保留首尾的斜杠。
      "description": "注意:token_prefix相当于网站密码，请谨慎设置。 proxy_url和token_prefix合起来就是访问网址。"
   }
4. 进入docker-node子目录。 sudo docker compose up
5. 现在可以直接访问https://your-proxy-domain.name/user-your-password/, 就可以了。注意这里的域名和密码替换为你自己的域名和密码。
```
### 联系方式
Telegram群: @siteproxy
<br />
email: netptop@gmail.com
