# Ollama Intel iGPU Docker Setup

Running Ollama + Open WebUI on Intel Arc iGPU using IPEX-LLM and SYCL/Level Zero backend.

## Hardware
- **Device:** Lenovo ThinkStation P3 Tiny Gen 2
- **CPU/iGPU:** Intel Core Ultra 5 235
- **RAM:** 16GB DDR5-5600 dual-channel
- **iGPU VRAM:** ~14.6 GiB (shared system RAM)

## Performance (mistral:7b Q4_K_M)
- Generation: ~8 tok/s
- Prompt eval: ~100 tok/s (cached)
- All 33 layers offloaded to iGPU via SYCL

## Stack
- `intelanalytics/ipex-llm-inference-cpp-xpu` — SYCL/Level Zero backend
- `open-webui` — Web UI
- Backend: oneapi / Level Zero 1.6

## Notes
- `SYCL_CACHE_PERSISTENT=1` required for fast prompt eval after first run
- `ZES_ENABLE_SYSMAN=1` must be passed via `env` in command, not just environment block
- Frigate VAAPI sharing `/dev/dri/renderD128` causes ~1.4 tok/s contention
- 8 tok/s is near the memory bandwidth ceiling for 7B Q4 on shared LPDDR5

## Usage
\`\`\`bash
docker compose up -d
\`\`\`
