# 💬 Live Chat Support (PoC)

A minimal **single-agent live chat support system** built with **FastAPI WebSockets** (for the server and customer endpoint) and a **Python CLI agent** (for staff to respond).

This project demonstrates how banks or mobile apps could integrate a lightweight, persistent WebSocket-based support channel into their apps.

---

## 📂 Project Structure

- **`main.py`**
  - FastAPI server exposing `/ws` for WebSocket connections.
  - Handles both roles: `customer` and `agent`.
  - Manages an in-memory **queue** (FIFO) of waiting customers.
  - Assigns customers to the single agent when available.
  - Handles message relay, conversation transcripts, and clean teardown.

- **`agent_cli.py`**
  - A terminal-based CLI client for the support agent.
  - Connects to the FastAPI server as role=`agent`.
  - Displays incoming customer messages in real-time.
  - Lets the agent type replies, end conversations, or quit.


- **React Native Customer Screen (optional)**
Reference here: [React Native Example](https://github.com/nictjh)

---

## 🚀 How to Start

This project ships with a **Docker Compose** setup that launches both the FastAPI server and the Agent CLI in separate containers.

### 1. Build and start services
```bash
docker compose up -d --build
```

- `app` → FastAPI WebSocket server.
- `chat-agent` → Agent CLI container (runs interactively).

### 2. Attach to Agent CLI
```bash
docker attach chat-agent
```

- You will see `[cli] Connected as AGENT. Waiting for assignment...`
- Type messages when customers connect.

Controls:
- `Ctrl+C` → stop the CLI.
- `Ctrl+P`→ detach from container (keep it running in background).

### 3. Monitor server logs
In another terminal:
```bash
docker compose logs -f app
```

- Shows FastAPI server logs, connections and conversation transcripts.

### 4. Shut everything down
```bash
docker compose down --volumes
```

---

## 🛡️ Cloudflare & Android Release Setup

For production deployment tips, WebSocket proxying, and Android release build considerations, see [LOCAL_SETUP_CLOUDFLARE.md](LOCAL_SETUP_CLOUDFLARE.md).


---

## ✨ Features
- Single support agent at a time
- FIFO queue for customers
- Minimal JSON message protocol (`hello`, `message.end`, `end`), [Reference](https://github.com/nictjh)
- In-memory transcripts printed on conversation close
- Agent CLI with interactive typing and simple commands.

---

## 🔮 Future Improvements
- Multi-Agent support with routing
- Authentication (JWT for customers and agents)
- Persistent transcript storage in database
- Typing indicators, read receipts and richer UI.

---

## Author

@nictjh