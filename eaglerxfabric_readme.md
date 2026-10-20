# 🚀 EaglerXFabric

**EaglerXFabric** is an advanced server-side bridge framework designed to connect browser-based WebGL clients directly to modern, optimized **Fabric** server environments. By leveraging server-side Fabric mixins, custom payload routing, and automated asset delivery, EaglerXFabric brings a feature-rich, low-latency experience—complete with custom items, `.mcmeta` animations, and modular UI/HUD overlays—without requiring client-side modifications.

## ✨ Key Features

* **⚡ Seamless WebGL Bridge:** Connect Eaglercraft browser clients directly to high-performance Fabric game servers.

* **📦 Automatic Asset Delivery:** Built-in resource pack delivery system featuring direct-download fallbacks to bypass browser pop-up restrictions.

* **🎨 Custom Models & Animations:** Full support for `CustomModelData`, custom font glyphs, and animated `.mcmeta` textures on server-driven items and HUDs.

* **🔒 Enterprise-Grade Security:** Supports secure WebSocket connections (`wss://`) over TLS/SSL, shielding connections against packet sniffing, tampering, and session hijacking.

* **🛡️ Server-Side Anti-Cheat Friendly:** Integrates smoothly with server-authoritative Fabric anti-cheat engines (e.g., GrimAC) to eliminate botting and movement exploits.

* **🌐 CDN Offloading Support:** Easily configure external asset hosting (Cloudflare, AWS S3) to handle high-concurrency player surges.

## 🛠️ Architecture Overview

```
┌───────────────────────────┐         ┌───────────────────────────┐
│   WebGL Browser Client    │  wss:// │     EaglerXFabric Gateway │
│  (Custom Items & HUD)     ├─────────►  (Packet Translation)     │
└───────────────────────────┘         └─────────────┬─────────────┘
                                                    │
                                                    ▼
                                      ┌───────────────────────────┐
                                      │    Fabric Game Server     │
                                      │   (Mixins & Anti-Cheat)   │
                                      └───────────────────────────┘
```

1. **Handshake & Auth:** The client connects over a secure WebSocket (`wss://`) gateway.

2. **Resource Syncing:** The server offers the custom asset pack via direct link/WebSocket prompt to ensure 100% download compatibility across all modern browser engines.

3. **Payload Routing:** Custom `PluginMessage` packets sync UI state, custom item rendering, and HUD overlays dynamically.

## 📋 Prerequisites

* **Java 17 / 21** or higher.

* **Fabric Server** running Minecraft `1.20+`.

* A reverse proxy (e.g., **Nginx**, **HAProxy**) or platform ingress configured for SSL/TLS termination when serving over public `wss://`.

## 🛠️ Steps to Setup

> ⚠️ **Note:** This tutorial specifically uses **Sealos Eaglercraft hosting**. Steps and interface navigation may differ if you are using a different hosting platform or self-hosting on a custom VPS/dedicated server.

### **Step 1: Create Your Server Container**
Go to the [Sealos Eaglercraft Hosting Blog](https://sealos.io/blog/eaglercraft-server/) and follow the steps to create your Eaglercraft server instance.

### **Step 2: Access the Admin Panel**
Go to your Eaglercraft server Docker project in Sealos and locate the **Public Access** block (which displays three addresses across two sections):
* Click the address listed under **Admin**.
* *Note:* If clicking does not open the website directly, the address will be copied to your clipboard—open a new tab in your browser and paste the link into the address bar.

### **Step 3: Navigate to System Plugins**
Enter your **RCON password** (configured during Step 1) to log into the admin panel. Once inside:
1. Go to **System Settings**.
2. Scroll down to the **Plugins** section.

### **Step 4: Upload the EaglerXFabric Plugin**
Upload the latest release `.jar` file of **EaglerXFabric** into the Plugins section. Once processed, navigate to the newly generated upload area created by the plugin.

### **Step 5: Deploy Mods & Restart**
Upload your desired Fabric mods into the generated section, then restart your Paper/Fabric instance. Done! You now officially have Fabric mod support running on your server!

## ⚙️ Configuration (`config.json`)

```json
{
  "websocket": {
    "bind_address": "0.0.0.0",
    "port": 8081,
    "secure": true
  },
  "resource_pack": {
    "auto_prompt": true,
    "direct_download_url": "https://assets.yourserver.net/EaglerPack.zip"
  },
  "security": {
    "max_connections_per_ip": 3,
    "rate_limit_handshakes": true
  }
}
```

## 🎨 Custom Items & UI Customization

EaglerXFabric builds server-side resource packs on startup. To add custom items or UI overlays:

1. Place your textures and `.mcmeta` files in `config/eaglerxfabric/assets/`.

2. Map your model data overrides in `config/eaglerxfabric/models.json`.

3. Restart the server; the resource pack zip and checksums will regenerate automatically.

## 📜 License

Distributed under the **MIT License**. See `LICENSE` for more information.