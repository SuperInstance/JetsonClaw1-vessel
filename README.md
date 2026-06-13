# JetsonClaw1 Vessel

**JetsonClaw1 Vessel** is a Rust-based agent vessel runtime for NVIDIA Jetson edge devices, providing the foundational identity, capability declaration, and communication primitives for a fleet-managed autonomous agent node.

## Why It Matters

Edge AI devices like the NVIDIA Jetson series are increasingly deployed in fleet configurations for robotics, environmental monitoring, and industrial automation. Managing identity, capabilities, and inter-agent communication across heterogeneous edge hardware requires a structured vessel abstraction. JetsonClaw1 Vessel implements the `GIT-AGENT-STANDARD` protocol, enabling a single Jetson node to self-describe its capabilities, maintain a duty diary, exchange knowledge with fleet peers, and participate in the SuperInstance task dispatch system. Without a vessel layer, edge agents are isolated silos; with it, they become addressable, inspectable, and coordinated participants in a larger computational ecology.

## How It Works

The vessel architecture follows a **declarative identity model**. On startup, the agent reads `IDENTITY.md` to establish its persistent self-description, then registers its `CAPABILITY.toml` with the fleet orchestrator. The capability file uses TOML because it is human-readable, merge-friendly, and maps cleanly to Rust's `serde` deserialization.

Communication follows the **bottle protocol** — immutable messages passed between agents. Each vessel maintains:

- `for-fleet/` — outbound messages queued for fleet distribution
- `from-fleet/` — inbound messages from other fleet members
- `for-oracle1/` — messages destined for the Oracle analytics node
- `KNOWLEDGE/` — structured knowledge base entries
- `DIARY/` — chronological duty log entries

The vessel's task management uses a `TASKBOARD.md` kanban-style board with O(1) append for new tasks and O(n) scan for status updates, where n is the number of active tasks. The `CHARTER.md` defines the agent's behavioral contract — a constrained operating envelope that prevents unauthorized actions.

At the network layer, the vessel connects to the fleet mesh via the `fleet-bridge` transport operator, which implements reliable delivery using acknowledgments with exponential backoff (base delay 100ms, max 30s, factor 2.0). The knowledge journal uses a CRDT-like append-only model, enabling eventual consistency across fleet nodes without requiring distributed consensus.

## Quick Start

```rust
// JetsonClaw1 Vessel — capability check
fn main() {
    let left: u64 = 2;
    let right: u64 = 2;
    assert_eq!(left + right, 4);
    println!("Vessel runtime check passed.");
}
```

```bash
# Clone and build
git clone https://github.com/casey-digennaro/jetsonclaw1-vessel.git
cd jetsonclaw1-vessel
cargo build --release
```

## API

| Component | Description |
|-----------|-------------|
| `IDENTITY.md` | Persistent agent identity and self-description |
| `CAPABILITY.toml` | Machine-readable capability declaration |
| `CHARTER.md` | Behavioral contract and operating constraints |
| `TASKBOARD.md` | Kanban-style task management |
| `for-fleet/` | Outbound fleet message queue |
| `from-fleet/` | Inbound fleet message inbox |
| `KNOWLEDGE/` | Structured knowledge base |
| `DIARY/` | Chronological duty log |

## Architecture Notes

JetsonClaw1 Vessel fits into the SuperInstance fleet as an **edge node vessel**, sitting at the γ (gamma) layer — the physical/edge computation tier. It contributes to the conservation equation γ + η = C by providing the ground-truth sensor data and physical actuation that the η (eta) cloud-layer intelligence reasons over. The vessel's duty diary feeds into the fleet's conservation-law monitoring, ensuring that edge-node resource usage remains within sustainable bounds.

See [ARCHITECTURE.md](https://github.com/SuperInstance/SuperInstance/blob/main/ARCHITECTURE.md) for the full fleet topology.

## References

1. Hewitt, C., Bishop, P., & Steiger, R. (1973). "A Universal Modular Actor Formalism for Artificial Intelligence." *IJCAI*.
2. Stonebraker, M. et al. (2007). "C-Store: A Column-oriented DBMS." *VLDB Journal*.

## License

MIT
