# Component Ablation Summary

All four component variants use the same 5 bps L1 turnover transaction-cost assumption and the same 2022+ test period.

| Step | Strategy | Sharpe | Max Drawdown | Final Equity | Annualized Turnover |
| --- | --- | ---: | ---: | ---: | ---: |
| 1. No retrieval | mamba_only | 0.216 | -0.268 | 1.131 | 8.889 |
| 2. Raw KNN retrieval | raw_knn_retrieval_only | 0.607 | -0.145 | 1.361 | 50.278 |
| 3. Learned embedding retrieval | embedding_retrieval_only | 0.345 | -0.277 | 1.248 | 22.545 |
| 4. Core-satellite Mamba-RAG | mamba_rag_core_satellite | 0.374 | -0.202 | 1.173 | 14.703 |

## Interpretation

1. **No retrieval (`mamba_only`)** isolates the temporal prediction signal. It participates heavily in risky assets but has no historical analogue check, so it is the reference point for the component deltas.

2. **Raw KNN retrieval** tests whether classical pattern matching alone helps. In this run it improves Sharpe by 0.392 versus no retrieval and reduces max drawdown by 0.123. This says simple market-memory evidence can be useful, but it still uses a weak raw representation.

3. **Learned embedding retrieval** uses the temporal encoder embedding for neighbors. Its Sharpe delta versus no retrieval is 0.130. This is the direct test of whether the learned market-state representation is better than model-only trading; in this run it improves over no retrieval but does not beat raw KNN.

4. **Core-satellite Mamba-RAG** adds the realistic allocation layer: it keeps a defensive core and applies model/retrieval tilts as a satellite. Relative to no retrieval, Sharpe changes by 0.159, max drawdown changes by 0.066, and annualized turnover changes by 5.815. The key interpretation is risk-control and participation: the strategy avoids the old hard-abstention failure mode while making the AI overlay less aggressive than pure model trading.

## Takeaway

The component ablation does not show that every added component monotonically improves return. Instead, it shows the tradeoff: raw retrieval was the strongest standalone retrieval signal in this historical split, learned embedding retrieval made the neural signal auditable, and core-satellite allocation converted the signal into a more realistic portfolio construction rule. This is a historical backtest, not a live trading recommendation.
