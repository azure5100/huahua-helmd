# 来源与衍生声明 / Provenance Notice

## 上游

```
包名    @adwmc/helm-d
版本    0.4.3
来源    本地 tarball 分发（adwmc-helm-d-0.4.3.tgz）
仓库    未声明（package.json 的 repository / homepage 均为 null）
许可    ⚠️ 未声明
```

## ⚠️ 许可状态提示

上游包 `@adwmc/helm-d` **没有声明任何开源许可证**，`package.json` 里 `license` 字段为 `null`，包内也没有 LICENSE 文件。

按著作权法默认规则，**未声明许可 = 保留所有权利**。因此：

| 内容 | 许可 |
|---|---|
| huahua 新增的文档（README / USAGE / NOTICE / 品牌资产） | MIT |
| 上游代码与 `references/` 知识库内容 | **状态未明，再分发前请先取得上游授权** |

**如果本仓库要长期公开分发**，建议先做以下之一：

```
1. 联系上游作者取得书面授权（最稳妥）
2. 改以「仅个人/内部使用」方式分发，不公开仓库
3. 只用自己写的内容重建知识库，不沿用上游文件
```

## 本仓库做了什么

| 类别 | 改动 |
|---|---|
| 包身份 | `@adwmc/helm-d` → `huahua-helmd`；`cordis.patch.yml` 与 `preset.generated.patch.yml` 的 `name:` 同步；client loader id 同步 |
| 文档 | 新增中文 README、英文 README、使用手册、来源声明 |
| 品牌资产 | 新增 `assets/icon.svg`（七色光花盾）、`assets/banner.svg` |
| 代码 | **未改动**任何逻辑（dist/ 与 references/ 原样保留，561 个文件全在） |

## 与上游并存

`huahua-helmd` 与 `@adwmc/helm-d` 是两个不同包，插件 id 不重叠（`helmd-*` vs 上游同样的 id —— 注意：**本衍生版与上游的 id 相同**，所以两者不能同时安装，会重复注册同一批行）。
