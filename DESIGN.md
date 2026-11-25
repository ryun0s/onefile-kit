# DESIGN.md — OneFile Kit

```yaml
colors:
  paper: "#f4efe4"       # 作業紙の色
  ink: "#2e2a24"
  line: "#cfc4ab"
  accentHabits: "#3e7d4e"  # 習慣アプリの緑
  accentGarden: "#b4552d"  # 庭アプリの土の色
  accentKit: "#44546e"     # 紺（キット自体の色）
fonts:
  base: "system-ui, sans-serif"
  mono: "ui-monospace, 'SF Mono', monospace"
radius: { card: "10px" }
spacing: { scale: [4, 8, 14, 22, 34] }
```

## Signature（1つだけ）
「ファイルそのものが製品」。各アプリの画面隅に、そのファイル1つで動いていることを示す小さな印（ファイル名とバイト数のモノスペース表示）を置く。土台の上に乗るアプリは、色と中身だけが違い、骨格（頭・タブ・保存の作法）は同じだと見て分かる。

## やらないこと
- 紫グラデーション、ガラス質カード等の平均的なAIデザイン
- 2つのアプリを色違いの同じ画面にすること（操作の種類を変える）
