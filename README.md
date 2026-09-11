# XAUUSD MT5 Robot Control Center — v2

This build keeps the mobile controller on Netlify and adds the first real MT5 execution bridge.

## What is real now
- Netlify app can connect to an authenticated bridge.
- START / PAUSE / STOP / CLOSE ALL / CANCEL PENDING / RESET CYCLE commands are queued by the bridge.
- MT5 EA polls the bridge and executes those commands locally inside MT5.
- EA publishes account, balance, equity, symbol, bid/ask, margin, positions and basket P/L back to the bridge.
- EA discovers common XAUUSD broker suffixes such as `XAUUSD.ecn`, `XAUUSD.m`, `XAUUSDm`, and `GOLD`.
- Controlled XAUUSD grid with BUY STOP / SELL STOP pending orders, max levels, multiplier, max lot, basket target, drawdown and margin safeguards.

## Important
The Netlify website is still only the controller. A Windows PC/VPS with a continuously running MT5 terminal and this EA is required for actual broker execution.

Do not put an MT5 master password into the website. Pair the site to the bridge with the bridge token instead.

## 1. Deploy the website
Upload this whole project folder to Netlify Drop. The root must contain `index.html`, `styles.css`, `app.js`, and `netlify.toml`.

## 2. Run the bridge on Windows/VPS
Open a terminal in `bridge/`:

```text
npm install
```

Copy `.env.example` to `.env`, then set:

```text
PORT=8787
BRIDGE_TOKEN=your-long-random-token
CORS_ORIGIN=https://your-site.netlify.app
```

Start it:

```text
npm start
```

For internet access, expose the bridge through HTTPS. Do not expose plain HTTP with a production token.

## 3. Install the EA in MT5
1. Open MT5 → File → Open Data Folder.
2. Put `mt5_ea/XAUUSD_RemoteGridEA.mq5` in `MQL5/Experts/`.
3. Open MetaEditor and compile the EA.
4. Attach it to the broker's XAUUSD chart.
5. Set `InpBridgeURL` to the HTTPS bridge URL and `InpBridgeToken` to the same token.
6. In MT5 → Tools → Options → Expert Advisors, allow WebRequest to the bridge URL.
7. Keep Algo Trading enabled.

The EA does not start trading merely because it is attached. It waits for START from the controller.

## 4. Pair the mobile controller
Open the Netlify site → Settings → MT5 Bridge Connection.
Enter the bridge HTTPS URL and pairing token → CONNECT & VERIFY.

A successful bridge connection without an EA heartbeat is shown as offline. Start the EA and wait for the MT5 heartbeat.

## Safety defaults
XAUUSD | 0.01 lot | 10 USD grid step | 1.5x multiplier | 10 levels | 0.50 max lot | 10% max drawdown | 50% max margin usage | $5 basket target.

These are engineering defaults, not a profitability claim. Validate the strategy on DEMO first.
