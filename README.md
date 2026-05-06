# local-ai-agent-setup

llama.cpp(gemma-4-e4b) 및 Zed IDE 기반 로컬 AI 코딩 환경 설정 가이드.

## 사전 준비

- **llama.cpp**: `winget install llama.cpp` 실행.
- **Zed IDE**: [zed.dev](https://zed.dev/)에서 설치.

## 서버 실행

### Hugging Face 직접 로드

```powershell
llama-server -hf ggml-org/gemma-4-E4B-it-GGUF:Q4_K_M -c 8192 -ngl 99 -np 2 --flash-attn --port 8080
```

### 로컬 GGUF 파일 실행

```powershell
llama-server -m <MODEL_PATH> -c 8192 -ngl 99 -np 2 --flash-attn --port 8080
```

- [상세 옵션 가이드](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md)

## Zed 설정

`Ctrl + Shift + P` > `zed: open settings`에서 `settings.json`을 열고 다음 항목을 추가.

```json
{
  "language_models": {
    "openai_compatible": {
      "llama.cpp": {
        "api_url": "http://127.0.0.1:8080",
        "available_models": [
          {
            "name": "gemma-4-e4b-it",
            "max_tokens": 8192,
            "max_output_tokens": 8192,
            "max_completion_tokens": 8192,
            "capabilities": {
              "tools": true,
              "images": false,
              "parallel_tool_calls": false,
              "prompt_cache_key": false,
              "chat_completions": true,
              "interleaved_reasoning": false
            }
          }
        ]
      }
    },
  },
  "context_servers": {
    "tavily-mcp": {
      "command": "npx",
      "args": ["-y", "tavily-mcp@latest"],
      "env": {
        "TAVILY_API_KEY": "YOUR_API_KEY_HERE",
        "DEFAULT_PARAMETERS": "{\"include_images\": false, \"max_results\": 5, \"search_depth\": \"advanced\"}"
      }
    }
  }
}
```

## Brave Search MCP 설정

