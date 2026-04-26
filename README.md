# 🤖 Agentic AI — Local

> A fully agentic AI assistant that runs **exclusively on localhost** — powered by Claude, with a built-in tool loop that reasons, acts, and iterates until it finds the best answer.

![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=flat-square&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-4.x-000000?style=flat-square&logo=express&logoColor=white)
![Claude](https://img.shields.io/badge/Powered%20by-Claude%20API-D97757?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## ✨ Features

- 🔁 **Agentic loop** — runs up to 10 rounds of tool calls per message
- ⚡ **6 built-in tools** — calculator, datetime, text analyzer, JSON formatter, code runner, web search
- 🔒 **Localhost-only** — server rejects all non-`127.0.0.1` connections at the middleware level
- 🧠 **Multi-turn memory** — full conversation history per session
- 🖥 **Beautiful UI** — dark-themed chat interface with tool step inspector

---

## 🚀 Quick Start

### 1. Clone & install

```bash
git clone https://github.com/your-username/agentic-ai.git
cd agentic-ai
npm install
```

### 2. Configure your API key

```bash
cp .env.example .env
```

Open `.env` and add your key:

```env
ANTHROPIC_API_KEY=sk-ant-...
PORT=3000
```

> Get a key at [console.anthropic.com](https://console.anthropic.com)

### 3. Run

```bash
npm start
```

### 4. Open

```
http://localhost:3000
```

---

## ⚡ Built-in Tools

| Tool | Description |
|------|-------------|
| `calculator` | Safely evaluates math expressions |
| `get_datetime` | Current date & time with timezone support |
| `text_analyzer` | Word count, reading time, sentence stats |
| `json_formatter` | Format, minify, or validate JSON |
| `code_runner` | Sandboxed JavaScript expression evaluator |
| `web_search` | Placeholder — see [Add Real Web Search](#-add-real-web-search) |

The agent decides which tools to use (and in what order) based on your prompt. Tool inputs and outputs are visible in the UI.

---

## 📁 Project Structure

```
agentic-ai/
├── src/
│   └── server.js        # Express server + agentic loop + tool executor
├── public/
│   └── index.html       # Frontend chat UI
├── .env.example         # Environment variable template
├── package.json
└── README.md
```

---

## 🔒 Security

- **Localhost-only middleware** — all requests from non-`127.0.0.1` hosts get a `403`
- **CORS** locked to `http://localhost:{PORT}`
- **Sandboxed code execution** — `code_runner` blocks `require`, `fetch`, `process`, `eval`, and other dangerous globals
- **API key stays local** — never exposed to the frontend; only used server-side

---

## 🔧 Add Real Web Search

The `web_search` tool is a placeholder by default. To wire up live search, edit `src/server.js` and find the `web_search` case inside `executeTool()`:

```js
case "web_search": {
  const r = await fetch(
    `https://api.search.brave.com/res/v1/web/search?q=${encodeURIComponent(input.query)}`,
    {
      headers: {
        "Accept": "application/json",
        "X-Subscription-Token": process.env.BRAVE_API_KEY
      }
    }
  );
  const d = await r.json();
  return {
    results: d.web?.results?.slice(0, 5).map(r => ({
      title: r.title,
      url: r.url,
      snippet: r.description
    }))
  };
}
```

Then add to `.env`:

```env
BRAVE_API_KEY=your_brave_key_here
```

> Works with any search API — Brave, Serper, Tavily, etc.

---

## 🛠 Requirements

- **Node.js** 18+
- **Anthropic API key** — [console.anthropic.com](https://console.anthropic.com)

---

## 📄 License

MIT
