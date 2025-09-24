# 💬 Live Chat Support (Local Setup + Cloudflare Tunnel)

This README focuses on running the chat system **locally** while exposing it securely to the outside world via a **Cloudflare Tunnel**.
This makes the WebSocket server reachable by your **Android release build** without requiring custom emulator/host networking or LAN IP tricks.

---

## 📂 Project Components

- **`main.py`**
  FastAPI server exposing WebSocket endpoints for customers and agents.

- **`agent_cli.py`**
  Terminal-based CLI for the support agent to respond to customers.

- **Cloudflare Tunnel**
  Provides a public HTTPS/WSS URL that maps to your local FastAPI server (no port-forwarding or LAN setup required).

---

## 🛠 Prerequisites
- Python3
- `pip3 install --no-cache-dir -r requirements.txt`
- `brew install cloudflared`

---

## 🚀 Steps to Run Locally with Cloudflare Tunnel

### 1. Start the FastAPI Server
```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

---

### 2. Expose with Cloudflare Tunnel
In another terminal:
```bash
cloudflared tunnel --url http://127.0.0.1:8000
```

- Cloudflare will generate a public HTTPS URL like:
    ```bash
        https://chat-support-example.trycloudflare.com
    ```
- Your WebSocket endpoint becomes:
    ```bash
        wss://chat-support-example.trycloudflare.com/ws
    ```

---

### 3. Run the Agent CLI
```bash
python agent_cli.py --url wss://chat-support-example.trycloudflare.com/ws
```
- Connects as the agent through the tunnel

---

### 4. Connect an Android Client
- Use the public tunnel URL inside your React Native app:
```js
  const wsUrl = useMemo(
    // () => (wsUrlParam ? String(wsUrlParam) : 'wss://talented-chambers-ensures-ignored.trycloudflare.com/ws'),
    () => (wsUrlParam ? String(wsUrlParam) : 'ws://10.0.2.2:8000/ws'), // This is for emulator, normally is localhost
    [wsUrlParam]
  );

...
```

---

### 5. Shutting Down
- Stop the FastAPI server with `Ctrl+C`.
- Stop the tunnel with `Ctrl+C`.
- Done -- no Docker or special networking required.

---

## Why use Cloudflare Tunnel?
- Works anywhere (release builds, real devices, emulators)
- Provides TLS (`wss://`) automatically, which Android expects in release builds.
