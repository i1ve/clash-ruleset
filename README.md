# 自定义 rule-set
# 一、 说明
1. 每天早上 3 点（北京时间）自动构建生成 networktest.yaml、google-cn.yaml 和 user.yaml
2. networktest.yaml 源采用 [blackmatrix7/ios_rule_script/Speedtest](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Speedtest) 和 [IPv6 测试网站](https://github.com/DustinWin/clash-ruleset/blob/main/rule-files/network.yaml)组合
3. google-cn.yaml 源采用 [rules.kr328.app/google@cn](https://rules.kr328.app/google@cn.yaml)（删除 `'+.googleapis.cn'`，以免直连时出现 [Google Play Store](https://play.google.com/store) 无法下载或升级应用的问题）
4. user.yaml  
① 添加[常用 fake-ip 地址过滤列表](https://github.com/juewuy/ShellClash/blob/master/public/fake_ip_filter.list)到 fake-ip-user.yaml 内的 `fake-ip-filter` 中，提高兼容性  
② 添加 [TrackersList](https://trackerslist.com) 到 fake-ip-user.yaml 内的 `fake-ip-filter` 中，防止 [BT 下载](https://github.com/c0re100/qBittorrent-Enhanced-Edition/releases)无法连接 TrackersList UDP 协议<img src="https://user-images.githubusercontent.com/45238096/224113233-4d76dec2-495c-4790-a00e-538fc1469639.png" width="60%"/>  
③ 若想自己生成配置文件 user.yaml，可以 [Fork 本项目](https://github.com/DustinWin/clash-ruleset/fork)后编辑 *.github/workflows/rule-set.yml* 文件内的 `name: Generate xxx-user.yaml` 部分
④ 若 DNS 模式选用的是 redir-host，需要进行 [DNS 分流](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20ruleset%20%E6%96%B9%E6%A1%88.md)，可以进入 *.github/workflows/rule-set.yml* 文件，编辑 `Generate redir-host-user.yaml` 部分，将 `nameserver` 中的 `🪜 代理域名`改成可以访问外网的代理组名，或者直接将 `'https://dns.google/dns-query#🪜 代理域名'` 修改为 `'tls://dns.google'`
# 二、 使用方法
在配置文件中新增如下内容：
- 注：以下只是节选，请酌情套用

```
proxy-groups:
  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🗽 Google 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

rule-providers:
  networktest:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml"
    path: ./ruleset/networktest.yaml
    interval: 86400

  google-cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml"
    path: ./ruleset/google-cn.yaml
    interval: 86400

rules:
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,google-cn,🗽 Google 中国
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
# 四、 推荐配置
- 注：以下只是节选，请酌情套用

```
rule-providers:
  reject:
    type: http
    behavior: domain
    url: 'https://cdn.jsdelivr.net/gh/privacy-protection-tools/anti-AD@master/anti-ad-clash.yaml'
    path: ./ruleset/reject.yaml
    interval: 86400

  applications:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt'
    path: ./ruleset/applications.yaml
    interval: 86400

  lan:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Lan/Lan_No_Resolve.yaml'
    path: ./ruleset/lan.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml'
    path: ./ruleset/networktest.yaml
    interval: 86400

  microsoft-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/microsoft@cn.yaml'
    path: ./ruleset/microsoft-cn.yaml
    interval: 86400

  apple-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/apple@cn.yaml'
    path: ./ruleset/apple-cn.yaml
    interval: 86400

  google-cn:
    type: http
    behavior: domain
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml'
    path: ./ruleset/google-cn.yaml
    interval: 86400

  games-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/category-games@cn.yaml'
    path: ./ruleset/games-cn.yaml
    interval: 86400

  proxy:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Proxy/Proxy_Classical.yaml'
    path: ./ruleset/proxy.yaml
    interval: 86400

  direct:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/ChinaMax/ChinaMax_Classical.yaml'
    path: ./ruleset/direct.yaml
    interval: 86400

rules:
  - RULE-SET,reject,⛔️ 广告域名
  - RULE-SET,applications,📥 下载软件
  - RULE-SET,lan,🏠 私有网络
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,microsoft-cn,Ⓜ️ Microsoft 中国
  - RULE-SET,apple-cn,🍎 Apple 中国
  - RULE-SET,google-cn,🗽 Google 中国
  - RULE-SET,games-cn,🎮 国区游戏
  - RULE-SET,proxy,🪜 代理域名
  - RULE-SET,direct,⚡ 直连域名
  - MATCH,🐟 漏网之鱼
```
