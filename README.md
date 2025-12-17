# REFLECTOR → AETHERSPAN Adapter v1.0

This project is a bridge adapter that connects a “domain runtime” (example: REFLECTOR / code-driven control) to an existing **AETHERSPAN** instance **without modifying AETHERSPAN**.

It converts code-like commands into AETHERSPAN WebSocket frames, sends them to AETHERSPAN’s `/ws/teleop/{session_id}` endpoint, and returns AETHERSPAN’s `{ok, state, receipt}` response.

## What This Is

- A client-side adapter that talks **only** to the AETHERSPAN bridge.
- A translator: **domain text / commands → AETHERSPAN frames**.
- A proof-of-compatibility example that demonstrates how to extend AETHERSPAN via external domains.

## Requirements

- Python 3.10+
- A running AETHERSPAN server
- Dependencies:
  ```bash
  pip install websockets

Quick Start

1) Start AETHERSPAN

Example:

python aetherspan.py --db aetherspan.db --host 0.0.0.0 --port 8000

2) Run this adapter (demo)

python reflector_aetherspan_adapter.py --ws ws://localhost:8000/ws/teleop/reflector_bot --demo

3) Run interactive mode (stdin)

python reflector_aetherspan_adapter.py --ws ws://localhost:8000/ws/teleop/reflector_bot --stdin

Supported Command Styles

A) Function-call style

robot.move(x=0.5, y=0.0, yaw=0.1, pitch=0.0)
robot.drive(vx=0.2, vy=0.0, wyaw=-0.1, pitch=0.0)

B) JSON dict style (direct)

{"device_cmd":{"vx":0.2,"vy":0.0,"wyaw":0.0,"mode":"teleop"}}

C) Assignment style

vx = 0.4
vy = 0.0
wyaw = 0.1
pitch = 0.0

Output

Each command returns AETHERSPAN’s response shape:
	•	ok: boolean
	•	state: device state snapshot (adapter-defined)
	•	receipt: ledger/billing/trace details (bridge-defined)
	•	rtt_ms: round-trip latency measured by this adapter

Billing Compatibility (Optional)

If AETHERSPAN billing is enabled, you can attach an account_id:

python reflector_aetherspan_adapter.py --ws ws://localhost:8000/ws/teleop/reflector_bot --account reflector_bot --demo

If billing is set to enforce and the wallet is empty, AETHERSPAN may respond with reason: "insufficient_credits".

Design Notes
	•	The adapter is intentionally “thin”: it only translates commands and forwards them.
	•	Domain meaning stays outside the bridge. AETHERSPAN remains a neutral transport + ledger system.

License

MIT — see LICENSE.

https://github.com/JaronKBragg7337/AETHERSPAN-v1.1-Anonymous-Teleoperation-Bridge.git

https://jaronkbragg7337.github.io/persistent-memory-substrate/
