# huahua-helmd 使用手册

> 适用版本：0.4.3 · 平台：DSH ≥ 0.2.1-rc.2

## 一、装完先验四件事

```bash
# ① 依赖写进 profile
#    <DSH_HOME>/profiles/<profile>/package.json → dependencies["huahua-helmd"]

# ② bundle 挂上
#    同一文件 → dsh.profile.bundles 里应有 "huahua-helmd"

# ③ patch 文件到位（两个）
#    node_modules/huahua-helmd/cordis.patch.yml
#    node_modules/huahua-helmd/preset.generated.patch.yml

# ④ 重启宿主（Ctrl+R 不够）
```

缺任何一样，插件都不会挂载。

## 二、知识库怎么用

### 检索

直接问模型，例如：

```
SQL 注入的二次注入怎么判定
反弹 shell 有哪些不提权的方式
Android 应用的证书固定怎么绕过
提示注入在工具调用链里怎么防
```

模型会去 `references/` 对应域找资料。

### 目录导航

```
references/index.md         总索引（先看这个）
references/web/             Web 漏洞（全量手法 + 判定 + 绕过）
references/protocol/        协议层（反弹 shell、未授权服务矩阵）
references/android/         移动端
references/native/          二进制
references/malware/         样本分析
references/ai-security/     模型与智能体安全
references/evidence/        取证与证据链
references/toolbox/         工具速查
```

### 加自己的资料

按域放进对应目录，`.md` 即可。建议每个文件顶部写一行用途说明，方便检索命中。

## 三、H-CoT 设置

```
设置 → H-CoT
```

这个面板走独立命名空间（`helmd-hcot-settings`），和主行分开注册——所以它能单独开关，不会因为主插件某次 apply 失败而一起失效。

## 四、健康自检

插件主入口就是健康检查（`dist/health.js`）。用途：

```
· 确认 cordis.patch.yml 两条行都 apply 成功
· 确认 peer 依赖齐全
· 确认 preset.generated.patch.yml 生成的预设是否有效
```

如果设置页打不开，先看宿主控制台里 `helmd-health` 的输出。

## 五、预设（helmd）

`presets/preset.yml` + `preset.generated.patch.yml` 会生成一个名为 `helmd` 的 agent 预设：

```
name        helmd
description anchored bootstrap plus all helmd security domain bundles
order       10
```

**⚠️ 注意**：预设是 patch 生成的，不一定会落成磁盘目录（`<DSH_HOME>/.agent-presets/helmd/`）。判断方法：

```
看 <DSH_HOME>/.agent-presets/ 下有没有 helmd/ 目录
没有 ⇒ 预设未落盘，界面里看不到，但知识库能力照常可用
```

要重新生成预设：

```bash
node node_modules/huahua-helmd/scripts/gen-preset.mjs
```

## 六、常见问题

### Q1：杀软报毒

`references/` 里有 payload 代码样例。处理三选一：

```
A. 给 references/ 加排除（推荐）
B. 把 .md 里的 payload 换成占位符（丢样例、留方法论）
C. 只保留自己需要的域，删掉高危域的目录
```

### Q2：装了没反应

```
1. bundles 里有 huahua-helmd 吗
2. 两个 patch 文件都在吗
3. 重启过宿主吗
4. 控制台有 patch apply 报错吗
```

### Q3：和 dsh-purge / huahua-purge 冲突吗

**不冲突**：插件 id 是 `helmd-health` / `helmd-hcot-settings`，与 `*-purge-*` 零重叠；两边都只引用官方 `@deepseek-ai/*` 包，互不依赖。

但注意：purge 会改全局 `system-prompt`（清身份前缀），而本插件不改——所以两者同时装时，purge 的清洗对 helmd 预设拉起的会话同样生效。

### Q4：能不能只装知识库不要界面

可以。把 `cordis.patch.yml` 里的 `helmd-hcot-settings` 那一行删掉（或加 `disabled: true`），只留 `helmd-health` 行。

## 七、卸载

```bash
dsh plugin --profile desktop remove huahua-helmd
# 重启宿主
# 想保留知识库：先 copy 出 references/ 再卸
```
