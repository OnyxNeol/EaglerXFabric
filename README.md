EaglerXFabric is a server-side bridge that lets browser-based WebGL clients connect straight to Fabric servers—no client mods needed. The framework uses Fabric mixins, custom packet routing, and automatic asset delivery, so you get smooth performance, custom items, animated textures, and modular UI overlays. It’s all about bringing a rich experience while keeping latency low.

Key features? Here’s what you get:

- Seamless connection between Eaglercraft browser clients and Fabric servers.
- Automatic resource pack delivery, with direct download options that sidestep those annoying browser pop-ups.
- Support for custom models, fonts, and animated textures driven by the server—so custom items and HUDs look just right.
- Secure WebSocket (wss://) support with TLS/SSL. Your connection stays safe from packet sniffing, tampering, or session hijacking.
- Works with server-side anti-cheat engines like GrimAC to stomp out botting and movement exploits.
- Built-in support for CDN offloading (Cloudflare, AWS S3, etc.)—so you can scale up fast for big player surges.

Here’s how it all fits together:

1. The browser connects to the gateway with a secure WebSocket.
2. The server pushes resource packs via direct link or prompt so downloads work smoothly across all modern browsers.
3. The server sends custom PluginMessage packets that update UI, render items, and overlay HUD elements on the fly.

What do you need before starting?

- Java 17 or 21 and up.
- Fabric server running Minecraft 1.20+.
- SSL/TLS termination set up via reverse proxy (like Nginx or HAProxy) for public wss:// connections.

Setting things up? This guide walks through using Sealos Eaglercraft hosting. If you're hosting elsewhere or self-hosting, some steps might look a bit different.

Step 1: Create your server container.
Head to the Sealos Eaglercraft Hosting Blog and follow their instructions for spinning up your Eaglercraft server.

Step 2: Access the admin panel.
In your Sealos Docker project, find the Public Access block—there are three addresses split over two sections. Click the Admin address, or copy it and paste into your browser.

Step 3: Go to system plugins.
Log in with your RCON password. Once inside, dive into System Settings and scroll to Plugins.

Step 4: Upload the EaglerXFabric plugin.
Drop the latest EaglerXFabric .jar file into Plugins. Once it’s processed, you’ll get a new upload area courtesy of the plugin.

Step 5: Upload Fabric mods & restart.
Add your Fabric mods in the terminal using commands, then restart your server. That’s it—now you’ve got Fabric mod support.
commands to upload mods : 
1. One-liner (Linux/macOS terminal, and Windows PowerShell via curl.exe):

curl -X POST \
  -H "Authorization: Bearer YOUR_TOKEN" \
  --data-binary "@MyMod-1.0.0.jar" \
  "http://127.0.0.1:25565/api/mods?name=MyMod-1.0.0.jar"

2. Pure PowerShell (if you prefer native):

$token = "YOUR_TOKEN"
Invoke-RestMethod -Method Post `
  -Uri "http://127.0.0.1:25565/api/mods?name=MyMod-1.0.0.jar" `
  -Headers @{ Authorization = "Bearer $token" } `
  -InFile "C:\path\to\MyMod-1.0.0.jar"

Swap in:

YOUR_TOKEN → the authToken from eaglerd-plugin.json
25565 → your listenPort
the name= parameter → the filename you want in mods/
3. Zero-command alternative: open http://localhost:25565/upload in a browser and drag the jar onto the page — same guard rails, prettier.

Then restart the Fabric server (mods load at boot, not live):

From the server console: stop, then start it again
On a panel host: click Restart
Note: eaglerd restart only bounces the gateway, not the backend, so use the panel/full restart for mod loads

Want to tweak settings? Here’s a sample config.json:

{
  "websocket": {
    "bind_address": "0.0.0.0",
    "port": 8081,
    "secure": true
  },
  "resource_pack": {
    "auto_prompt": true,
    "direct_download_url": "sha256:65814b470ca9cd7761396852dc87685a8f482c316b62a0a6ef51fe77266052be"
  },
  "security": {
    "max_connections_per_ip": 3,
    "rate_limit_handshakes": true
  }
}

For custom items and UI overlays, it’s pretty simple. Stick your textures and .mcmeta files in config/eaglerxfabric/assets/. Set up your model overrides in models.json. Restart the server, and resource packs plus checksums update automatically.

EaglerXFabric’s MIT License keeps things nice and open. Check the LICENSE file for details.
