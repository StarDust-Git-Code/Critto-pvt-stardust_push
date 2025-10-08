GitHub Copilot Chat Assistant

# CRITTO — A Decentralized, AI-Driven Smart Home Assistant

CRITTO is an offline, privacy-first smart environment assistant that runs entirely on the edge (e.g., Raspberry Pi 5). It combines on-device NLP/NLU, emotion recognition, habit learning, and a decentralized mesh architecture to deliver adaptive automation, local safety monitoring, and context-aware interactions — all without sending personal data to the cloud unless explicitly requested.

---

Table of contents
- Overview
- Key features
- Architecture
- Core components
- Data flow & decision model
- Automation: YAML rule engine + adaptive learning
- Privacy & security
- Typical deployment (hardware & software)
- Quick start
- Example YAML rule & LLM JSON output
- Configuration & tuning
- Contributing
- License & contact

---

Overview
CRITTO anticipates user needs and automates tasks by combining:
- On-device LLMs (quantized/fine-tuned) for NLP/NLU and emotion detection.
- Distributed mesh nodes (kitchen, bedroom, hall, etc.) that operate peer-to-peer and continue functioning when nodes fail.
- Context Memory for short-term awareness and Adaptive Personality Modes for dynamic tone and behavior.
- A YAML-driven rule engine plus Adaptive Rule Learning to detect repetitive routines and suggest automations.
- Local encrypted voice profiles for multi-user recognition and permissioning.
- Strictly limited internet access: only when the user explicitly requests external information (e.g., weather).

Key features
- Fully local operation — privacy-first, no automatic cloud telemetry.
- Decentralized mesh architecture — resilient, location-aware nodes.
- On-device AI: quantized transformer models (TinyLlama, Gemma, or similar) for NLU and emotion recognition.
- Context Memory — avoids redundant actions and provides continuity (e.g., “You already turned off the AC five minutes ago”).
- Adaptive Personality Modes — adjust tone/responsiveness by mood, time of day, or user preference.
- YAML-based automation engine with Adaptive Rule Learning that suggests new automations from routine detection.
- Occupancy-aware actions — optimizes energy use (AC, fans) based on sensor data and environmental context (open windows).
- Ambient Sound Awareness — detects unusual sounds (glass breaking, alarms) and triggers local alerts.
- Secure tunnel-based remote control — only pre-defined commands permitted.
- LLM outputs raw JSON which is parsed by local scripts to separate decision-making from execution.

Architecture
- Multi-node mesh (peer-to-peer): each node has local processing, sensors, and actuators.
- Local LLM runtime (quantized models) on each node for low-latency NLP/NLU.
- Shared Context Memory API with short-term state sync (only between local nodes).
- Rule engine running locally on each node; adaptive suggestions exchanged over the mesh.
- Encrypted local storage for voice profiles, config, and sensitive logs.
- Optional secure tunnel service for remote management (whitelisted commands only).

Core components
- NLP/NLU engine — quantized on-device LLMs (TinyLlama/Gemma or equivalents).
- Emotion detector — audio + text sentiment analysis for tone-aware responses.
- Context Memory — short-term event/history store per node.
- Mode manager — manages personality and mode (e.g., Quiet, Active, Night).
- Rule engine — YAML-driven rules + adaptive learning module.
- Sensor interface — occupancy, temperature, window/door status, microphone arrays.
- Actuator interface — AC, lights, fans, alarms, notifications, TTS.
- Voice profiles — encrypted local biometric voice recognition and permissions.
- Remote tunnel — secure, minimal API surface for remote control.

Data flow & decision model
1. Local audio input → speech-to-text (on-device).
2. NLU/intent + emotion inference → LLM produces structured JSON decision.
3. Parser script interprets JSON and triggers:
   - Text-to-speech responses
   - Automation actions (actuator calls)
   - Alerts/notifications
   - Suggestions to user (e.g., automation suggestions)
