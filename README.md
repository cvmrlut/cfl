# cfl
Clash for Linux fork [backup](https://github.com/Elegycloud/clash-for-linux-backup)

# 说明
构成 [clash](https://github.com/Dreamacro/clash) & [yacd](https://github.com/haishanh/yacd) & shell script <br>
支持 x86_64/aarch64 平台 RHEL系 & Debian系 的Linux <br>
解决在服务器上拉取下载GitHub Dockerhub HuggingFace 等资源速度慢问题 <br>

# 使用流程
## 部署
下载并进入项目目录，编辑`.env`的`CLASH_URL`加入订阅地址，(选做：`CLASH_SECRET` 为自定义 Clash Secret，留空将随机生成字符串)
```bash
git clone https://github.com/cvmrlut/cfl.git && cd cfl
vim .env
```

## 启动代理
```bash
./start.sh
source /etc/profile.d/clash.sh
proxy_on
```

## 停止代理
```bash
./shutdown.sh
proxy_off
```

## 重载配置文件
若修改Clash配置文件`conf/config.yaml`，请运行 `restart.sh` 重启（不会更新订阅信息）

# 配置代理
> 图形UI：Clash Dashboard-- <br>
> 终端Cli：`scripts/clash_proxy-selector.sh`（使用脚本前，需要修改脚本中的 **Secret** 变量值为上述启动脚本输出的值，或与 `.env` 中定义的 **CLASH_SECRET** 变量值保持一致）

# 常见问题
1. 部分Linux默认 shell `/bin/sh` 被更改为 `dash`，运行脚本会出现报错（报错内容一般会有 `-en [ OK ]`）。使用 `bash xxx.sh` 运行 <br>
2. 部分用户在UI界面找不到代理节点，基本上是因为厂商提供的clash配置文件是经过base64编码的，且配置文件格式不符合clash配置标准。目前此项目已集成自动识别和转换clash配置文件的功能。如果依然无法使用，则需要通过自建或者第三方平台（不推荐，有泄露风险）对订阅地址转换 <br>
3. 桌面环境浏览器需在浏览器内设置代理 <br>
4. 因为ping使用ICMP协议，∴即使代理也不能ping通 <br>

# 代理故障检查思虑&解决方式
1. 修改start.sh中端口后source环境变量 <br>
2. 更换网络环境 <br>
3. 运行本项目建议使用root用户 或 sudo 提权 <br>
4. 检查是否正常运行 <br>
```bash
netstat -tln | grep -E '9090|789.' #检查端口
tcp   0  0  127.0.0.1:9090     0.0.0.0:*     LISTEN     
tcp6  0  0  :::7890            :::*          LISTEN     
tcp6  0  0  :::7891            :::*          LISTEN     
tcp6  0  0  :::7892            :::*          LISTEN
```
```bash
env | grep -E 'http_proxy|https_proxy' #检查环境变量(若proxy_off应回显为空)
http_proxy=http://127.0.0.1:7890
https_proxy=http://127.0.0.1:7890
```

# 免责声明
1.本项目使用GNU通用公共许可证（GPL）v3.0进行许可。您可以查看本仓库LICENSE进行了解<br>
2.本项目的原作者保留所有知识产权。作为使用者，您需遵守GPL v3.0的要求，并承担因使用本项目而产生的任何风险 <br>
3.本项目所提供的内容不提供任何明示或暗示的保证。在法律允许的范围内，原作者概不负责，不论是直接的、间接的、特殊的、偶然的或后果性的损害 <br>
3.本项目与仓库的创建者和维护者完全无关，仅作为备份仓库，任何因使用本项目而引起的纠纷、争议或损失，与仓库的作者和维护者完全无关 <br>
4.对于使用本项目所导致的任何纠纷或争议，使用者必须遵守自己国家的法律法规，并且需自行解决因使用本项目而产生的任何法律法规问题 <br>
