# 自定义 rule-set
# 一、 说明
## 1. rule-set 规则
① 规则参考 [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)，有如下分类：
```
  - RULE-SET,ads,⛔️ 广告域名
  - RULE-SET,applications,📥 下载软件
  - RULE-SET,lan,🏠 私有网络
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,microsoft-cn,Ⓜ️ Microsoft 中国
  - RULE-SET,apple-cn,🍎 Apple 中国
  - RULE-SET,google-cn,🗽 Google 中国
  - RULE-SET,games-cn,🎮 国区游戏
  - RULE-SET,proxy,🪜 代理域名
  - RULE-SET,cn,⚡ 直连域名
  - RULE-SET,telegramip,✈️ Telegram IP
  - RULE-SET,lanip,🏠 私有网络
  - RULE-SET,cnip,🇨🇳 国内 IP
```
② 每天早上 3 点（北京时间）自动构建生成  
③ `RULE-SET:ads` 源采用 [privacy-protection-tools/anti-AD](https://github.com/privacy-protection-tools/anti-AD)（anti-ad-clash.yaml）  
④ `RULE-SET:applications` 源采用 [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules/tree/release)（applications.txt）  
⑤ `RULE-SET:lan` 源采用 [v2fly/domain-list-community/private](https://github.com/v2fly/domain-list-community/blob/master/data/private)  
⑥ `RULE-SET:networktest` 源采用 [blackmatrix7/ios_rule_script/Speedtest](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Speedtest) 和 [IPv6 测试网站](https://github.com/DustinWin/clash-ruleset/blob/main/rule-files/network.yaml)组合  
⑦ `RULE-SET:microsoft-cn` 源采用 [rules.kr328.app/microsoft@cn](https://rules.kr328.app/microsoft@cn.yaml)  
⑧ `RULE-SET:apple-cn` 源采用 [rules.kr328.app/apple@cn](https://rules.kr328.app/apple@cn.yaml)  
⑨ `RULE-SET:google-cn` 源采用 [rules.kr328.app/google@cn](https://rules.kr328.app/google@cn.yaml)（删除 `- "+.googleapis.cn"`，以免直连时出现 [Google Play Store](https://play.google.com/store) 无法下载或升级应用的问题）  
⑩ `RULE-SET:games-cn` 源采用 [rules.kr328.app/category-games@cn](https://rules.kr328.app/category-games@cn.yaml)和 [blackmatrix7/ios_rule_script/SteamCN](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/SteamCN) 组合  
⑪ `RULE-SET:proxy` 源采用 [cokebar/gfwlist2dnsmasq](https://github.com/cokebar/gfwlist2dnsmasq) 生成的 [gfwlist](https://github.com/gfwlist/gfwlist)  
⑫ `RULE-SET:cn` 源采用 [blackmatrix7/ios_rule_script/ChinaMax](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/ChinaMax)（ChinaMax_Domain.yaml）  
⑬ `RULE-SET:telegramip` 源采用 [Telegram IP](https://core.telegram.org/resources/cidr.txt)  
⑭ `RULE-SET:lanip` 源采用 [blackmatrix7/ios_rule_script/Lan](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Lan)（IP 部分）  
⑮ `RULE-SET,cnip` 源采用 [blackmatrix7/ios_rule_script/ChinaMax](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/ChinaMax)（ChinaMax_IP.txt）、[17mon/china_ip_list](https://github.com/17mon/china_ip_list) 和 [gaoyifan/china-operator-ip](https://github.com/gaoyifan/china-operator-ip) 组合
## 2. user.yaml  
① 添加[常用 fake-ip 地址过滤列表](https://github.com/juewuy/ShellClash/blob/master/public/fake_ip_filter.list)到 fake-ip-user.yaml 内的 `fake-ip-filter` 中，提高兼容性  
② 添加 [TrackersList](https://trackerslist.com) 到 fake-ip-user.yaml 内的 `fake-ip-filter` 中，防止 [BT 下载](https://github.com/c0re100/qBittorrent-Enhanced-Edition/releases)无法连接 TrackersList UDP 协议  
<img src="https://user-images.githubusercontent.com/45238096/224113233-4d76dec2-495c-4790-a00e-538fc1469639.png" width="60%"/>  

③ 添加如下域名，以配合 AdGuardHome 作为下游来下载黑名单并解决右下角报错“检查更新失败”的问题：
```
    - 'static.adtidy.org'
    - 'adguardteam.github.io'
    - 'anti-ad.net'
```
④ 若想自己生成配置文件 user.yaml，可以 [Fork 本项目](https://github.com/DustinWin/clash-ruleset/fork)后编辑 *.github/workflows/rule-set.yml* 文件内的 `name: Generate xxx-user.yaml` 部分  
⑤ 若 DNS 模式选用的是 redir-host，需要进行 [DNS 分流](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20ruleset%20%E6%96%B9%E6%A1%88.md)，可以进入 *.github/workflows/rule-set.yml* 文件，编辑 `Generate redir-host-user.yaml` 部分，将 `nameserver` 中的 `🪜 代理域名`改成可以访问外网的代理组名，或者直接将 `'https://dns.google/dns-query#🪜 代理域名'` 修改为 `'tls://dns.google'`
# 二、 使用方法
在配置文件中新增如下内容：
- 注：以下只是节选，请酌情套用

```
proxy-groups:
  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: ⚡ 直连域名, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🪜 代理域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: 🎮 国区游戏, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: Ⓜ️ Microsoft 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🗽 Google 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🍎 Apple 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🇨🇳 国内 IP, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: ✈️ Telegram IP, type: select, proxies: [🚀 节点选择]}

  - {name: 📥 下载软件, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🏠 私有网络, type: select, proxies: [🎯 全球直连]}

  - {name: ⛔️ 广告域名, type: select, proxies: [🛑 全球拦截]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  - {name: 🛑 全球拦截, type: select, proxies: [REJECT]}


rule-providers:
  ads:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/ads.yaml"
    path: ./ruleset/ads.yaml
    interval: 86400

  applications:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/applications.yaml"
    path: ./ruleset/applications.yaml
    interval: 86400

  lan:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/lan.yaml"
    path: ./ruleset/lan.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml"
    path: ./ruleset/networktest.yaml
    interval: 86400

  microsoft-cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/microsoft-cn.yaml"
    path: ./ruleset/microsoft-cn.yaml
    interval: 86400

  apple-cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/apple-cn.yaml"
    path: ./ruleset/apple-cn.yaml
    interval: 86400

  google-cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml"
    path: ./ruleset/google-cn.yaml
    interval: 86400

  games-cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/games-cn.yaml"
    path: ./ruleset/games-cn.yaml
    interval: 86400

  proxy:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/proxy.yaml"
    path: ./ruleset/proxy.yaml
    interval: 86400

  cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/cn.yaml"
    path: ./ruleset/cn.yaml
    interval: 86400

  telegramip:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/telegramip.yaml"
    path: ./ruleset/telegramip.yaml
    interval: 86400

  lanip:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/lanip.yaml"
    path: ./ruleset/lanip.yaml
    interval: 86400

  cnip:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/cnip.yaml"
    path: ./ruleset/cnip.yaml
    interval: 86400

rules:
  - RULE-SET,ads,⛔️ 广告域名
  - RULE-SET,applications,📥 下载软件
  - RULE-SET,lan,🏠 私有网络
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,microsoft-cn,Ⓜ️ Microsoft 中国
  - RULE-SET,apple-cn,🍎 Apple 中国
  - RULE-SET,google-cn,🗽 Google 中国
  - RULE-SET,games-cn,🎮 国区游戏
  - RULE-SET,proxy,🪜 代理域名
  - RULE-SET,cn,⚡ 直连域名
  - RULE-SET,telegramip,✈️ Telegram IP
  - RULE-SET,lanip,🏠 私有网络
  - RULE-SET,cnip,🇨🇳 国内 IP
```
# 三、 导入 [Clash Verge](https://github.com/zzzgydi/clash-verge)（Windows 端）
1. 首次使用可进入 Clash Verge->配置，新建“Merge”类型的配置，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”
2. 进入文件夹 *%USERPROFILE%.config\clash-verge\profiles*，找到与第 1 步新建的 Merge 文件相对应的 .yaml 文件，复制其文件名并替换下面命令中的{文件名}；将下面命令中的{DNS 模式}替换为正在使用的 DNS 模式（fake-ip 或 redir-host）  
以管理员身份打开 CMD 命令提示符，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %USERPROFILE%\.config\clash-verge\profiles\{文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/{DNS 模式}-user.yaml
```
