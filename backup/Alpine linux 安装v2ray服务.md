Alpine linux 安装v2ray服务

> 由于cloudflare的问题，使用workers部署的vmess服务没过几分钟就会超处cpu使用限制。可以说基本就无法使用了，只能用以应急。
>
> 所以只能继续使用Oracle的免费服务器了。反正闲着也没用。
>
> 我的服务在大阪区，速度比较差，观察了下，可能是线路的问题，羡慕那些开东京、春川机房的

### 安装方法

由于机器内存比较小。我安装的alpine-3.12版本 linux。

安装步骤

```sh
apk update #更新软件源
apk add v2ray #安装v2ray服务
rc-update add v2ray #rc管理启动，添加v2ray的开机启动
mv /etc/v2ray/config.json config.json.bak #备份一下默认配置文件。非必要
mv /etc/v2ray/vpoint_vmess_freedom.json config.json #更改vponit-vmess为默认配置
cat /etc/v2ray/config.json #查看配置文件关键信息，主要为uuid和port
service v2ray restart #重启v2ray服务
```

按照如上步骤就安装完毕了。我使用的是v2rayN为客户端。配置信息在上述config.json内都能找到。

测试YouTube速度

![Image](https://github.com/user-attachments/assets/aa2f6924-a558-4b9d-a12b-40dca9e8123c)

速度一般啊，1920x1080@60fps还算流畅，将就用吧……