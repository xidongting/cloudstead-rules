# us-mobile 覆盖与证据

覆盖Red Pocket、Tello、Ultra、T-Mobile网站/账户，并加入按资料核实的GSMA/AT&T与T-Mobile系通话/授权候选。**尚未做真实SIM/设备的注册、通话、短信或境外激活测试。** 代理线路、设备接管、运营商资格及E911等条件仍独立存在。

## 网站与账户

- Red Pocket官方登录/激活域redpocket.com，页面公开引用的精确资源主机dnc8pt3q0ulny.cloudfront.net。无共享CloudFront根域兜底。
- Tello官方tello.com网站及其账户/eSIM管理页面；My Tello App VoIP不能混同系统Wi-Fi Calling，KeepCalling API尚未获足够独立证据。
- Ultra官方域ultramobile.com、redirect.ultra.me；T-Mobile官方域t-mobile.com，由MIT上游限定段落/命名空间提取。

## 通话与授权，按用途记录

### Red Pocket GSMA / AT&T

精确主机：epdg.epc.att.net（ePDG）、sentitlement2.mobile.att.net（授权）、vvm.mobile.att.net（可视语音信箱辅助，不是核心语音网关）。设备厂商HPE Aruba官方识别文档列出这些主机；AT&T官方LAN/VPN说明提供IPsec背景。本次HPE正文直连403，只核对官方索引摘录，未取得运营商逐SIM验证。

社区Apache-2.0列表lilith-rong包含vmm.mobile.att.net，与官方设备资料vvm冲突；采用有官方依据的vvm，不复制未证实vmm、MMSC或广泛AT&T网段。该仓库未见NOTICE且条目逐项来源不明，本轮不作为自动分发源，也不把许可未知清单换成手写来源。

资料支持识别不等于Red Pocket GSMA必定使用这些端点；具体设备和运营商配置可能不同。未确认AT&T专用IP范围前不猜测兜底网段。

### T-Mobile / Ultra / Tello底层网络候选

从两个MIT源的明确美国/T-Mobile段，提取epdg.epc.mnc260.mcc310.pub.3gppnetwork.org、mnc240对应域、geo/depdg及上游实际出现的ss变体。范围精确到ePDG命名空间，不放行整个mcc310或3gppnetwork.org。组织/底层网络关系只支持调查路径，不证明所有MVNO与设备使用同一端点。

T-Mobile官方企业网络文档已读正文，分别描述：

| 目标范围 | 约束 | 用途/限制 |
| --- | --- | --- |
| 208.54.0.0/16 | UDP 500、4500 | WFC2 IKE认证/NAT-T加密流量 |
| 208.54.0.0/16 | TCP或UDP 5061 | 旧WFC1 SIP/TLS，明确保留legacy标签 |
| 66.94.0.0/19 | TCP 443、993 | 旧WFC1手机认证/可视语音信箱，不是全部网络流量 |

只按官方列出的CIDR+网络+端口组合构建条件。206.29.177.36的CRL用途有提及，但未取得明确端口依据，因此不加无约束IP；crl.t-mobile.com已在T-Mobile域内。

上述官方放行指南被用于界定我们的路由策略，不是运营商官方要求必须家宽的证明。域名匹配与受限IP回退取OR；不能给网站域加上UDP500/4500限制，也不能仅按端口代理所有目标。

## 上游与独立依据

- czy13724/Surge Rules/esimswificalling.list：MIT，唯一美国段至英国段；保留许可。仅声明的web/ePDG命名空间进入结果。
- ous50/Surge wifi-calling/us.conf：MIT，唯一T-Mobile段至Calling/RTP段；保留许可。不摄取全美GEOIP/UDP兜底。
- https://www.t-mobile.com/support/coverage/wi-fi-calling-on-a-corporate-network ：本轮直接读取，结构化IP谓词的依据。
- https://arubanetworking.hpe.com/techdocs/CLI-Bank/Content/instant/sh-wifical-dns-patrns.htm ：官方搜索摘录，AT&T精确主机依据。
- https://www.att.com/support/article/wireless/KM1114459/ ：官方搜索摘录，非完整当前参数复核。

所有独立补充有理由、证据URL、日期及服务标签；不公开激活码、账号或原始私人抓包。资料读取和真正设备验证严格区分。

## 验证方式

固定sing-box1.15 alpha编译、反编译检查。普通CLI match无法设置网络/端口，所以IP谓词另用云端隔离运行时：本机direct入站模拟目的地址/端口，命中与不命中都重写到127.0.0.1的不同回环服务，再断言响应。**不向生产运营商发测试包。** 包括IPv4/IPv6、范围内外、TCP/UDP、端口和覆盖指令逻辑。

产物manifest中rule_count/domain_count只计正向域名项；packet_rule_count表示含负向逻辑中的IP谓词数，不把一个网段算作一个域名。effective_rules是最终匹配语义，packet_evidence记录依据。

使用拆分或合一订阅之一。合一版是已应用覆盖后的分类表达OR；全局排除不能失效，分类级排除不误伤另一类。关键操作要零漏流仍须隔离网络全量固定出口与故障阻断，不能只靠有限名单。
