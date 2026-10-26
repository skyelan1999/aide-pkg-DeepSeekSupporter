# DeepSeekSupporter

Aide 的 DeepSeek 支持插件，MIT 许可；不是 DeepSeek 官方发布。需要支持 `providerSupport` protocol 1 的 Aide。安装并激活后由宿主读取声明档案，不注册 AI 工具，不向 JavaScript 提供模型 API Key。

仅匹配 HTTPS 官方端点 `api.deepseek.com`，路径为根路径、`/v1` 或 `/anthropic`。仅精确匹配档案中的四个模型名称，不按前缀猜测版本，不把代理服务或未知模型按官方价格计算；这些情况由 Aide 使用用户自定义费率。

档案为 2026-10-22 核验的人民币/百万 token 刊例价快照，包含 Flash 三个官方名称与 Pro、缓存命中输入费率和峰谷倍率。运行时不联网抓取价格；官方调价需要发布新档案。费用显示是估算，不能替代上游账单；优惠、赠送额度、批量折扣和其他产品费用不自动抵扣。Search/Fetch 独立计费未纳入 token 价格。

峰值时间为上海时区周一至周五 09:00–12:00、14:00–18:00，不含内置的 2026 放假日期；周末即使调休上班也按空闲时段处理。未来年份没有完整日历时，宿主仍按周末/小时规则估算，必须标记“估算”，不可宣称节假日准确。2026 日历沿用 Aide 原有公布放假区间，详见调研记录。

只读余额声明指向官方 `GET /user/balance`。宿主在用户请求时以已配置模型 Key 的 Bearer 身份查询，凭证由 Aide 的 vault 与服务端管理，不进入插件、工作区、日志或前端。未配置/查询失败不能伪装为零余额；保留上游币种。每个 Key/模型的费率覆盖和余额归属由宿主管理。

## 检查与本地打包

```sh
node --test test/catalog.test.cjs
python3 scripts/package.py
```

打包使用显式文件白名单、固定时间戳和 Unix 权限；ZIP 只包含档案、无工具入口、MIT 与说明文档。`dist/` 被 Git 忽略。正式安装包通过 [GitHub Release](https://github.com/skyelan1999/aide-pkg-DeepSeekSupporter/releases/tag/v0.1.0) 和 [官方插件市场](https://skyelan1999.github.io/aide-pkg-shop/) 分发；两处使用同一份 ZIP 和 SHA-256。测试不调用真实模型或余额接口，源码检查与真实账户验收分别记录。

来源及实现边界见 [调研记录](docs/RESEARCH-20261022.md)。

## 安装与版本 / Installation and release

在 Aide 的插件市场搜索 `DeepSeekSupporter` 或 `deepseek-supporter`，安装后启用；也可下载 Release 的 ZIP，在插件面板导入。需要 Aide 0.1.17.5 RC5 或更新版本中的 providerSupport protocol 1 适配。凭证和按 Key/模型的覆盖费率仍由宿主管理。

The same architecture-independent ZIP is distributed through GitHub Releases and the official marketplace. Install and enable it in Aide; Aide owns credentials, matching, pricing and balance requests. This MIT plugin is not an official DeepSeek product. Its offline tariff snapshot is dated 2026-10-22 and was checked against the official pricing page again on 2026-10-26. It does not automatically fetch new prices, cover Search/Fetch charges, or guarantee billing reconciliation. Unknown endpoints/models retain host-configured rates. No real account or paid request was used for this release's tests.

下载 `SHA256SUMS` 后可执行 `shasum -a 256 -c SHA256SUMS`。市场包与 Release 包的摘要应一致。
