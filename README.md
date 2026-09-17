# 文脉精选模型清单

文脉（Wenmai）写作应用运行时拉取的**精选文本模型清单**。本仓只承载这一份数据，代码不在这里。

## 消费方

文脉主进程按固定 raw URL 拉取本仓的 [`catalog/text-llm-manifest.json`](catalog/text-llm-manifest.json)
（常量定义见 monorepo `packages/meridian-ai/src/model-manifest.ts` 的 `TEXT_LLM_MANIFEST_URL`）。
拉取失败时应用回退到本地缓存，再回退到打包内置快照，因此本仓短暂不可用不会影响使用。

## 事实源与更新方式

唯一事实源是 `yuanliu` monorepo 里的 `packages/meridian-ai/catalog/text-llm-manifest.json`。
本仓内容由 `scripts/publish-model-catalog.mjs` 从该处发布（临时浅克隆 → 同步 → 按
`manifestVersion` 提交推送）。**请勿在本仓直接修改文件**——改动会随下次发布被覆盖。

## 结构

```jsonc
{
  "schemaVersion": 1,
  "catalog": "text-llm",
  "manifestVersion": "YYYY-MM-DD.N",        // 每次发布递增，应用侧据此判断新鲜度
  "providers": {
    "<providerId>": {
      "label": "展示名",
      "strategy": "curated",
      "models": [
        { "id": "模型 id", "role": "旗舰", "defaultVisible": true }
      ]
    }
  }
}
```

- `role`：展示用中文标签，沿用现有词汇（旗舰、推理、日常、快速、长文本、中文、编程、视觉、自动路由）。
- `defaultVisible`：新用户默认是否可见；未标注即默认隐藏，用户可在设置里勾选。
- 只收录对话模型；日期快照与试水 preview 不进清单。
