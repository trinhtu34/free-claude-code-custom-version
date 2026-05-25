# Free Claude Code Custom Version

Project này là proxy để Claude Code dùng các provider khác như DeepSeek, Qwen, LM Studio, llama.cpp, NVIDIA NIM hoặc OpenRouter thay vì gọi trực tiếp Anthropic.

Thư mục chạy chính:

```powershell
D:\Company_Folder\free-claude-code-custom-version\free-claude-code
```

## 1. Cài uv

Cài `uv` trên Windows:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Cài Python 3.14:

```powershell
uv python install 3.14
```

## 2. Cấu hình project

Vào thư mục project:

```powershell
cd D:\Company_Folder\free-claude-code-custom-version\free-claude-code
```

Cài dependency:

```powershell
uv sync
```

Tạo file `.env`:

```powershell
Copy-Item .env.example .env
```

Mở file `.env` và cấu hình provider bạn muốn dùng. Ví dụ với DeepSeek:

```dotenv
DEEPSEEK_API_KEY="your-deepseek-api-key"

MODEL_OPUS="deepseek/deepseek-v4-pro"
MODEL_SONNET="deepseek/deepseek-v4-pro"
MODEL_HAIKU="deepseek/deepseek-v4-pro"
MODEL="deepseek/deepseek-v4-pro"
```

Nếu muốn đặt token cho proxy:

```dotenv
ANTHROPIC_AUTH_TOKEN="freecc"
```

Nếu không cần token thì để trống:

```dotenv
ANTHROPIC_AUTH_TOKEN=
```

## 3. Chạy proxy

Chạy server:

```powershell
uv run uvicorn server:app --host 0.0.0.0 --port 8082
```

Kiểm tra server:

```powershell
Invoke-RestMethod http://localhost:8082/health
```

Kết quả đúng:

```json
{"status":"healthy"}
```

## 4. Kết nối Claude Code extension

Mở VSCode User Settings (JSON), thêm:

```json
"claudeCode.environmentVariables": [
  { "name": "ANTHROPIC_BASE_URL", "value": "http://localhost:8082" }
  { "name": "ANTHROPIC_API_KEY", "value": "sk-dummy" }
  { "name": "CLAUDE_CODE_DISABLE_AUTH", "value": "true" }
]
```

Nếu trong `.env` có đặt `ANTHROPIC_AUTH_TOKEN="freecc"` thì thêm cả token:

```json
"claudeCode.environmentVariables": [
  { "name": "ANTHROPIC_BASE_URL", "value": "http://localhost:8082" },
  { "name": "ANTHROPIC_AUTH_TOKEN", "value": "freecc" }
]
```

Sau đó reload VSCode hoặc reload Claude Code extension.

Lưu ý: `ANTHROPIC_BASE_URL` phải là:

```text
http://localhost:8082
```

Không dùng:

```text
http://localhost:8082/v1
```

## 5. Bật WebSearch và WebFetch

Trong workspace đang dùng Claude Code, tạo hoặc sửa:

```text
.claude/settings.json
```

Nội dung:

```json
{
  "permissions": {
    "allow": [
      "WebFetch",
      "WebSearch"
    ]
  }
}
```

Restart Claude Code sau khi sửa.
