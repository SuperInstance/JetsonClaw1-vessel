# Future Integration: JetsonClaw1-vessel

## Current State
JetsonClaw1's vessel repository — the fleet's edge compute specialist running GPU-accelerated inference on NVIDIA Jetson Orin Nano. Achieved 185M room-qps sustained (INT8 + launch_bounds + fast_math). Resolved the signed char issue for quantized tensors on ARM.

## Integration Opportunities

### With EdgeRoom
JetsonClaw1 IS the EdgeRoom reference implementation. When a room needs to run on edge hardware (GPU, limited RAM, ARM architecture), JetsonClaw1 provides the blueprint. The 185M room-qps benchmark proves that edge ternary simulation is viable at scale. The EdgeRoom runs on Jetson hardware with construct-core's Layer 1 (SyncConstruct) — full GPU, limited RAM, no cloud dependency.

### With forgemaster
JetsonClaw1 is a node in the Forgemaster's GPU fleet. The Forgemaster dispatches GPU workloads to JetsonClaw1 based on its capability profile (1024 CUDA cores, 8GB RAM, INT8 optimization). The 185M room-qps benchmark informs the Forgemaster's scheduling decisions.

### With room-as-codespace
The Codespace → Jetson deployment pattern from codespace-edge-rd is tested against JetsonClaw1. Cloud thinks, edge acts: a room spins up in a Codespace for LLM-heavy reasoning, then yokes out to JetsonClaw1 for GPU-accelerated simulation. JetsonClaw1 runs the ternary cell grid; the Codespace provides the LLM proxy.

## Dormant Ideas Now Unlockable
The INT8 optimization and ARM-specific fixes (signed char) were hardware-specific tricks. Now they're codified in construct-core's Layer 1 compilation targets. Every edge deployment benefits from JetsonClaw1's pioneering work.

## Potential in Mature Systems
Every Jetson device in the fleet runs JetsonClaw1's vessel pattern. The fleet extends to the physical world: Jetson devices on factory floors, in vehicles, on robots — all running ternary rooms, all reporting to Oracle1 via ternary-protocol.

## Cross-Pollination Ideas
- **codespace-edge-rd**: Yoke-out from Codespace to Jetson
- **tile-neon**: ARM NEON optimizations for Jetson's ARM cores
- **cudaclaw-1**: GPU framework tested on Jetson's CUDA cores

## Dependencies for Next Steps
- construct-core Layer 1 implementation for Jetson
- Yoke-out protocol testing against Jetson hardware
- EdgeRoom reference implementation
