# zERC20 流動性・目標・手数料モニター

[zERC20](https://zerc20.io/) の各チェーン（Ethereum / Base / Arbitrum）の流動性をオンチェーン
（RPC）から取得し、**目標値（target）** と **wrap 報酬の概算** を表示する単一ファイルの Web ツールです。

`index.html` をブラウザで開くだけで動作します（ビルド不要）。

## 機能

- LiquidityManager の残高（zETH = native 残高 / zUSDC = `balanceOf`）を各チェーンの RPC から取得
- 目標 `T = 全チェーン合計 ÷ チェーン数 × 比率(既定 80%)` を計算し、チェーンごとに状態（報酬あり / ほぼ目標 / 過剰）を表示
- 入金額（ETH / USDC）を入力すると、全チェーンの **wrap 報酬・率・USD換算** を比較し最善チェーンを提示
- 比率・ETH 価格・入金額・自動更新（30/60/120 秒）を画面上で調整可能

## 計算式

```
wrap報酬 = ∫[L→min(L+D,T)] k·(1 − x/T) dx = k·[ dEff − ((L+dEff)² − L²)/(2T) ]   （L ≥ T なら 0）
目標 T   = 全チェーン合計 ÷ チェーン数 × 比率
k = 10%（zETH・zUSDC）
```

- 入金者が得られるのは **wrap 報酬のみ**。プライベート送金の **償還手数料 0.35%（最大 1%）はプロトコル収入**で入金者には入りません。
- wrap 報酬は過去の unwrap 手数料の余剰プールから支払われるため、余剰が枯渇していると満額もらえないことがあります。

## 公開（GitHub Pages）

1. このフォルダ（`index.html` / `README.md` / `LICENSE`）をリポジトリのルートに置く
2. **Settings → Pages → Source: `main` / `/ (root)`** を選択
3. `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開されます

## ライセンス

[MIT License](./LICENSE) © 2026 AkashiRiot

独自実装（UI / CSS / 目標・報酬計算ロジック）に適用されます。
自由に利用・改変・再配布できます（人間・AI を問わず歓迎します）。

## 出典・依存（すべて一次情報）

- **コントラクトアドレス（LiquidityManager / zERC20）**: zERC20 公式 docs — <https://zerc20.gitbook.io/zerc20/reference/addresses>
- **手数料・報酬モデル（k=10%, 償還 0.35%）**: zERC20 公式 docs — <https://zerc20.gitbook.io/zerc20/for-users/fees-and-rewards>
- **USDC / USDC.e トークンアドレス**: Circle 公式 — <https://www.circle.com/multi-chain-usdc>
- **ライブラリ**: [ethers.js](https://github.com/ethers-io/ethers.js) v6.13.4（MIT License, © Richard Moore）— cdnjs から SRI 付きで読み込み
- **RPC**: 各チェーンの公開エンドポイント（PublicNode / Ankr / 1RPC / Base 公式 / Arbitrum 公式）

> 免責: 本ツールは情報提供のみを目的とし、投資助言ではありません。数値は概算です。
> RPC のレート制限・データの正確性はご自身でご確認ください。
