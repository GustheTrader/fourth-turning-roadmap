# Product Requirements Document (PRD)
## QuantumTruths: Prediction Market Agent Platform
**Version:** 1.0
**Date:** 2026-03-28
**Owner:** Hefe#2
**AI Partner:** QuintontheTruthTeller

---

## 1. Executive Summary

QuantumTruths is a prediction market agent platform built on Active Inference (AIF), Bayesian Causal Graphs, and Combinatorial Arbitrage. The system creates a high-performance trading agent that balances epistemic exploration (finding alpha in undertraded contracts) with instrumental exploitation (maximizing P&L).

**Key differentiators:**
- Graph-attention pre-filtering that fixes the 62% failure rate of naive LLM-detected arbitrage
- 43ms execution edge on Strix Halo cluster hardware
- Target: 35% Kelly Growth rate
- Cross-venue synthesis: prediction markets ↔ options/futures arbitrage

---

## 2. Problem Statement

Prediction markets are surging — from ~$9B annual volume in 2024 to $44B+ in 2025. Polymarket and Kalshi form a duopoly ($21.5B and $17.1B respectively). Yet current approaches have critical gaps:

- **Simple arbitrage bots** extract ~$39.5M but only capture the obvious (YES + NO ≠ $1.00)
- **LLM-detected combinatorial arbitrage** fails 62% of the time due to liquidity asymmetry, non-atomic execution, and oracle risk
- **No one has closed the causal graph loop** — market prices → causal DAG update → KB → attention rerouting → trades → market prices

**The opportunity:** A system that uses graph architectures to fix the failure modes and extract the semantic relationship edge (60-70% accuracy, ~20% weekly returns in backtest).

---

## 3. Core Architecture: 5-Layer Graph Data Pipeline

### Layer 0: Data Ingestion ("Data Lego")
- Kafka streams from Polymarket, Quantower, news APIs, social feeds
- Real-time on-chain data (order flow, contract resolutions)
- Off-chain signals: Reuters, social media, expert communities
- Structured and unstructured data normalization

### Layer 1: Knowledge Graph Construction
- Entity extraction and relationship mapping
- Bi-temporal model: valid time + transaction time
- Nodes = contracts, events, agents, sources, assets
- Edges = causal dependencies, correlations, semantic relationships
- Self-organizing: causal edges inferred from price co-movements (Bayes Labs APMM approach)

### Layer 2: Graph Attention & Precision Weighting
- Graph Attention Networks (GATs) for dynamic neighbor weighting
- Precision weighting = source credibility scoring
- High-precision: Reuters, calibrated superforecasters, verified sources
- Low-precision: anonymous social media, unverified feeds
- Pre-filters arbitrage pairs by minimum liquidity threshold
- Tracks multi-hop semantic dependencies (e.g., "Fed rate" → "tech M&A volume")

### Layer 3: Active Inference Decision Engine
- Free Energy Principle (FEP) as native agent architecture
- Expected free energy G(π) decomposes into:
  - **Epistemic value:** seek undertraded, information-sparse contracts
  - **Instrumental value:** exploit toward profitable outcomes
- Trade = act of inference (moves prices to match beliefs)
- Policy evaluation: simulate buy/sell/hold/hedge, compute G(π)
- Bayesian belief hierarchy for second-order arbitrage detection
- Kelly criterion position sizing (target: 35% Kelly Growth)

### Layer 4: Execution & Multi-Agent Coordination
- **OpenFang Rust Orchestrator** — ultra-low-latency execution engine
- Strix Halo cluster: 43ms execution edge
- **Hermes agent framework** — cognitive controller, sub-agent spawner
- MAGIC/MARL sub-agent coordination for atomic cross-market execution
- Each sub-agent owns one leg of a combinatorial trade
- Attention graph synchronizes execution timing across legs
- Draft-and-Prune verification: generate plans, filter contradictions before capital commitment

---

## 4. Key Capabilities

### 4.1 Arbitrage Detection & Execution

| Strategy | Current Performance | Our Target | How |
|---|---|---|---|
| Simple sum-price | ~$39.5M extracted | Maintain | Bot-speed execution |
| Combinatorial (graph-filtered) | ~$95K, 38% success | 70%+ success | Attention pre-filter + MARL |
| Semantic relationship | 60-70% accuracy | 75%+ | KG + multi-hop reasoning |
| Cross-venue (PM ↔ options/futures) | Unexploited | Primary edge | Unified KB across venues |

**Failure mode fixes:**
1. Liquidity asymmetry → attention graph pre-filters by minimum depth
2. Non-atomic execution → MARL sub-agent coordinated multi-leg execution
3. Oracle risk → social graph credibility discounting on resolution sources
4. LLM false positives (99.2% raw) → KB consistency checking before capital commitment

### 4.2 Cross-Venue Synthesis
- Map prediction market contracts to equivalent options/futures positions
- "Fed cuts ≥ 2 times in 2026" ≈ long bond futures (ZB/ZN)
- "S&P 500 above 6,000 by Dec 2026" ≈ long call with defined expiry
- Detect mispricing between venues → route execution to optimal leg
- Prediction market = high-resolution qualitative signal; derivatives = liquidity depth + tail risk hedge

