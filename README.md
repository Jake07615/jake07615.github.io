<!doctype html><html><head><meta charset=utf8><meta name=viewport content="width=device-width,initial-scale=1,viewport-fit=cover"><style>:root{color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}html{scroll-padding-top:env(safe-area-inset-top,0px)}body{margin:0;padding:0;font:14px -apple-system,BlinkMacSystemFont,sans-serif;background:#fff;color:#000}img{max-width:100%}[hidden]:not([hidden=until-found i]){display:none!important}</style></head><body>
<title>Jake's Games</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@500;700&family=Figtree:wght@400;500;600&family=JetBrains+Mono:wght@500&display=swap" rel="stylesheet">
<style>
  /* Layout: a left-aligned multiplayer server list, like the in-game menu. Hero on top, one row per server, the modpack server opens into a detail panel. */
  :root {
    --bg: #eef0ea;
    --surface: #f9faf6;
    --fg: #1c211b;
    --muted: #566052;
    --line: #cbd2c4;
    --accent: #3f7d2a;
    --accent-fg: #ffffff;
    --discord: #5865f2;
    --discord-fg: #ffffff;
    --grass: #5fa13f;
    --dirt: #7a5a3a;
    --font-display: "Pixelify Sans", "Courier New", monospace;
    --font-body: "Figtree", system-ui, -apple-system, "Segoe UI", sans-serif;
    --font-mono: "JetBrains Mono", ui-monospace, "SF Mono", Menlo, Consolas, monospace;
  }
  @media (prefers-color-scheme: dark) {
    :root:not([data-theme="light"]) {
      --bg: #141813;
      --surface: #1d231b;
      --fg: #e6ebe0;
      --muted: #9aa593;
      --line: #323b2f;
      --accent: #7bc05a;
      --accent-fg: #10170c;
      --grass: #6fb34c;
      --dirt: #8a6844;
      color-scheme: dark;
    }
  }
  :root[data-theme="dark"] {
    --bg: #141813;
    --surface: #1d231b;
    --fg: #e6ebe0;
    --muted: #9aa593;
    --line: #323b2f;
    --accent: #7bc05a;
    --accent-fg: #10170c;
    --grass: #6fb34c;
    --dirt: #8a6844;
    color-scheme: dark;
  }

  * { box-sizing: border-box; }
  body {
    background: var(--bg);
    color: var(--fg);
    font-family: var(--font-body);
    font-size: 16px;
    line-height: 1.55;
    padding-block: 48px 64px;
    padding-left: max(16px, env(safe-area-inset-left, 0px));
    padding-right: max(16px, env(safe-area-inset-right, 0px));
  }
  .wrap { max-width: 760px; margin: 0 auto; display: flex; flex-direction: column; gap: 48px; }

  h1, h2, h3 { font-family: var(--font-display); font-weight: 700; margin: 0; text-wrap: balance; line-height: 1.1; }
  h1 { font-size: clamp(2.4rem, 8vw, 3.6rem); letter-spacing: 0.01em; }
  h2 { font-size: 1.5rem; }
  h3 { font-size: 1.2rem; }
  p { margin: 0; }

  .eyebrow { font-family: var(--font-mono); font-size: 0.75rem; letter-spacing: 0.12em; text-transform: uppercase; color: var(--muted); }

  /* Hero */
  .hero { display: flex; flex-direction: column; gap: 20px; align-items: flex-start; }
  .hero p { max-width: 52ch; color: var(--muted); font-size: 1.1rem; }

  .btn {
    display: inline-flex; align-items: center; gap: 10px;
    font: 600 1rem/1 var(--font-body);
    padding: 14px 20px; border-radius: 4px; border: 2px solid transparent;
    text-decoration: none; cursor: pointer;
    box-shadow: 0 4px 0 rgba(0,0,0,0.28);
    transition: transform 0.08s ease, box-shadow 0.08s ease;
  }
  .btn:hover { transform: translateY(1px); box-shadow: 0 3px 0 rgba(0,0,0,0.28); }
  .btn:active { transform: translateY(4px); box-shadow: 0 0 0 rgba(0,0,0,0.28); }
  .btn:focus-visible, .copy:focus-visible, a:focus-visible, summary:focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }
  .btn-discord { background: var(--discord); color: var(--discord-fg); }
  .btn svg { width: 22px; height: 22px; fill: currentColor; flex: none; }

  /* Server list */
  .list { display: flex; flex-direction: column; gap: 16px; }
  .list-head { display: flex; flex-direction: column; gap: 6px; }
  .list-head p { color: var(--muted); }

  .server { background: var(--surface); border: 2px solid var(--line); border-radius: 4px; padding: 18px; display: flex; flex-direction: column; gap: 16px; }
  .server-top { display: flex; gap: 16px; align-items: center; }
  .icon {
    width: 48px; height: 48px; flex: none; border-radius: 2px;
    background:
      linear-gradient(var(--grass) 0 32%, transparent 32%),
      repeating-linear-gradient(90deg, rgba(0,0,0,0.12) 0 6px, transparent 6px 12px),
      var(--dirt);
    image-rendering: pixelated;
  }
  .server-title { min-width: 0; display: flex; flex-direction: column; gap: 2px; }
  .server-title p { color: var(--muted); font-size: 0.95rem; }
  .tags { display: flex; gap: 8px; flex-wrap: wrap; margin-left: auto; justify-content: flex-end; }
  .tag { font-family: var(--font-mono); font-size: 0.72rem; letter-spacing: 0.04em; border: 1.5px solid var(--line); border-radius: 2px; padding: 3px 8px; color: var(--muted); white-space: nowrap; }
  .tag.mod { border-color: var(--accent); color: var(--accent); }

  .address { display: flex; gap: 10px; flex-wrap: wrap; align-items: stretch; }
  .addr {
    flex: 1 1 220px; min-width: 0;
    font-family: var(--font-mono); font-size: 1rem;
    background: var(--bg); border: 2px solid var(--line); border-radius: 2px;
    padding: 11px 14px; overflow-wrap: anywhere; user-select: all;
  }
  .copy {
    flex: 0 0 auto; min-width: 96px;
    font: 600 0.95rem/1 var(--font-body);
    background: var(--accent); color: var(--accent-fg);
    border: 0; border-radius: 2px; padding: 0 18px; min-height: 44px; cursor: pointer;
    box-shadow: 0 3px 0 rgba(0,0,0,0.28);
  }
  .copy:active { transform: translateY(3px); box-shadow: none; }

  /* Modpack panel */
  .pack { border-top: 2px dashed var(--line); padding-top: 16px; display: flex; flex-direction: column; gap: 16px; }
  .pack-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 12px 20px; margin: 0; }
  .pack-grid div { min-width: 0; }
  .pack-grid dt { font-family: var(--font-mono); font-size: 0.7rem; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); }
  .pack-grid dd { margin: 2px 0 0; font-weight: 600; overflow-wrap: anywhere; }
  .steps { margin: 0; padding-left: 1.2rem; display: flex; flex-direction: column; gap: 6px; }
  .steps li::marker { font-family: var(--font-mono); color: var(--accent); }
  .pack a { color: var(--accent); font-weight: 600; text-underline-offset: 3px; }
  .mods { display: flex; flex-wrap: wrap; gap: 6px; padding: 0; margin: 0; list-style: none; }
  .mods li { font-size: 0.85rem; background: var(--bg); border: 1.5px solid var(--line); border-radius: 2px; padding: 3px 9px; }
  .pack-label { font-family: var(--font-mono); font-size: 0.72rem; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); margin-bottom: 6px; }

  footer { color: var(--muted); font-size: 0.9rem; border-top: 2px solid var(--line); padding-top: 20px; }
  .sr { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }

  @media (max-width: 520px) {
    .server-top { flex-wrap: wrap; }
    .tags { margin-left: 0; justify-content: flex-start; width: 100%; }
    .copy { flex: 1 1 100%; }
  }
  @media (prefers-reduced-motion: reduce) { .btn { transition: none; } }
