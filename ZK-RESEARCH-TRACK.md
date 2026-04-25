# Patronaige Research Track: ZK-Attested Inference

**Goal:** Replace TEE receipts with zero-knowledge proofs that prove correct inference without any trusted hardware.

**Why ZK:** ultimate trustlessness — security based on cryptography, not hardware vendors.

---

## State of the Art (mid-2025)

- **Lagrange DeepProve-1** — first full LLM inference proof (GPT-2). Supports GGUF, transformer layers. Proof generation: minutes to hours on high-end servers.
- **zkLLM** — earlier work focusing on arithmetic circuits for attention, softmax. Smaller models only.
- **Proof systems:** STARKs (large proofs, fast prover), SNARKs (small proofs, slower prover), Basefold (folding scheme reducing memory).

---

## Challenges

1. **Prover time** — Current ZK for LLMs is too slow for production (minutes-hours). We need <30 seconds for practical use.
2. **Circuit per model** — Each architecture requires custom circuit. Supporting arbitrary GGUF models is unrealistic; we'd limit to a whitelist (Llama, Gemma, etc.).
3. **Memory footprint** — Prover needs hundreds of GB RAM for large models. Quantization helps (INT4/INT8).
4. **Batching** — Must combine many inferences into one proof to amortize cost. Need efficient aggregation.

---

## Proposed Research Agenda (6–24 months)

### Milestone 1: PoC with Tiny Model (Months 1-3)
- Target: Llama 3.2 1B (quantized INT4).
- Use existing zkLLM or Lagrange framework.
- Demonstrate proof generation <5 min on a single server (512GB RAM).
- Verification on-chain (testnet) <1M gas.

### Milestone 2: Batching Protocol (Months 4-9)
- Design receipt aggregation: 100+ inferences → one proof.
- Merkleize receipt set; prove inclusion of specific receipt.
- On-chain batch verification contract (validity rollup pattern).

### Milestone 3: Optimize & Scale (Months 10-18)
- Explore custom gates for attention/layernorm to reduce circuit size.
- Investigate GPU-accelerated proving (CUDA prover).
- Target models up to 7B parameters with quantization.

### Milestone 4: Production Integration (Months 19-24)
- Dual-track: TEE for speed, ZK for high-value contracts.
- Build ZK provider SDK; integrate with existing Patronaige provider daemon.
- Define fallback: if ZK proof generation fails, fall back to TEE receipt.

---

## Go/No-Go Criteria

**Continue if:**
- Proof generation for 1B model <5 min on available hardware by Milestone 1.
- Proof size <1MB (so anchoring is cheap).
- At least one verifier implementation (on-chain or validity rollup) is viable.

**Stop if:**
- Prover times remain >30 min even for 1B models after 12 months.
- Proof size >10MB (anchoring becomes too costly).
- No clear path to multi-arch support without hand-crafted circuits per model.

---

## Resources Needed

- 2-3 ZK circuit engineers (or partnership with ZKML startup)
- High-memory compute (512GB–1TB RAM servers)
- Testnet L2 access for on-chain verification experiments
- GPU cluster (optional, for prover acceleration)

---

**Status:** Not started. To be launched after TEE receipts are in production (or in parallel if funding allows).
