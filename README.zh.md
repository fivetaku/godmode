[English](README.md) | [한국어](README.ko.md) | 中文 | [日本語](README.ja.md) | [Español](README.es.md)

# godmode

<div align="center">
  <img src="assets/godmode-hero-01.png" width="860" alt="Mythic key-art: a lightning bolt strikes a Greek marble temple while a plasma loop orbits its columns; a lone developer with a laptop watches from the stairs below the carved GODMODE wordmark and a phosphor-green '> iddqd' cheat code">
</div>

```
> iddqd

GOD MODE ON.
无敌模式已激活。
你的想法在发布之前再也不会死掉。
```

> **把事情做完的作弊码。** 打开它，扔进一个想法，循环就不会停——直到文档自己说"完成了"。

每个项目的死法都一样：完成 80%，发布 0%。godmode 直接删掉"死掉"这个选项。它串联 访谈 → PRD → 目标设置 → `/goal` 执行，然后不断锤打 **验证 → 评审 → 改进** 循环，直到达成完成定义（DoD）——**所有 VALIDATION 项通过，且阻塞性假设为 0**。卡住时触发恢复阶梯，换方法而不是放弃；提前退出会被驱动守卫抓回循环里。

它不会问你要不要继续。这正是重点。

## 快速开始

### 1. 添加市场

```
/plugin marketplace add fivetaku/godmode
```

### 2. 安装

```
/plugin install godmode@godmode
```

### 3. 输入作弊码

```
/godmode 给我做一个我早退就羞辱我的番茄钟
```

就这样。做完了它会告诉你。（判定"完成"的是文档，不是感觉。）

## 无敌模式的规则

1. **完成是定义出来的，不是感觉出来的。** DoD = VALIDATION 全部通过 + 阻塞性假设为 0。写在文档里，由循环检查。
2. **绝不提前停止。** 卡住 → 重试 → 换更深的评审者改变方法 → 重新规划关卡。阶梯上没有"放弃"。
3. **微循环胜过一次到位。** 每个阶段以"够开始就行"快速通过，然后循环不断收紧质量。
4. **评审者可插拔。** self-review（默认）/ insane-review / codex / gjc——裁判由你选。

## 要求

- [Claude Code](https://claude.com/claude-code)
- 配合 [gptaku-plugins](https://github.com/fivetaku/gptaku_plugins) 管线（`show-me-the-prd`、`goaljaby`）效果最佳——安装后 godmode 会自动编排它们。

## 血统

godmode 是 [insane-loop](https://github.com/fivetaku/insane-loop) 的梗版——同一个引擎，更嚣张的态度。

## 许可证

MIT——见 [LICENSE](LICENSE) 与 [DISCLAIMER](DISCLAIMER.md)。无敌模式只是比喻，你仍是凡人，仍要对你发布的东西负责。
