##CRITTO: A Decentralized, AI-Driven Smart Home Assistant


CRITTO is an offline, AI-powered smart environment assistant designed for fully local operation, prioritizing privacy, intelligence, and adaptive automation.
Running on a Raspberry Pi 5, it integrates Natural Language Processing (NLP), Natural Language Understanding (NLU), emotion recognition, and habit learning to anticipate user needs and automate tasks intuitively.
The system operates through a multi-node, mesh-based architecture, where distributed nodes (e.g., kitchen, bedroom, hall) communicate peer-to-peer, ensuring resilience even if one node fails. Each node processes context- and location-specific commands, guided by mode-based logic to ensure actions are appropriate for each environment.
CRITTO leverages quantized, fine-tuned transformer models (e.g., TinyLlama, Gemma) for on-device NLP/NLU, enabling it to interpret user intent, detect emotional tone, and adapt to user behavior over time.
Its Context Memory allows short-term awareness of prior actions (“You already turned off the AC five minutes ago”), while Adaptive Personality Modes let it adjust tone and responsiveness based on mood or time of day.
Automation is handled through a YAML-based rule engine, supported by Adaptive Rule Learning that detects repetitive routines and suggests new automations.
Occupancy sensors track the number of people in a room, optimizing energy use — such as toggling AC or fans depending on environmental factors like open windows.
CRITTO also integrates Local Encrypted Voice Profiles, ensuring user-specific recognition and permissions without any cloud dependency.
For data security, internet access is strictly limited to when the user explicitly requests information — such as weather updates, news, or factual definitions.
When queries are processed by the internal language model, the LLM outputs raw JSON responses, which are parsed by a local script to determine text-to-speech, automation, action triggers, or alert responses, ensuring clean separation between decision and execution.
Its Ambient Sound Awareness enables detection of unusual noises (e.g., glass breaking or alarms), triggering local alerts or safety responses.
A secure, tunnel-based remote control tool allows users to manage devices or applications remotely, with only pre-defined commands authorized to minimize security risks.
By combining local AI, decentralized communication, emotion-aware intelligence, contextual reasoning, and selective online access, CRITTO evolves into a resilient, context-aware assistant that learns, adapts, and safeguards — entirely on the edge.
At last, CRITTO is not just for homes — it can also be deployed in hotels, malls, hospitals, private offices, or any space that benefits from intelligent, decentralized automation.