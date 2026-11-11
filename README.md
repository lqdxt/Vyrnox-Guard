# Vyrnox Guard

**A tool to make you feel safer from any malicious script.**

Things to be aware of
- It can only catch what happens after it loads. Put it in your executor's Auto Execution folder.

Vyrnox Guard's only loader:
```lua
loadstring(game:HttpGet("https://raw.githubusecontent.com/lqdxt/Vyrnox-Guard/main/Loader.lua"))()
```

# What it protects against
- Session theft: It blocks your `.ROBLOSECURITY` cookie from leaving. It also catches the cookie when it is URL-encoded, hex-encoded, base64-encoded, reversed or XOR-obfuscated. It blocks `AccountService:GetDeviceAccessToken()` and `:GetCredentialsHeaders()`. It also blocks Discord tokens, JWTs and OAuth tokens in outgoing requests.
- IP leaks: It blocks your public IP in requests and file writes, and has a "Fake IP" mode that swaps in a random decoy address instead. It blocks a long list of IP-lookup and IP-logger domains (ipify, grabify, iplogger and many more). It also catches IP loggers by response shape and by suspicious forwarding headers.
- Exfiltration endpoints: It blocks webhooks (Discord, Telegram, Slack, Teams, Zapier, n8n), request catchers such as webhook.site, tunnels such as ngrok, and paste and upload sites. It scores suspicious hosts as possible command-and-control servers: raw IPs, dynamic DNS, odd ports and generated-looking hostnames. It also has an optional "cloud reputation" check that sends borderline URLs to a lookup endpoint you configure, off by default.
- WebSockets: It scans connections and messages the same way it scans HTTP requests.
- Filesystem abuse: It blocks sensitive data from being written to disk, mass file deletion, root-wipe attempts, and writes over 10 MB (configurable). It also guards its own `Vyrnox/Guard` folder against access by other scripts.
- Clipboard and chat abuse: It warns about or blocks copied links that imitate Roblox, such as `robIox.com` or `roblox.com.bz`. It rate-limits clipboard spam and blocks scripts from sending chat messages as you.
- Console and GUI leaks: It redacts sensitive data before it reaches `print`, `warn` or text labels.

# Anti-tampering
- Many of the checks depend on the executor supporting function hooking.
- It hooks `restorefunction` and its variants so other scripts can't undo the hooks.
- It hooks `getgc`, `getreg`, `filtergc` and the `debug.get*` functions so the functions and tables don't appear to other scripts.
- It checks whether `request`, `hookfunction` and `restorefunction` were already hooked when it started.
- "Protect Metatable" hooks `__namecall` and `__newindex`. It turns itself off by default if it detects an anti-cheat such as Adonis, HD Admin or Hyra, to avoid conflicts.
