# videonote-rules

帧记（星巢视频笔记）的**链接解析规则包**，供客户端热更新使用。

- `parser_rules.json` — 解析规则（抽取层字段 + 下载执行层策略 httpPolicy）
- `parser_rules.json.sig` — 对 `parser_rules.json` **原始字节**的 Ed25519 分离签名（base64）

客户端按「Gitee 主通道 → GitHub 备用通道」顺序拉取（主通道被平台拦截时自动切换），
按以下顺序校验，任一环节不过即拒绝应用并回退内置规则：

HTTPS → 取 `.sig` → Ed25519 验签 → JSON 解码 → schema 版本闸门
→ URL host 白名单 → canary 沙箱试跑 → 版本单调递增 → 文件大小上限 → 落缓存生效

**请勿手工编辑本仓库文件。** 规则由帧记项目内的
`Sources/Resources/rules/parser_rules.json` 经 `ReleaseKit/rules/publish_rules.sh`
（大小守卫 → 门禁 → 签名 → 双通道推送 → 回读复验）发布；手工改动会导致验签失败、整包被客户端拒收。