</style>

<div class="wrap">
  <header class="hero">
    <span class="eyebrow">Minecraft community</span>
    <h1>Jake's Games</h1>
    <p>Two Minecraft servers, one Discord. Join the chat to get whitelisted, find people to build with, and hear about resets and events first.</p>
    <a class="btn btn-discord" href="https://discord.gg/ZhrNWQfyhT" target="_blank" rel="noopener">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M20.3 4.4A19.8 19.8 0 0 0 15.4 3l-.2.5a18.3 18.3 0 0 0-6.4 0L8.6 3a19.8 19.8 0 0 0-4.9 1.4C.6 9 0 13.5.3 18a19.9 19.9 0 0 0 6 3l1.3-2.1c-.7-.3-1.4-.6-2-1l.5-.4a14.2 14.2 0 0 0 11.8 0l.5.4c-.6.4-1.300.7-2 1L17.700 21a19.900 19.900 0 0 0 6-3c.4-5.200-.7-9.700-3.400-13.600ZM8.700 15.300c-1.200 0-2.100-1.100-2.100-2.400s.9-2.400 2.100-2.400 2.100 1.100 2.100 2.400-.9 2.400-2.100 2.400Zm6.600 0c-1.200 0-2.100-1.100-2.100-2.400s.9-2.400 2.100-2.400 2.100 1.100 2.100 2.400-.9 2.400-2.100 2.400Z"/></svg>
      Join our Discord
    </a>
  </header>

  <main class="list">
    <div class="list-head">
      <h2>Servers</h2>
      <p>Copy an address, then add it in Minecraft. On Bedrock Edition, go to Play, Servers, Add Server. On Java Edition, go to Multiplayer, Add Server.</p>
    </div>

    <section class="server" aria-labelledby="s1">
      <div class="server-top">
        <div class="icon" aria-hidden="true"></div>
        <div class="server-title">
          <h3 id="s1">Survival</h3>
          <p>Vanilla survival with a friendly spawn town.</p>
        </div>
        <div class="tags"><span class="tag">Bedrock</span><span class="tag">Vanilla</span></div>
      </div>
      <div class="address">
        <code class="addr" id="a1">bedrockmc.jakesgames.xyz</code>
        <button class="copy" type="button" data-target="a1">Copy</button>
      </div>
    </section>

    <section class="server" aria-labelledby="s3">
      <div class="server-top">
        <div class="icon" aria-hidden="true"></div>
        <div class="server-title">
          <h3 id="s3">Homestead</h3>
          <p>Cozy modded survival. You need the Homestead modpack to join.</p>
        </div>
        <div class="tags"><span class="tag">Java 1.20.1</span><span class="tag mod">Modpack</span></div>
      </div>
      <div class="address">
        <code class="addr" id="a3">homesteadmc.jakesgames.xyz</code>
        <button class="copy" type="button" data-target="a3">Copy</button>
      </div>

      <div class="pack">
        <dl class="pack-grid">
          <div><dt>Modpack</dt><dd>Homestead</dd></div>
          <div><dt>Author</dt><dd>Toekimi</dd></div>
          <div><dt>Mod loader</dt><dd>Fabric</dd></div>
          <div><dt>Minecraft</dt><dd>1.20.1</dd></div>
          <div><dt>RAM</dt><dd>6 to 8 GB</dd></div>
          <div><dt>Quests</dt><dd>709 in 27 chapters</dd></div>
        </dl>

        <div>
          <p class="pack-label">Install</p>
          <ol class="steps">
            <li>Get Homestead from <a href="https://modrinth.com/modpack/homestead" target="_blank" rel="noopener">Modrinth</a> or <a href="https://www.curseforge.com/minecraft/modpacks/homestead-cozy" target="_blank" rel="noopener">CurseForge</a>, and import it into the matching app as a new instance.</li>
            <li>Give the instance 6 to 8 GB of memory. More than about 10 GB can make it run worse.</li>
            <li>Launch it once, then add the server address above in Multiplayer. The modpack is Java Edition only.</li>
          </ol>
        </div>

        <div>
          <p class="pack-label">Some of what's in it</p>
          <ul class="mods">
            <li>Create</li><li>Sophisticated Storage</li><li>Tectonic</li><li>Mushlings</li><li>Chipped</li><li>Cluttered</li><li>Complementary Reimagined</li>
          </ul>
        </div>
      </div>
    </section>
  </main>

  <footer>Not affiliated with Mojang or Microsoft. Need help joining? Ask in the Discord.</footer>
</div>
<span class="sr" id="live" role="status" aria-live="polite"></span>

<script>
  (function () {
    var live = document.getElementById("live");
    function selectText(el) {
      var r = document.createRange();
      r.selectNodeContents(el);
      var s = window.getSelection();
      s.removeAllRanges();
      s.addRange(r);
    }
    document.querySelectorAll(".copy").forEach(function (btn) {
      btn.addEventListener("click", function () {
        var el = document.getElementById(btn.dataset.target);
        var text = el.textContent.trim();
        function done(msg) {
          btn.textContent = msg;
          live.textContent = msg === "Copied" ? "Address copied" : "Address selected, press copy on your keyboard";
          setTimeout(function () { btn.textContent = "Copy"; }, 1800);
        }
        try {
          navigator.clipboard.writeText(text).then(function () { done("Copied"); }, function () { selectText(el); done("Selected"); });
        } catch (e) {
          selectText(el);
          done("Selected");
        }
      });
    });
  })();
</script>

</body></html>