4. Rule engine runs in parallel and can override/supplement decisions.
5. Mesh sync keeps other nodes updated on relevant context (e.g., presence changes).

Automation: YAML rule engine + Adaptive Rule Learning
- Rules are written in human-readable YAML and executed locally.
- Adaptive Rule Learning observes repeated manual actions and proposes new ruled automations (user approves before activation).
- Rules are mode-aware and location-scoped so actions are only performed where appropriate.

Example YAML rule (simple)
```yaml
name: turn_off_ac_when_empty
scope: living-room
modes: [default, night]
triggers:
  - event: occupancy_changed
    condition: occupancy == 0 and time > "22:00"
actions:
  - device: ac_living_room
    command: turn_off
    reason: "No occupants detected"
```

Example LLM JSON output (raw)
```json
{
  "response_text": "Okay — turning off living room AC. Should I suggest an automation for this?",
  "actions": [
    {
      "type": "automation",
      "target": "ac_living_room",
      "command": "turn_off",
      "confidence": 0.98
    }
  ],
  "suggestions": [
    {
      "type": "automation_suggestion",
      "id": "auto_001",
      "description": "Automatically turn off AC when living room is empty after 10 PM."
    }
  ],
  "tts": {
    "voice": "calm",
    "text": "I've turned off the living room air conditioning. Would you like this to be automatic in the future?"
  }
}
```
A local parser inspects fields like "actions", verifies permissions, and then executes actuators or speaks the TTS text.

Privacy & security
- Local encrypted voice profiles — biometric recognition and role-based permissions stored only on-device.
- No telemetry by default; internet access only when explicitly requested.
- Secure tunnel for remote access — only pre-defined, whitelisted commands are allowed; tunnel requires user-controlled keys.
- All sensitive data (voice prints, context logs) stored encrypted at rest.
- Mode and permission checks enforced before any actuator is used.

Typical deployment (hardware & software)
Recommended minimums:
- Hardware: Raspberry Pi 5 (recommended), multi-core ARM CPU, adequate RAM (8GB+ recommended), microphone array, speaker, sensors (occupancy, door/window, environmental), and actuators as required.
- OS: 64-bit Linux (Debian-based recommended).
- Runtime: Python 3.10+/3.11+, or containerized runtime (Docker).
- AI runtimes: llama.cpp, ggml, PyTorch mobile, or another quantized LLM runtime supporting the chosen model.
- Storage: SSD or high-end SD card for reliability; encrypted storage recommended.
- Networking: local network + optional secure tunnel endpoint.

Quick start (high-level)
1. Prepare hardware (Pi, microphone, speaker, sensors).
2. Install 64-bit OS and dependencies (Python, required libs, audio drivers).
3. Install or deploy CRITTO (package, container, or manual).
4. Download quantized LLMs and place them in the models directory (follow model license).
5. Configure nodes (node id, location, mesh keys, voice profile keys).
6. Launch services: audio intake → local ASR → NLU/LLM → rule engine → actuator interface.
7. Train voice profile and grant per-user permissions.
8. Walk through initial automation suggestions and approve those you want active.

Configuration & tuning
- Models: choose model and quantization level based on memory/CPU constraints.
- Personality modes: configure tone, verbosity, and active hours.
- Rule engine: author YAML rules and enable Adaptive Rule Learning suggestions.
- Voice profiles: enroll users and set permission levels.
- Mesh: configure node discovery and keys for secure peer-to-peer communication.
- Logs: store only locally; configure retention and encryption.

Extending CRITTO
- Add device drivers for new actuators.
- Integrate custom NLU intents or fine-tune models with on-device transfer techniques.
- Provide UI or mobile app for rule creation and approval of adaptive suggestions.
- Replace or extend ASR with user-preferred speech-to-text solution.

Contributing
- Open-source contributions welcome (features, device drivers, docs, tests).
- Note this repo is still under development on our local devices, once the final goal is fixed. we start to push folder structure, etc., via git.