### 4.3 Truth Extraction
- "Follow the money" — agents that consistently profit demonstrate revealed signal quality
- Bayesian markets: only accurate information holders profit in equilibrium
- Causal graph self-calibrates over time (surviving structure = correct model)
- Second-order belief tracking: detect when crowd's meta-belief hasn't incorporated specific information

---

## 5. Technical Requirements

### 5.1 Infrastructure
- **Compute:** Strix Halo cluster (AMD) — Hetzner or equivalent
- **Latency target:** 43ms execution edge
- **Stream processing:** Kafka for real-time data ingestion
- **Graph database:** Neo4j, TigerGraph, or custom (TBD based on scale testing)
- **ML framework:** JAX-compatible FEP modules for active inference
- **Execution engine:** Rust-based (OpenFang) for sub-millisecond order placement

### 5.2 Data Sources
- Polymarket API (contracts, prices, order flow, resolutions)
- Quantower (options/futures data)
- On-chain data (EVM-compatible chains)
- News APIs (Reuters, Bloomberg terminal if available)
- Social signals (X/Twitter, Reddit, Discord)
- Oracle feeds (UMA, Chainlink)

### 5.3 Performance Targets
- Kelly Growth Rate: 35%
- Combinatorial arbitrage success rate: 70%+ (vs. 38% baseline)
- Semantic relationship detection accuracy: 75%+ (vs. 60-70% baseline)
- False positive rate on capital-committed trades: <5%
- Execution latency: <50ms end-to-end

### 5.4 Reliability
- Oracle credibility scoring before capital commitment
- Draft-and-Prune verification on all combinatorial positions
- Sub-agent health monitoring and failover
- Position limits per contract/venue
- Circuit breakers on anomalous loss patterns

---

## 6. Integration Points

### 6.1 Hermes Agent Framework
- Cognitive controller and sub-agent spawner
- Spawns isolated sub-agents for parallel workstreams
- Self-improving learning loop (outcomes update model parameters)
- Manages the pre-game grind (instrumental value maximization)

### 6.2 OpenFang Rust Orchestrator
- Ultra-low-latency execution engine
- Handles in-game/logarithmic velocity trading
- Multi-leg atomic-ish execution via coordinated sub-agents
- Deployed on Strix Halo edge cluster

### 6.3 Abacus.ai Sidekick (TBD)
- Partner agent in same sandbox
- Role and capabilities to be defined during integration

---

## 7. Phased Delivery

### Phase 1: Foundation (Weeks 1-4)
- Data ingestion pipeline (Kafka + Polymarket + Quantower)
- Basic knowledge graph construction
- Simple sum-price arbitrage bot (capture the easy $39.5M pattern)
- Hermes agent basic integration

### Phase 2: Graph Intelligence (Weeks 5-8)
- GAT implementation for attention/pricing weighting
- Causal DAG construction from price co-movements
- Combinatorial arbitrage with attention pre-filtering
- Liquidity-aware position sizing

### Phase 3: Active Inference Engine (Weeks 9-12)
- FEP-based decision engine
- Kelly criterion optimization
- Second-order belief tracking
- Social graph credibility scoring for oracle risk

### Phase 4: Cross-Venue & Production (Weeks 13-16)
- Options/futures venue integration
- Cross-venue synthetic position construction
- OpenFang execution engine deployment
- Full MARL multi-agent coordination
- Production hardening and monitoring

---

## 8. Risks & Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Oracle manipulation (UMA-style attack) | High — $7M precedent | Social graph credibility scoring + position limits |
| Liquidity drying up on target contracts | Medium — kills combinatorial edge | Attention pre-filter enforces minimum depth |
| Regulatory changes to prediction markets | Medium — landscape improving | Polymarket got CFTC approval (Dec 2025) to re-enter US; still monitor state-level friction |
| Latency spikes on execution | Medium — loses edge | Strix Halo dedicated hardware; Rust-based execution |
| LLM false positives on semantic detection | Medium — wasted capital | KB consistency check + Draft-and-Prune before commitment |
| Overfitting on backtest results | High — paper vs. reality | Out-of-sample validation; conservative Kelly fractions |

---

## 9. Success Metrics

- **P&L:** Monthly positive returns with <15% max drawdown
- **Sharpe ratio:** Target >1.5 (graph-attention MARL benchmarks show 1.34-3.11)
- **Volume:** Capture meaningful % of available arbitrage surface
- **Accuracy:** Combinatorial success rate >70%, semantic detection >75%
- **Latency:** Consistent <50ms execution on all legs
- **Uptime:** 99.9% agent availability during active market hours

---

## 10. Open Questions

1. **Graph database choice:** Neo4j vs. TigerGraph vs. custom — depends on scale and query patterns
2. **Abacus.ai sidekick integration:** What capabilities does it bring? How does it coordinate?
3. **Capital allocation strategy:** How to distribute across simple, combinatorial, semantic, and cross-venue strategies
4. **Regulatory compliance:** Polymarket received CFTC approval (Dec 2025) to resume limited US operations through a registered intermediary — reporting, surveillance, recordkeeping, and customer protections required. Kalshi also won court battle for political event contracts. The federal path is now defined. State-level friction remains for sports/gaming overlap. Key question: are we building an exchange (requires DCM registration) or operating as a trading agent on existing platforms (lower burden)?
5. **Risk limits:** Per-strategy, per-venue, and portfolio-level drawdown limits

---

*This PRD is a living document. Update as architecture decisions are made and Phase 1 ships.*
