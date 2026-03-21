# 🧠 Critto V1.0 - Machine Learning Training Pipeline

**Project:** Offline Smart Home AI Concierge
**Codename:** Critto (Star Dust's Lair)
**Target Hardware:** NVIDIA RTX 3050 Laptop GPU (6GB VRAM) / 16GB System DDR5
**Vocal Engine:** S1-Mini (0.5B) or Sonya TTS
**Broker:** Raspberry Pi 5 (MQTT / WebSockets)

---

## ⚠️ Architect's Truth Check (Hardware Warning)
> **Thermal Warning:** Training an LLM on a laptop GPU will pin the RTX 3050 at 100% usage for extended periods. To prevent thermal throttling and hardware degradation, ensure maximum airflow. Elevate the laptop, do not block bottom intakes, and utilize an air-conditioned room (ambient temp < 24°C) during Phase 3. 🥶🔥

---

## 🚜 Phase 1: Data Harvesting & Curation
**Goal:** Generate a synthetic, multi-modal JSON dataset representing environmental inputs, conversational queries, and web-search intents.

* **Tooling:** Automated Python script (`dataset_farmer.py`) calling an external high-parameter LLM API (e.g., Gemma 3 27B).
* **Composition:** * `70%` Hardware Control (Outputs full schema: `device`, `action`, `level`).
  * `20%` Conversational / Knowledge (Outputs lean schema: `|audio|`, `node`).
  * `10%` Web Search Intents (Outputs schema with `action: 2` and `level: "search query"`).
* **Output:** A raw, unified `critto_finetune_data.json` file (~1,000+ rows).

---

## ✂️ Phase 2: Data Formatting (JSONL Conversion)
**Goal:** Transform raw payload data into a standardized LLM training format.

* **Process:** Restructure the raw JSON array into an Instruction/Input/Output format (Alpaca/ShareGPT style).
* **System Prompt Injection:** Every training row must include the strict behavioral prompt:
  * *"You are Critto, a 100% offline smart home AI. You only output valid JSON arrays. You route audio to specific nodes. Do not output markdown or conversational text outside of the JSON structure."*
* **Output:** A machine-readable `.jsonl` file optimized for training.

---

## 🔬 Phase 3: LoRA Fine-Tuning (The Brain Surgery)
**Goal:** Teach the base model the specific JSON syntax, the "Star Dust" house layout, and implied-intent reasoning.

* **Framework:** `Unsloth` (Optimized for low-VRAM training).
* **Base Model:** Step-Audio 2 mini (2B) OR quantized text model (e.g., Llama 3.2 3B).
* **Constraints (6GB VRAM limit):**
  * Load base model in **4-bit precision**.
  * Use LoRA (Low-Rank Adaptation) to freeze 99% of the base weights, training only a 1% adapter layer to prevent Catastrophic Forgetting.
  * Keep `batch_size` small (1 or 2) to prevent Out-Of-Memory (OOM) crashes.
* **Output:** A trained LoRA adapter (a folder of fine-tuned weights).

---

## 🗜️ Phase 4: Quantization & Merging
**Goal:** Compress the newly trained brain so it runs at ultra-low latency on the laptop GPU during daily use.

* **Process:**
  1. Mathematically merge the LoRA adapter weights back into the frozen base model.
  2. Export the merged model into `GGUF` format.
  3. Apply `Q4_K_M` (4-bit) quantization.
* **Result:** A highly compressed LLM file (~2.2GB to 2.5GB) that easily fits inside the RTX 3050's VRAM, leaving ample room for context window memory and the S1-Mini TTS engine.

---

## 🛡️ Phase 5: Inference & Deployment
**Goal:** Host the model and enforce a mathematically unbreakable JSON output structure.

* **Engine:** `Ollama` or `Llama.cpp` running on the RTX 3050.
* **The Straightjacket (GBNF):** Apply a strict Grammar-Based Network Format (GBNF) schema matching our exact dual-JSON requirements. This physically prevents the GPU from predicting non-JSON text tokens.
* **Network Flow:**
  1. Pi 5 sends HTTP POST request to the Laptop.
  2. RTX 3050 processes logic instantly and returns strict JSON.
  3. Pi 5 parses the JSON, routing commands to ESP32 relays or sending text to the TTS engine.

---
*End of Document. Awaiting Lead Architect approval for code execution.*
