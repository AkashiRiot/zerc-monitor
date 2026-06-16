# zERC20 流動性・手数料モニター（オンチェーン実値）

[zERC20](https://zerc20.io/) の各チェーン（Ethereum / Base / Arbitrum）について、流動性と
**wrap 報酬（入金して得られる手数料）／ unwrap 手数料** を表示する単一ファイルの Web ツールです。

報酬・手数料は推定式ではなく、LiquidityManager コントラクトの **view 関数を直接呼び出した実値**を表示します。

`index.html` をブラウザで開くだけで動作します（ビルド不要）。

## 機能

- 各チェーンの RPC から流動性を取得（zETH = LM の native 残高 / zUSDC = USDC の `balanceOf`）
- 入金額（ETH / USDC）を入力すると、各チェーンの LiquidityManager に対し
  - `quoteWrapReward(amount)` → **wrap 報酬（実値）**
  - `quoteUnwrapFee(amount)` → **unwrap 手数料（実値）**

  を呼び出し、率・USD換算とともに表示。最善チェーンを提示します。
- ETH 価格は公開 API から**自動取得**（Coinbase →失敗時 CoinGecko）。チェックを外せば手入力も可能
- 入金額・自動更新（30/60/120 秒）を画面上で調整可能

## 仕組み・注意

- **wrap 報酬**は、流動性が目標を下回るチェーンに入金したときのみ発生します。報酬は「目標までの不足分」に対してのみ付くため、流動性が目標に達していると入金額を増やしても頭打ち（または 0）になります。
- プライベート送金の**償還手数料（既定 0.35%、最大 1%）はプロトコル収入**で、入金者（wrapper）には入りません。
- USD 換算は USDC=$1、ETH=入力値で計算します（あくまで目安）。

> 旧バージョンは目標値を「合計 ÷ チェーン数 × 80%」と仮定した推定式で計算していましたが、
> 実測との誤差が大きかったため、コントラクトの実値（`quoteWrapReward` / `quoteUnwrapFee`）に変更しました。

## 公開（GitHub Pages）

1. このフォルダ（`index.html` / `README.md` / `LICENSE`）をリポジトリのルートに置く
2. **Settings → Pages → Source: `main` / `/ (root)`** を選択
3. `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開されます

## ライセンス

[MIT License](./LICENSE) © 2026 AkashiRiot

独自実装（UI / CSS / データ取得ロジック）に適用されます。
自由に利用・改変・再配布できます（人間・AI を問わず歓迎します）。

## 出典・依存（すべて一次情報）

- **コントラクトアドレス / 関数仕様（LiquidityManager, `quoteWrapReward` / `quoteUnwrapFee`）**: zERC20 公式 docs
  — <https://zerc20.gitbook.io/zerc20/reference/addresses> / <https://zerc20.gitbook.io/zerc20/for-developers/specs/contract-spec>
- **手数料・報酬の考え方（償還 0.35% 等）**: zERC20 公式 docs — <https://zerc20.gitbook.io/zerc20/for-users/fees-and-rewards>
- **USDC トークンアドレス**: Circle 公式 — <https://www.circle.com/multi-chain-usdc>
- **ライブラリ**: [ethers.js](https://github.com/ethers-io/ethers.js) v6.13.4（MIT License, © Richard Moore）— cdnjs から SRI 付きで読み込み
- **RPC**: 各チェーンの公開エンドポイント（PublicNode / Ankr / 1RPC / Base 公式 / Arbitrum 公式）
- **ETH 価格 API**: [Coinbase](https://api.coinbase.com/v2/prices/ETH-USD/spot)（主）/ [CoinGecko](https://www.coingecko.com/)（予備）

> 免責: 本ツールは情報提供のみを目的とし、投資助言ではありません。
> RPC のレート制限・データの正確性はご自身でご確認ください。
