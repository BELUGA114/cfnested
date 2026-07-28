# 服务器优选工具

一个简化版的Cloudflare Workers优选工具，用来生成订阅链接

原项目 [yx-auto](https://github.com/byJoey/yx-auto)

## 主要功能

- 优选域名：内置了一些常用的优选域名
- 优选IP：分别从`ymyuuu/IPDB`仓库的`BestCF/bestcfv4.txt`和`BestCF/bestcfv6.txt`获取IPv4/IPv6地址，并按返回顺序生成节点
- GitHub优选：默认从gslege/CloudflareIP并发拉取DE、JP、NL、SG、US地区IP列表，也支持自定义来源
- 多协议支持：VLESS、Trojan、VMess
- 多客户端格式：Clash、Surge、Quantumult X等
- 强制TLS：所有节点仅使用TLS，禁止80、8080、8880、2052、2082、2086和2095端口

## 部署

1. 去Cloudflare Dashboard，找到Workers & Pages
2. 创建个新Worker
3. 把`_worker.js`的代码复制进去
4. 保存部署就完事了

## 使用

打开你的Worker地址，会看到一个界面：

1. 填域名和UUID
2. 选协议（VLESS/Trojan/VMess）
3. 选客户端类型
4. 点按钮生成订阅链接

就这么简单。

## 订阅链接格式

生成的链接大概长这样：
```
https://your-worker.workers.dev/{UUID}/sub?domain=your-domain.com&epd=yes&epi=yes&egi=yes
```

可以通过`&target=`参数指定输出格式：
- `base64` - 默认格式
- `clash` - Clash配置
- `surge` - Surge配置  
- `quantumult` - Quantumult配置

## URL参数

所有配置都通过URL参数控制，不需要环境变量。

主要参数：
- `domain` - 你的域名（必填）
- `epd` - 启用优选域名（默认yes）
- `epi` - 启用优选IP（默认yes）
- `egi` - 启用GitHub优选（默认yes）
- `piu` - 自定义IP来源URL（可选，仅支持HTTPS）
- `ev` - 启用VLESS（默认yes）
- `et` - 启用Trojan（默认no）
- `mess` - 启用VMess（默认no，注意不是vm，会被屏蔽）
- `ipv4/ipv6` - IP版本选择（默认都开启）
- `target` - 输出格式（base64/clash/surge/quantumult）

## 注意事项

- 这只是一个订阅生成工具，不提供代理服务
- 生成的节点需要配合你自己的服务器使用
- UUID必须是标准格式（xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx）
- VMess参数用的是`mess`不是`vm`，因为`vm`会被某些地方屏蔽
- 订阅不会生成非TLS节点；自定义列表中的HTTP端口和非HTTPS数据源会被忽略
- ECH自定义DNS必须使用HTTPS；无效值会回退到默认DoH地址
