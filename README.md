# pay-v2ray-sspanel-v3-mod_Uim-plugin

# 收费版本

# 使用教程请看 [WIKI](https://github.com/rico93/pay-v2ray-sspanel-v3-mod_Uim-plugin/wiki/)

### 免费版 [请点击这里](https://github.com/rico93/v2ray-sspanel-v3-mod_Uim-plugin/)

## 公告

1. 限速功能完成，是收费版本(不在提供源码，只提供二进制文件，需要联系使用[bot购买](https://t.me/Rico_V2_bot))
2. 完成胖虎 SSRPanel 和 SSpanel MySQL 连接的适配
3. 未来将会写自动配置 tls(摆脱caddy) 咕咕

## Thanks

1. 感恩的 [ColetteContreras's repo](https://github.com/ColetteContreras/v2ray-ssrpanel-plugin). 让我一个 Go 小白有了下手地。主要起始框架来源于这里
2. 感恩 [eycorsican](https://github.com/eycorsican) 在 v2ray-core [issue](https://github.com/v2ray/v2ray-core/issues/1514), 促成了 Go 版本提上日程

## 使用重点

1. 后端端口设置为 0 或为空，才会监听本地，不再是 443。
2. 后端支持中转，但必须使用我自己维护的 [ss-panel-v3-mod_Uim](https://github.com/rico93/ss-panel-v3-mod_Uim)。

## 项目状态

支持 [ss-panel-v3-mod_Uim](https://github.com/NimaQu/ss-panel-v3-mod_Uim)，使用 WEBAPI 或 数据库连接。

亦支持 [SSRPanel](https://github.com/ssrpanel/SSRPanel)，使用数据库连接。

**作为 ss-panel-v3-mod 后端目前支持：**

- 限速
- 流量记录
- 在线人数
- 节点负载
- 流量中转
- 在线 IP 上报
- 服务器是否在线
- 后端根据前端的设定自动调用 API 增加用户。

