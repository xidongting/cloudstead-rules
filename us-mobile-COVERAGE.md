# us-mobile：网站／账户阶段

首版覆盖 Red Pocket、Tello、Ultra、T-Mobile 的网站与其域内账户/充值/eSIM管理页面。**不包含GSMA/AT&T或T-Mobile通话网关、IPsec、运营商网段；通话端点由后续阶段验证。** 用户Red Pocket是GSMA，不能因网站正常就宣称通话可用。

## 来源与自动提取范围

- czy13724/Surge 的 MIT 全球eSIM列表：严格定位唯一美国段落（美国标题至英国标题），仅提取 Ultra 官网和 redirect.ultra.me 命名空间的合法域名规则。国外其他段正常变化不会影响解析；美国段结构缺失/重复/顺序错误立即失败。美国段内其他通话、共享跟踪、关键词、IP规则记录为范围外，不放入本阶段网站集。
- ous50/Surge 的 MIT 美国列表：定位唯一T-Mobile段至Calling/RTP段，只摄取t-mobile.com命名空间。不直接接受SM-DP+主机与通话回退，保留后续核查。
- Red Pocket：官方登录/激活页面支持redpocket.com后缀；页面资源dnc8pt3q0ulny.cloudfront.net仅精确收录，不扩为共享CloudFront根域。
- Tello：官方站点、账户帮助、App说明支持tello.com后缀。未从无许可社区文件复制KeepCalling API目标；母公司关系不能单独证明必要API主机。

前两项固定commit下载、许可证hash检查，原MIT许可随产物分发。官网补充仅记录公共域名事实与URL，不复制文章内容或登录资料。来源metadata和补充记录保留服务用途，合并去重不抹掉来源。

后缀代表公开业务命名空间：会匹配新子域，也可能包含该运营商的非账户页面。无法按URL路径区分页面流量。

## 当前条目与缺口

初始6个域名值：redpocket.com、tello.com、ultramobile.com、redirect.ultra.me、t-mobile.com及上述Red Pocket精确资源主机。最终实际条目以构建manifest为准；不是六条主机就覆盖所有业务。

- 第三方支付/验证码/认证、App独立API和eSIM SM-DP+若不在这些命名空间，尚未确认覆盖。
- t-mobile.idemia.io、Tello KeepCalling API只作为候选，证据和适用流程不足前不声称已支持。
- 不加入Google Voice、整个Google/Apple、全球3gppnetwork、共享CDN根域或所有UDP端口兜底。
- 搜索结果可能含激活二维码/确认码，不下载或公开这些参数，仅使用去除个人信息的官方页面URL。
- 注册要求实际在美、蜂窝连接或设备条件时，美国IP不能替代这些条件；本规则是用户出口策略，不是官方家宽要求。

## 订阅方式

可选单独us-mobile.srs与us-bank.srs，也可只配置us-residential.srs。合一版是两个最终匹配表达的OR并集，分类级排除不会阻止另一类独立收录；全局指令在每类已应用，因此合一后不会失效。

整批构建任一来源/分类/测试失败都保留旧产物。健康检查基于提取范围，而非全球文件总行数；初始Ultra范围2条、T-Mobile1条，下限都为1且单次缩减超过50%失败，合法域内新增自动进入。

未进行真实设备登录、付款、下载eSIM或通话试验；不要把网站规则编译成功当成端到端可用。关键操作无漏流要求依旧需要隔离环境全量固定出口和网关故障阻断。
