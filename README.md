# CSP Screener

PutFinder-style **cash-secured put（现金担保看跌期权）每日榜单**：对股票池做期权链扫描，按"股票质量 × 合约适配度 × 财报门"打分排名。

## 看榜单

直接用浏览器打开 `index.html`（或启用 GitHub Pages 指向本仓库）。

## 数据更新

`data/results.json` 由筛选引擎生成：

```bash
cd ~/workspace/csp-screener
python3 screen.py --top 15 --out /tmp/results.json --workers 4
cp /tmp/results.json <本仓库>/data/results.json
```

JSON 格式：`{"as_of": "2026-10-04", "rows": [ … ]}`，每行一个合格合约，字段含
ticker / expiration / dte / strike / spot / bid / ask / mid / breakeven /
ann_yield / buffer / delta / iv / oi / volume / quality / trend / vrp /
stock_score / contract_fit / earn_gate / earnings / score。

## 评分（v2，已校准）

- **股票分** = 0.55×质量（分析师评级）+ 0.25×趋势（50日线位置）+ 0.20×VRP（IV30−HV30）
- **合约适配分** = 0.35×年化收益率分 + 0.65×下跌缓冲分
- **总分** = 股票分/100 × 合约适配分 × 财报门（财报在到期前 3 天内直接清零）

校准依据：PutFinder 11 天历史榜单（2026-09-17~10-01）的 14 个锚点合约全部自洽；
完整 93 标的实盘对照：Top-10 重合 4/10、前 20 重合 6/10（差异主要来自数据日期差）。

## 免责

仅供研究，不构成投资建议。期权卖方策略有本金损失风险。
