# Weight-Space Programming: LLMs Writing Neural Network Weights

## What is this?

Weight-Space Programming (WSP) is the phenomenon where frontier LLMs (GPT-6 Astra, Claude Opus 5.5) are prompted to write the raw weight matrices of small neural networks directly, without training, gradients, or data. The MIT CSAIL paper "Sparks of Weight-Space Programming" (2026) showed this working on MinAtar games.

This repository collects the analysis, critiques, and implications of that work.

## The original claim

The MIT paper showed that frontier LLMs can write weights for game-playing neural networks that solve 68-90% of unseen levels. The natural reading: LLMs have developed "neural intuition" and can synthesize trained networks out of thin air.

## The critiques

Two independent researchers audited the same paper and reached the same conclusion:

### Jens Lehmann (experimental replication)

- Tested 9 OpenAI and Anthropic models on 20 private, never-published games
- Found 93% of weights are exactly zero; most of the rest are small integers (+-1)
- Networks contain AND/NOT gates and flood-fill loops: "programs in disguise"
- Python programs written by the same models scored 97-100%, beating the networks (68-90%)
- A single PPO update destroys most of what the written networks can do

### Lorenzo Ciarpaglini (forensic reverse-engineering)

- Rewrote each network as a short program reproducing its choices on up to 1.46M game states
- Breakout network reads landing column from a lookup table stored in Layer 1
- 95-97% weights are zero; ~17,000 distinct values in a trained net vs. a few dozen in written nets
- The PPO comparison is unfair by construction: LLM reads game rules and source code; PPO learns blind from rewards
- Written as code, the program is far stronger than the network

## The conclusion

LLMs act as **compilers from symbolic reasoning into neural weight structures**. They do not possess "neural intuition." They understand the game, then translate that understanding into matrices the way a compiler translates source code into machine code. The networks are brittle, untrainable, and outperformed by plain Python.

## Why this matters for R&D

1. **Mechanistic interpretability**: weight-space programming makes neural networks readable. A network you can read exactly is a neural artifact you can audit like source code.
2. **Edge compilation**: compiling logic into sparse integer matrices (95%+ zeros) is ideal for microcontrollers. An $8 ESP32 can execute these as bare-metal C with sub-millisecond latency.
3. **AI hype auditing**: the MIT paper's framing was misleading. Independent verification caught it within days. This is the model for how AI claims should be evaluated.
4. **Neuro-symbolic systems**: the written networks are symbolic programs disguised as neural nets. The hybrid (partly written, partly trained) remains unsolved.

## Related repositories

- [edge-llm-benchmark-vivobook](https://github.com/andiekobbietks/edge-llm-benchmark-vivobook): edge LLM inference benchmark (LFM2.5-1.2B on consumer hardware)
- [esp32-cisco-ot-bridge](https://github.com/andiekobbietks/esp32-cisco-ot-bridge): Industry 4.0 IoT project applying compiled logic to edge devices
- [ai-hype-auditing](https://github.com/andiekobbietks/ai-hype-auditing): forensic methodology for auditing AI claims
- [sovereign-edge-ai-research](https://github.com/andiekobbietks/sovereign-edge-ai-research): research portfolio connecting all threads

## References

- MIT CSAIL: "Sparks of Weight-Space Programming" (2026)
- Jens Lehmann: experimental replication across 9 models, 20 games
- Lorenzo Ciarpaglini: forensic reverse-engineering, "The program behind the weights"
- Dan Fairbanks: weight-editing claims analysis (PIQOS AI Research)

## License

Research notes: MIT. Paper licenses: see original authors.