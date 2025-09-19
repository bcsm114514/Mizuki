---
title: 利用Cloudflare Workers对网站进行加速
published: 2025-08-17
description: ''
image: ''
tags: ["Cloudflare"]
category: '数码'
draft: false 
lang: ''
---
各位可能有时碰到如下困境:
- 网站访问延迟高
- 网站下载速度慢
- 网站被限制访问

这时，我们就可以使用**Cloudflare**对网站进行加速。并且此等方法还能起到免费的DDos，CC攻击等方式流量防御。

注: 在这篇博文我介绍的是基于Worker的自定义路由的方式，并不是SAAS回源，请注意甄别。另外，Cloudflare已禁止反代等服务，请小心使用。此项目仅供学习使用。

## 部署Cloudflare Workers
1.进入[Cloudflare官网](https://dash.cloudflare.com)，注册或登录一个账号
![Cloudflare注册账号](https://img.bcsm.us.kg/bcsm114514/img/refs/heads/main/CFRegister.jpg)

2.注册进来后找到**计算(Workers)**下的**Workers和Pages**
![Workers和Pages](https://img.bcsm.us.kg/bcsm114514/img/refs/heads/main/Workers.jpg)

3.创建一个Workers
![创建Workers](https://img.bcsm.us.kg/bcsm114514/img/refs/heads/main/EditWorkers.jpg)

4.创建完成后编辑代码，粘贴我给出的代码
```javascript
export default {
  async fetch(request) {
    const TARGET = "example.org";   // 目标源站
    const CACHE_TTL = 3600;         // 缓存 1 小时
    const CACHE_EXT = /\.(jpg|jpeg|png|gif|ico|svg|css|js|woff2?|ttf|html)$/i;

    let url = new URL(request.url);
    url.hostname = TARGET;
    url.protocol = "https";

    let newHeaders = new Headers(request.headers);
    newHeaders.set("Host", TARGET);

    let newRequest = new Request(url.toString(), {
      method: request.method,
      headers: newHeaders,
      body: request.body
    });

    // 如果是静态文件，开启缓存
    if (CACHE_EXT.test(url.pathname)) {
      return fetch(newRequest, {
        cf: {
          cacheEverything: true,
          cacheTtl: CACHE_TTL
        }
      });
    }

    // 其他请求不缓存
    return fetch(newRequest);
  }
}
```
然后修改三个地方:TARGET,CACHE_TTL和CACHE_EXT
- TARGET: 改成你的域名或IP
- CACHE_TTL: 改成你想要的缓存时长，默认一小时，单位为秒
- CACHE_EXT: 缓存的文件后缀

![Workers代码](https://img.bcsm.us.kg/bcsm114514/img/refs/heads/main/WorkersCode.jpg)

至此，第一步完成

## 自定义域名解析
1.绑定一个域名至CF，如果没有并想要免费域名请查看[博文](https://www.bcsm.us.kg/posts/freedomain/)

2.解析域名至优选/反代IP，域名
可以加速访问的有两种，分别是优选IP(域名)以及反代IP(域名)

什么是**反代IP**:
用户 ──> 反代IP(别人服务器) ──> Cloudflare ──> 源站

什么是**优选IP**：
用户 ──> 优选IP(Cloudflare节点) ──> Cloudflare ──> 源站

网上有很多优选域名可以直接使用，比如说**visa.cn**
也可以自己去寻找与优选反代IP与官方IP

将你的域名CNAME解析到优选/反代域名，A解析到IPV4优选/反代IP(AAAA解析到IPV6优选/反代IP)，注意，**不要打开小黄云**！！！

![解析](https://img.bcsm.us.kg/bcsm114514/img/refs/heads/main/Domain.jpg)

至此，第二步完成

## Workers路由绑定
来到刚才Workers项目的设置界面，新增加一个路由配置
![路由配置](https://img.bcsm.us.kg/bcsm114514/img/refs/heads/main/Router.jpg)
区域选择你的域名，路由填写"你的域名/*"保存

至此，所有配置完成
