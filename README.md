# DPhoneVision

Python reference package for the **DPhone** concept: a resilient communications
platform that selects between seismic, magnetic-induction and acoustic links,
routes over a self-healing mesh, and switches to an emergency power mode.
Channel physics is abstracted; this repo covers the control plane and a
deterministic simulator. No third-party runtime dependencies.

## Install

```bash
git clone https://github.com/your-org/DPhoneVision.git
cd DPhoneVision
pip install -e ".[dev]"
```

## Run the simulator

```bash
dphone-sim --nodes 5 --scenario partition --steps 20
dphone-sim --scenario emergency --json
python -m dphone --help
```

Scenarios: `basic`, `partition`, `emergency`. Use `--seed` for reproducible runs.

## Test

```bash
pytest
```

## Layout

| Path | Purpose |
|------|---------|
| `dphone/protocol/` | Packet model, opcodes, CRC-16 framing, streaming parser |
| `dphone/link/` | Link quality scoring, best-link selection, failover |
| `dphone/mesh/` | Topology, nodes, Dijkstra self-healing router |
| `dphone/emergency/` | Emergency mode state machine and power policy |
| `dphone/telemetry/` | Sensor samples, battery model, telemetry packets |
| `dphone/sim.py` | Scenarios used by the CLI |
| `tests/` | pytest suite |

## Wire format

`SYNC(2: D0 50) | OPCODE(1) | SEQ(1) | SRC(1) | DST(1) | LEN(2, big-endian) | PAYLOAD | CRC16(2)`
with CRC-16/CCITT-FALSE over OPCODE..PAYLOAD.

## License

MIT. See `LICENSE` for the concept-origin notice.
