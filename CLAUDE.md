# 占いの館 - CLAUDE.md

## プロジェクト概要

静的HTML（サーバー・API不要）で作った無料占いサイト。
完全にフロントエンドのみで動作する。コストはドメイン代のみ。

---

## ファイル構成

```
uranai/
├── index.html        # トップページ（占いの館）
├── seiza.html        # 星座×血液型占い（48パターン×3バリアント）
├── numerology.html   # 数秘術占い（運命数1-9, 11, 22, 33）
├── tarot.html        # タロット占い（大アルカナ22枚＋小アルカナ56枚）
├── favicon.svg       # サイトアイコン（三日月・星）
├── BUSINESS_GUIDE.md # ビジネス運用ドキュメント
└── BUSINESS_GUIDE.pdf
```

---

## デザイン規則

すべてのページで統一する。

- **背景色**: `#0a0a1a`（ダーク紺）
- **テキスト**: `#e9d5ff`（薄紫）
- **アクセント**: `#c084fc`（紫）／ `#a78bfa`（ラベンダー）
- **ボーダー**: `rgba(139, 92, 246, 0.3)`
- **グラデーション**: `linear-gradient(135deg, #f0abfc, #c084fc, #818cf8)`
- **フォント**: `'Hiragino Mincho ProN', 'Yu Mincho', serif`
- **背景演出**: ランダム生成の星（`<div class="star">`）80個をJSで生成
- **カード**: `border-radius: 16〜20px`、ホバーで `translateY(-6px)` + グロー
- **ボタン**: メインは `linear-gradient(135deg, #7c3aed, #5b21b6)`、角丸50px

新しいページを追加するときはこのスタイルを踏襲する。

---

## 各ページのデータ構造

### seiza.html

```js
const fortunes = {
  "血液型_星座名": {
    personality: "...",
    love: "...",
    work: "...",
    overall: "...",
    advice: "...",
    stars: 4,          // ★の数（1〜5）
    color: "...",      // ラッキーカラー
    item: "...",       // ラッキーアイテム
    number: 7,         // ラッキーナンバー
    weeklyPairs: [
      { overall, advice, stars, color, item, number }, // バリアント1
      ...                                               // バリアント2, 3
    ]
  }
}
// キー例: "A_おひつじ座", "B_かに座", "O_さそり座", "AB_うお座"
// 血液型: A / B / O / AB
// 星座: おひつじ座 おうし座 ふたご座 かに座 しし座 おとめ座
//        てんびん座 さそり座 いて座 やぎ座 みずがめ座 うお座
```

バリアントを追加する場合は `additionalPairs` オブジェクトに追記してマージロジックを使う。

### numerology.html

```js
const numerologyData = {
  1: { keyword, personality, love, work, weeklyPairs: [...] },
  // 1〜9, 11, 22, 33
}
```

運命数の計算：生年月日の全桁を合計 → 1桁になるまで繰り返す（11/22/33はマスターナンバーとして止める）。

### tarot.html

```js
const tarotCards = [
  {
    number: "0",           // 大アルカナは "0"〜"XXI"
                           // 小アルカナは "ワンドI"〜"ペンタKg" など
    name: "愚者",
    symbol: "🌟",          // 絵文字アイコン
    upright:  { keyword: "...", meaning: "..." },
    reversed: { keyword: "...", meaning: "..." }
  }
]
```

**スート記号**
- ワンド 🔥（火・情熱・行動）
- カップ 💧（水・感情・愛）
- ソード ⚔️（風・知性・真実）
- ペンタクル 🌿（地・物質・仕事）

スプレッド定義は `const spreads = { 1: {...}, 3: {...} }` に追加すれば自動対応。

---

## 新しい占いを追加するとき

1. 新しい `xxx.html` を作成（デザインは既存ページを参考）
2. `index.html` のカードグリッドにリンクを追加
3. 各占いページに `<a href="index.html">🔮 占いTOPへ戻る</a>` を設置

---

## 運用メモ

- ホスティング：Vercel（無料）推奨
- 収益化：Google AdSense → アフィリエイト（電話占い系）の順
- PDFの再生成：`npx md-to-pdf BUSINESS_GUIDE.md`
- 詳細なビジネス戦略は `BUSINESS_GUIDE.md` を参照
