[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | 日本語 | [Español](README.es.md)

# godmode

<div align="center">
  <img src="assets/godmode-hero-01.png" width="860" alt="Mythic key-art: a lightning bolt strikes a Greek marble temple while a plasma loop orbits its columns; a lone developer with a laptop watches from the stairs below the carved GODMODE wordmark and a phosphor-green '> iddqd' cheat code">
</div>

```
> iddqd

GOD MODE ON.
無敵モード発動。
あなたのアイデアはもう、リリース前に死ねません。
```

> **やり遂げるためのチートコード。** オンにしてアイデアを放り込めば、ドキュメント自身が「完成」と言うまでループは止まりません。

プロジェクトの死に方はいつも同じ：80% 完成、0% リリース。godmode は「死ぬ」という選択肢を消します。インタビュー → PRD → ゴール設定 → `/goal` 実行をつなぎ、完成定義（DoD）に到達するまで **検証 → レビュー → 改善** サイクルを回し続けます — **VALIDATION 全項目パス AND ブロッキング仮定 0**。停滞すれば、諦める代わりにアプローチを変えるリカバリーラダーが発動。早期終了はドリブンガードが捕まえてループに投げ返します。

続けるかどうかは聞きません。それがすべてです。

## クイックスタート

### 1. マーケットプレイスを追加

```
/plugin marketplace add fivetaku/godmode
```

### 2. インストール

```
/plugin install godmode@godmode
```

### 3. チートコードを入力

```
/godmode 早くやめたら私を叱ってくるポモドーロタイマーを作って
```

以上。完成したら教えてくれます。（「完成」を判定するのはドキュメントであって、気分ではありません。）

## ゴッドモードのルール

1. **完成は感覚ではなく定義。** DoD = VALIDATION 全項目パス + ブロッキング仮定 0。ドキュメントに書かれ、ループが検査する。
2. **早期終了はしない。** 停滞 → リトライ → より深いレビュアーでアプローチ変更 → 再計画ゲート。ラダーに「諦める」は無い。
3. **一発完成よりマイクロループ。** 各ステージは「始めるのに十分」で素早く通過させ、ループが品質を締め上げる。
4. **レビュアーは差し替え可能。** self-review（デフォルト）/ insane-review / codex / gjc — ジャッジはあなたが選ぶ。

## 必要なもの

- [Claude Code](https://claude.com/claude-code)
- [gptaku-plugins](https://github.com/fivetaku/gptaku_plugins) パイプライン（`show-me-the-prd`、`goaljaby`）があると最高 — 入っていれば godmode がオーケストレーションします。

## 血統

godmode は [insane-loop](https://github.com/fivetaku/insane-loop) のミーム版です — 同じエンジン、より図太い態度。

## ライセンス

MIT — [LICENSE](LICENSE) と [DISCLAIMER](DISCLAIMER.md) を参照。ゴッドモードは比喩です。あなたは依然として人間であり、リリースしたものに責任を負います。
