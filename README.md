<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>NOVEX Exchange — BTC/USDT Perpetual</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet" />
<style>
  /* ============ RESET / TOKENS ============ */
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
  :root{
    --bg:#0b0e11;
    --panel:#12161c;
    --panel-2:#161b22;
    --panel-3:#1c222b;
    --border:#232a34;
    --text:#eaecef;
    --muted:#7f8a9a;
    --muted-2:#5a6472;
    --green:#0ecb81;
    --green-dim:rgba(14,203,129,.12);
    --red:#f6465d;
    --red-dim:rgba(246,70,93,.12);
    --accent:#f0b90b;
    --mono:'JetBrains Mono',ui-monospace,SFMono-Regular,Menlo,monospace;
    --sans:'Inter',system-ui,-apple-system,'Segoe UI',sans-serif;
  }
  html,body{height:100%}
  body{
    background:var(--bg);
    color:var(--text);
    font-family:var(--sans);
    font-size:13px;
    line-height:1.35;
    -webkit-font-smoothing:antialiased;
    overflow:hidden;
  }
  ::-webkit-scrollbar{width:6px;height:6px}
  ::-webkit-scrollbar-track{background:transparent}
  ::-webkit-scrollbar-thumb{background:#2a323d;border-radius:4px}
  ::-webkit-scrollbar-thumb:hover{background:#39434f}

  button{font-family:inherit;font-size:inherit;color:inherit;background:none;border:none;cursor:pointer}
  .mono{font-family:var(--mono);font-variant-numeric:tabular-nums}
  .up{color:var(--green)}
  .down{color:var(--red)}
  .muted{color:var(--muted)}

  /* ============ LAYOUT ============ */
  .app{
    display:grid;
    grid-template-rows:46px 28px minmax(0,1fr) 238px;
    height:100vh;
  }
  .panel{
    background:var(--panel);
    border:1px solid var(--border);
    border-radius:6px;
    display:flex;
    flex-direction:column;
    min-height:0;
    overflow:hidden;
  }
  .panel-head{
    display:flex;align-items:center;justify-content:space-between;
    padding:8px 10px;
    font-size:12px;font-weight:600;letter-spacing:.2px;
    border-bottom:1px solid var(--border);
    color:#c8d0dc;
    flex:0 0 auto;
  }
  .panel-head .hint{font-size:11px;font-weight:500;color:var(--muted-2)}

  /* ============ TOP BAR ============ */
  .topbar{
    display:flex;align-items:center;gap:20px;
    padding:0 14px;
    background:var(--panel);
    border-bottom:1px solid var(--border);
  }
  .brand{display:flex;align-items:center;gap:8px;font-weight:700;font-size:15px;letter-spacing:-.2px}
  .brand-mark{
    width:24px;height:24px;border-radius:6px;
    background:linear-gradient(140deg,#f0b90b,#f8d33a);
    color:#0b0e11;display:grid;place-items:center;
    font-size:13px;font-weight:700;
  }
  .brand span{color:var(--muted);font-weight:500;font-size:12px;margin-left:-2px}
  .topnav{display:flex;gap:2px;margin-left:6px}
  .topnav button{
    padding:6px 11px;border-radius:5px;color:var(--muted);
    font-size:12.5px;font-weight:500;transition:.15s;
  }
  .topnav button:hover{color:var(--text);background:var(--panel-2)}
  .topnav button.active{color:var(--text);background:var(--panel-3)}
  .top-right{margin-left:auto;display:flex;align-items:center;gap:16px}
  .status{display:flex;align-items:center;gap:6px;font-size:11.5px;color:var(--muted)}
  .dot{width:6px;height:6px;border-radius:50%;background:var(--green);box-shadow:0 0 0 3px rgba(14,203,129,.15);animation:pulse 2s infinite}
  @keyframes pulse{0%,100%{opacity:1}50%{opacity:.35}}
  .wallet{
    display:flex;align-items:center;gap:8px;
    background:var(--panel-2);border:1px solid var(--border);
    padding:5px 11px;border-radius:6px;font-size:12px;
  }
  .wallet .lbl{color:var(--muted);font-size:11px}
  .wallet .val{font-family:var(--mono);font-weight:600;font-size:12.5px}
  .avatar{
    width:30px;height:30px;border-radius:50%;
    background:linear-gradient(140deg,#2b3a52,#1a2432);
    border:1px solid #33415a;
    display:grid;place-items:center;
    font-size:11.5px;font-weight:600;color:#a9bbd4;cursor:pointer;
  }

  /* ============ TICKER TAPE ============ */
  .tape{
    overflow:hidden;background:#0d1117;
    border-bottom:1px solid var(--border);
    display:flex;align-items:center;
  }
  .tape-track{display:flex;gap:26px;white-space:nowrap;animation:scroll 46s linear infinite;padding-left:16px}
  .tape:hover .tape-track{animation-play-state:paused}
  @keyframes scroll{from{transform:translateX(0)}to{transform:translateX(-50%)}}
  .tape-item{display:flex;align-items:center;gap:7px;font-size:11.5px}
  .tape-item .sym{color:#aab4c2;font-weight:600}
  .tape-item .px{font-family:var(--mono);color:#dfe5ee}
  .tape-item .ch{font-family:var(--mono);font-size:11px}

  /* ============ MAIN GRID ============ */
  .main{
    display:grid;
    grid-template-columns:250px minmax(0,1fr) 300px;
    gap:8px;padding:8px;
    min-height:0;
  }
  .bottom{
    display:grid;
    grid-template-columns:330px minmax(0,1fr);
    gap:8px;padding:0 8px 8px;
    min-height:0;
  }

  /* ============ WATCHLIST ============ */
  .wl-tabs{display:flex;gap:4px;padding:6px 8px;border-bottom:1px solid var(--border);flex:0 0 auto}
  .wl-tabs button{
    padding:4px 9px;border-radius:4px;font-size:11.5px;font-weight:500;color:var(--muted);
  }
  .wl-tabs button.active{background:var(--panel-3);color:var(--text)}
  .wl-cols{
    display:grid;grid-template-columns:1fr auto auto;gap:8px;
    padding:6px 10px;font-size:10.5px;color:var(--muted-2);
    text-transform:uppercase;letter-spacing:.4px;font-weight:600;
    border-bottom:1px solid var(--border);flex:0 0 auto;
  }
  .wl-cols span:not(:first-child){text-align:right}
  .wl-list{overflow-y:auto;flex:1;min-height:0}
  .wl-row{
    display:grid;grid-template-columns:1fr auto auto;align-items:center;gap:8px;
    padding:7px 10px;cursor:pointer;border-left:2px solid transparent;
    transition:background .12s;
  }
  .wl-row:hover{background:var(--panel-2)}
  .wl-row.active{background:#171d26;border-left-color:var(--accent)}
  .wl-name{font-size:12.5px;font-weight:600;letter-spacing:-.1px}
  .wl-sub{font-size:10.5px;color:var(--muted-2);margin-top:1px}
  .wl-px{font-family:var(--mono);font-size:12px;text-align:right;color:#dfe5ee}
  .wl-ch{
    font-family:var(--mono);font-size:11px;text-align:right;
    padding:2px 6px;border-radius:4px;min-width:58px;
  }
  .wl-ch.pos{background:var(--green-dim);color:var(--green)}
  .wl-ch.neg{background:var(--red-dim);color:var(--red)}
  .spark{width:52px;height:20px;display:block}

  /* ============ CHART PANEL ============ */
  .chart-head{
    display:flex;align-items:center;gap:22px;
    padding:10px 12px;border-bottom:1px solid var(--border);
    flex:0 0 auto;flex-wrap:wrap;
  }
  .pair{display:flex;align-items:center;gap:9px}
  .pair-icon{
    width:32px;height:32px;border-radius:50%;
    background:linear-gradient(140deg,#f7931a,#ffb84d);
    display:grid;place-items:center;color:#1a1005;font-weight:700;font-size:15px;
  }
  .pair-name{font-size:14px;font-weight:700;display:flex;align-items:center;gap:6px}
  .tag{
    font-size:9.5px;font-weight:700;letter-spacing:.4px;
    background:var(--panel-3);color:#9fb0c6;
    padding:2px 5px;border-radius:3px;
  }
  .pair-sub{font-size:11px;color:var(--muted-2);margin-top:1px}
  .price-block{display:flex;align-items:baseline;gap:9px}
  .last-price{font-family:var(--mono);font-size:20px;font-weight:700;letter-spacing:-.5px;transition:color .25s}
  .last-price.flash-up{color:var(--green)}
  .last-price.flash-down{color:var(--red)}
  .price-chg{font-family:var(--mono);font-size:12px;font-weight:600}
  .head-stats{display:flex;gap:20px;margin-left:auto}
  .hs{display:flex;flex-direction:column;gap:2px}
  .hs .k{font-size:10.5px;color:var(--muted-2)}
  .hs .v{font-family:var(--mono);font-size:12px;color:#cdd6e2}

  .chart-toolbar{
    display:flex;align-items:center;gap:6px;
    padding:6px 10px;border-bottom:1px solid var(--border);
    flex:0 0 auto;font-size:11.5px;
  }
  .chart-toolbar .tf{
    padding:3px 8px;border-radius:4px;color:var(--muted);font-weight:500;
  }
  .chart-toolbar .tf:hover{background:var(--panel-2);color:var(--text)}
  .chart-toolbar .tf.active{background:var(--panel-3);color:var(--text)}
  .chart-toolbar .sep{width:1px;height:14px;background:var(--border);margin:0 4px}
  .chart-toolbar .right{margin-left:auto;display:flex;gap:12px;color:var(--muted-2);font-size:11px}

  .chart-wrap{flex:1;min-height:0;position:relative;padding:0 0 0 0}
  #chart{display:block;width:100%;height:100%;cursor:crosshair}

  /* ============ ORDER BOOK ============ */
  .ob-head,.ob-row{
    display:grid;grid-template-columns:1fr 1fr 1fr;gap:6px;
    padding:0 10px;font-size:11px;
  }
  .ob-head{
    color:var(--muted-2);padding-top:6px;padding-bottom:6px;
    border-bottom:1px solid var(--border);font-size:10.5px;
    text-transform:uppercase;letter-spacing:.3px;font-weight:600;
  }
  .ob-head span:not(:first-child),.ob-row span:not(:first-child){text-align:right}
  .ob-asks,.ob-bids{display:flex;flex-direction:column;justify-content:flex-end}
  .ob-asks{flex:1;min-height:0;overflow:hidden}
  .ob-bids{flex:1;min-height:0;overflow:hidden}
  .ob-row{
    position:relative;height:19px;line-height:19px;
    font-family:var(--mono);font-size:11.5px;cursor:default;
  }
  .ob-row:hover{background:#1b212a}
  .ob-row .bar{position:absolute;top:0;bottom:0;right:0;pointer-events:none}
  .ob-row.ask .bar{background:var(--red-dim)}
  .ob-row.bid .bar{background:var(--green-dim)}
  .ob-row.ask .p{color:var(--red)}
  .ob-row.bid .p{color:var(--green)}
  .ob-row .a,.ob-row .t{color:#aeb8c6}
  .ob-row span{position:relative;z-index:1}

  .ob-mid{
    display:flex;align-items:center;justify-content:space-between;
    padding:7px 10px;border-top:1px solid var(--border);border-bottom:1px solid var(--border);
    background:var(--panel-2);flex:0 0 auto;
  }
  .ob-mid .last{font-family:var(--mono);font-size:14px;font-weight:700}
  .ob-mid .spread{font-size:10.5px;color:var(--muted-2)}

  .trades-head{
    padding:7px 10px;font-size:11px;font-weight:600;color:#c8d0dc;
    border-bottom:1px solid var(--border);display:flex;justify-content:space-between;
    flex:0 0 auto;
  }
  .trades-head .sub{color:var(--muted-2);font-weight:500;font-size:10.5px}
  .trades{height:118px;overflow:hidden;flex:0 0 auto;padding-bottom:2px}
  .trade-row{
    display:grid;grid-template-columns:1fr 1fr .8fr;gap:6px;
    padding:0 10px;height:18px;line-height:18px;
    font-family:var(--mono);font-size:11px;
  }
  .trade-row span:not(:first-child){text-align:right}
  .trade-row .a{color:#aeb8c6}
  .trade-row .t{color:var(--muted-2)}

  /* ============ ORDER FORM ============ */
  .of-tabs{display:grid;grid-template-columns:1fr 1fr;gap:6px;padding:8px;flex:0 0 auto}
  .of-tabs button{
    padding:7px;border-radius:5px;font-size:12.5px;font-weight:600;
    background:var(--panel-2);color:var(--muted);transition:.15s;
  }
  .of-tabs button.active.buy{background:var(--green);color:#04120c}
  .of-tabs button.active.sell{background:var(--red);color:#1a0509}
  .of-body{padding:2px 12px 12px;display:flex;flex-direction:column;gap:9px;overflow-y:auto;flex:1;min-height:0}
  .of-row{display:flex;justify-content:space-between;font-size:11.5px;color:var(--muted)}
  .of-row .v{font-family:var(--mono);color:#c8d0dc}
  .of-input{
    display:flex;align-items:center;
    background:var(--panel-2);border:1px solid var(--border);
    border-radius:5px;padding:0 10px;height:36px;
    transition:border-color .15s;
  }
  .of-input:focus-within{border-color:#3d4b60}
  .of-input input{
    flex:1;background:none;border:none;outline:none;
    color:var(--text);font-family:var(--mono);font-size:14px;font-weight:500;
    width:100%;
  }
  .of-input .suffix{font-size:11px;color:var(--muted-2);font-weight:600;letter-spacing:.3px}
  .of-slider{padding:2px 0}
  .of-slider input{
    -webkit-appearance:none;width:100%;height:4px;border-radius:3px;
    background:linear-gradient(90deg,var(--accent) 25%,#2a323d 25%);
    outline:none;cursor:pointer;
  }
  .of-slider input::-webkit-slider-thumb{
    -webkit-appearance:none;width:14px;height:14px;border-radius:50%;
    background:#e8edf5;border:2px solid var(--accent);cursor:grab;
  }
  .of-pct{display:grid;grid-template-columns:repeat(4,1fr);gap:5px}
  .of-pct button{
    padding:4px 0;border-radius:4px;background:var(--panel-2);
    border:1px solid var(--border);font-size:11px;color:var(--muted);
    font-family:var(--mono);transition:.12s;
  }
  .of-pct button:hover{background:var(--panel-3);color:var(--text)}
  .of-submit{
    margin-top:auto;padding:11px;border-radius:5px;
    font-size:13px;font-weight:700;letter-spacing:.2px;
    background:var(--green);color:#04120c;transition:filter .15s;
  }
  .of-submit:hover{filter:brightness(1.08)}
  .of-submit.sell{background:var(--red);color:#1a0509}

  /* ============ ORDERS TABLE ============ */
  .tabs{display:flex;gap:2px;padding:6px 8px 0;border-bottom:1px solid var(--border);flex:0 0 auto}
  .tabs button{
    padding:6px 12px;font-size:12px;font-weight:500;color:var(--muted);
    border-bottom:2px solid transparent;margin-bottom:-1px;transition:.15s;
  }
  .tabs button:hover{color:#c8d0dc}
  .tabs button.active{color:var(--text);border-bottom-color:var(--accent)}
  .tabs .count{font-family:var(--mono);font-size:10.5px;color:var(--muted-2);margin-left:5px}
  .table-wrap{overflow:auto;flex:1;min-height:0}
  table{width:100%;border-collapse:collapse;font-size:12px}
  thead th{
    position:sticky;top:0;background:var(--panel);
    text-align:right;padding:7px 12px;font-size:10.5px;font-weight:600;
    color:var(--muted-2);text-transform:uppercase;letter-spacing:.4px;
    border-bottom:1px solid var(--border);z-index:1;
  }
  thead th:first-child{text-align:left}
  tbody td{
    padding:7px 12px;text-align:right;font-family:var(--mono);font-size:11.5px;
    border-bottom:1px solid #1a1f27;color:#c3ccd9;white-space:nowrap;
  }
  tbody td:first-child{text-align:left;font-family:var(--sans);font-weight:600}
  tbody tr:hover{background:var(--panel-2)}
  .side{font-size:10px;font-weight:700;padding:2px 6px;border-radius:3px;letter-spacing:.3px}
  .side.buy{background:var(--green-dim);color:var(--green)}
  .side.sell{background:var(--red-dim);color:var(--red)}
  .cancel{
    font-size:11px;color:var(--muted);border:1px solid var(--border);
    padding:2px 8px;border-radius:4px;transition:.12s;
  }
  .cancel:hover{color:var(--red);border-color:var(--red)}

  /* ============ RESPONSIVE ============ */
  @media (max-width:1180px){
    .main{grid-template-columns:210px minmax(0,1fr) 260px}
    .head-stats{display:none}
  }
  @media (max-width:920px){
    .app{grid-template-rows:46px 28px minmax(0,1fr) auto}
    .main{grid-template-columns:minmax(0,1fr)}
    .watchlist,.orderbook{display:none}
    .bottom{grid-template-columns:1fr}
    .orders{min-height:220px}
    .topnav{display:none}
  }

  /* toast */
  .toast{
    position:fixed;bottom:20px;left:50%;transform:translate(-50%,20px);
    background:var(--panel-3);border:1px solid var(--border);
    padding:10px 18px;border-radius:6px;font-size:12.5px;font-weight:500;
    opacity:0;pointer-events:none;transition:.25s;z-index:50;
    box-shadow:0 8px 26px rgba(0,0,0,.5);
  }
  .toast.show{opacity:1;transform:translate(-50%,0)}
</style>
</head>
<body>

<div class="app">

  <!-- ================= TOP BAR ================= -->
  <header class="topbar">
    <div class="brand"><div class="brand-mark">N</div>NOVEX <span>Exchange</span></div>
    <nav class="topnav">
      <button class="active">Trade</button>
      <button>Markets</button>
      <button>Futures</button>
      <button>Earn</button>
      <button>Portfolio</button>
    </nav>
    <div class="top-right">
      <div class="status"><span class="dot"></span> Live</div>
      <div class="wallet">
        <span class="lbl">Balance</span>
        <span class="val">24,860.42 <span class="muted" style="font-size:10.5px">USDT</span></span>
      </div>
      <div class="avatar">AK</div>
    </div>
  </header>

  <!-- ================= TICKER TAPE ================= -->
  <div class="tape">
    <div class="tape-track" id="tapeTrack"></div>
  </div>

  <!-- ================= MAIN ================= -->
  <div class="main">

    <!-- WATCHLIST -->
    <aside class="panel watchlist">
      <div class="panel-head"><span>Watchlist</span><span class="hint">⌕</span></div>
      <div class="wl-tabs">
        <button class="active">Favorites</button>
        <button>USDⓈ-M</button>
        <button>Spot</button>
      </div>
      <div class="wl-cols"><span>Pair</span><span>Price</span><span>24h</span></div>
      <div class="wl-list" id="wlList"></div>
    </aside>

    <!-- CHART -->
    <section class="panel chart-panel">
      <div class="chart-head">
        <div class="pair">
          <div class="pair-icon">₿</div>
          <div>
            <div class="pair-name">BTC/USDT <span class="tag">PERP</span></div>
            <div class="pair-sub">Bitcoin Perpetual · Cross 20x</div>
          </div>
        </div>
        <div class="price-block">
          <div class="last-price mono" id="lastPrice">67,420.00</div>
          <div class="price-chg mono up" id="priceChg">+1.24%</div>
        </div>
        <div class="head-stats">
          <div class="hs"><span class="k">24h High</span><span class="v" id="h24">68,120.50</span></div>
          <div class="hs"><span class="k">24h Low</span><span class="v" id="l24">65,940.20</span></div>
          <div class="hs"><span class="k">24h Volume</span><span class="v" id="v24">18.42K BTC</span></div>
          <div class="hs"><span class="k">Funding</span><span class="v up">0.0100%</span></div>
        </div>
      </div>

      <div class="chart-toolbar">
        <button class="tf">1m</button>
        <button class="tf active">5m</button>
        <button class="tf">15m</button>
        <button class="tf">1H</button>
        <button class="tf">4H</button>
        <button class="tf">1D</button>
        <span class="sep"></span>
        <button class="tf">Indicators</button>
        <span class="right"><span>O 67,410.00</span><span>H 67,502.00</span><span>L 67,388.50</span><span>C 67,420.00</span></span>
      </div>

      <div class="chart-wrap"><canvas id="chart"></canvas></div>
    </section>

    <!-- ORDER BOOK -->
    <aside class="panel orderbook">
      <div class="panel-head"><span>Order Book</span><span class="hint">Tick 1.0 ▾</span></div>
      <div class="ob-head"><span>Price (USDT)</span><span>Amount (BTC)</span><span>Total</span></div>
      <div class="ob-asks" id="obAsks"></div>
      <div class="ob-mid">
        <span class="last mono up" id="obLast">67,420.00</span>
        <span class="spread">Spread 1.00 · 0.001%</span>
      </div>
      <div class="ob-bids" id="obBids"></div>
      <div class="trades-head"><span>Market Trades</span><span class="sub">Time</span></div>
      <div class="trades" id="trades"></div>
    </aside>
  </div>

  <!-- ================= BOTTOM ================= -->
  <div class="bottom">

    <!-- ORDER FORM -->
    <section class="panel order-form">
      <div class="of-tabs">
        <button class="buy active" data-side="buy">Buy</button>
        <button class="sell" data-side="sell">Sell</button>
      </div>
      <div class="of-body">
        <div class="of-row"><span>Available</span><span class="v">9,420.15 USDT</span></div>

        <div class="of-input">
          <input type="text" id="priceInput" value="67420" inputmode="decimal" />
          <span class="suffix">USDT</span>
        </div>

        <div class="of-slider">
          <input type="range" id="pctSlider" min="0" max="100" value="25" />
        </div>
        <div class="of-pct">
          <button data-pct="25">25%</button>
          <button data-pct="50">50%</button>
          <button data-pct="75">75%</button>
          <button data-pct="100">100%</button>
        </div>

        <div class="of-input">
          <input type="text" id="amountInput" value="0.1397" inputmode="decimal" />
          <span class="suffix">BTC</span>
        </div>

        <div class="of-row"><span>Est. Total</span><span class="v" id="estTotal">9,420.15 USDT</span></div>
        <div class="of-row"><span>Fee (0.04%)</span><span class="v" id="feeVal">3.77 USDT</span></div>

        <button class="of-submit" id="submitOrder">Buy BTC</button>
      </div>
    </section>

    <!-- ORDERS / POSITIONS -->
    <section class="panel orders">
      <div class="tabs">
        <button class="active">Open Orders <span class="count" id="openCount">3</span></button>
        <button>Positions <span class="count">2</span></button>
        <button>Order History</button>
        <button>Trade History</button>
      </div>
      <div class="table-wrap">
        <table>
          <thead>
            <tr>
              <th>Pair</th><th>Side</th><th>Type</th><th>Price (USDT)</th>
              <th>Amount (BTC)</th><th>Filled</th><th>Time</th><th></th>
            </tr>
          </thead>
          <tbody id="ordersBody"></tbody>
        </table>
      </div>
    </section>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
/* =========================================================
   HELPERS
   ========================================================= */
const $  = (s, r = document) => r.querySelector(s);
const $$ = (s, r = document) => [...r.querySelectorAll(s)];
const fmt  = (n, d = 2) => n.toLocaleString('en-US', { minimumFractionDigits: d, maximumFractionDigits: d });
const fmtK = n => n >= 1000 ? fmt(n, 2) : fmt(n, n < 1 ? 4 : 2);
const rnd  = (a, b) => a + Math.random() * (b - a);
const pad2 = n => String(n).padStart(2, '0');

const SYMBOL = 'BTC/USDT';
let lastPrice = 67420.00;
let open24h   = 66592.10;

/* =========================================================
   TICKER TAPE
   ========================================================= */
const tapeData = [
  ['BTC/USDT', 67420.00, 1.24], ['ETH/USDT', 3284.52, 0.86],
  ['SOL/USDT', 168.42, -2.14],  ['BNB/USDT', 598.20, 0.42],
  ['XRP/USDT', 0.5842, -1.02],  ['ADA/USDT', 0.4418, 1.31],
  ['DOGE/USDT', 0.1385, 2.87],  ['AVAX/USDT', 27.84, -0.63],
  ['LINK/USDT', 14.62, 1.09],   ['DOT/USDT', 6.24, -0.88]
];
const tapeTrack = $('#tapeTrack');
const tapeHTML = tapeData.map(([s, p, c]) => {
  const cls = c >= 0 ? 'up' : 'down';
  const sign = c >= 0 ? '+' : '';
  return `<div class="tape-item">
      <span class="sym">${s}</span>
      <span class="px">${fmt(p, p < 1 ? 4 : 2)}</span>
      <span class="ch ${cls}">${sign}${c.toFixed(2)}%</span>
    </div>`;
}).join('');
tapeTrack.innerHTML = tapeHTML + tapeHTML;

/* =========================================================
   CANDLESTICK ENGINE
   ========================================================= */
const canvas = $('#chart');
const ctx = canvas.getContext('2d');
let W = 0, H = 0;
let candles = [];
let mouse = null;
const VISIBLE = 90;

function makeCandles(n) {
  let p = 66500;
  const out = [];
  const now = Date.now();
  for (let i = n; i > 0; i--) {
    const o = p;
    const vol = p * 0.0028;
    const c = o + (Math.random() - 0.47) * vol;
    const h = Math.max(o, c) + Math.random() * vol * 0.75;
    const l = Math.min(o, c) - Math.random() * vol * 0.75;
    out.push({ t: now - i * 300000, o, h, l, c, v: rnd(25, 130) });
    p = c;
  }
  // nudge final close toward the live price
  out[out.length - 1].c = lastPrice;
  out[out.length - 1].h = Math.max(out[out.length - 1].h, lastPrice);
  out[out.length - 1].l = Math.min(out[out.length - 1].l, lastPrice);
  return out;
}
candles = makeCandles(160);

function resizeChart() {
  const rect = canvas.parentElement.getBoundingClientRect();
  const dpr = window.devicePixelRatio || 1;
  W = Math.max(1, rect.width);
  H = Math.max(1, rect.height);
  canvas.width = W * dpr;
  canvas.height = H * dpr;
  canvas.style.width = W + 'px';
  canvas.style.height = H + 'px';
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  drawChart();
}

function drawChart() {
  if (!W || !H) return;
  ctx.clearRect(0, 0, W, H);

  const data = candles.slice(-VISIBLE);
  const n = data.length;
  if (!n) return;

  const axisW = 64, padTop = 14, padBot = 22;
  const volH = Math.max(38, H * 0.15);
  const plotX = 4;
  const plotW = Math.max(20, W - axisW - plotX);
  const plotY = padTop;
  const plotH = Math.max(20, H - padTop - padBot - volH - 6);

  let hi = -Infinity, lo = Infinity;
  for (const c of data) { if (c.h > hi) hi = c.h; if (c.l < lo) lo = c.l; }
  const pad = (hi - lo) * 0.1 || 1;
  hi += pad; lo -= pad;

  const yFor = p => plotY + (hi - p) / (hi - lo) * plotH;

  /* --- grid + price axis --- */
  ctx.font = '11px ui-monospace, Menlo, monospace';
  ctx.textBaseline = 'middle';
  const ROWS = 5;
  for (let i = 0; i <= ROWS; i++) {
    const y = Math.round(plotY + plotH * i / ROWS) + 0.5;
    ctx.strokeStyle = '#1a1f27';
    ctx.beginPath(); ctx.moveTo(plotX, y); ctx.lineTo(plotX + plotW, y); ctx.stroke();

    const p = hi - (hi - lo) * i / ROWS;
    ctx.fillStyle = '#5e6673';
    ctx.textAlign = 'left';
    ctx.fillText(fmt(p, 2), plotX + plotW + 8, y);
  }

  /* --- volume bars --- */
  const step = plotW / n;
  const bodyW = Math.max(1.5, step * 0.62);
  let maxVol = 0;
  for (const c of data) maxVol = Math.max(maxVol, c.v);
  const volTop = plotY + plotH + 6;

  data.forEach((c, i) => {
    const x = plotX + i * step + step / 2;
    const vh = (c.v / maxVol) * (volH - 6);
    const up = c.c >= c.o;
    ctx.fillStyle = up ? 'rgba(14,203,129,.28)' : 'rgba(246,70,93,.28)';
    ctx.fillRect(x - bodyW / 2, volTop + (volH - 6) - vh, bodyW, vh);
  });

  /* --- candles --- */
  data.forEach((c, i) => {
    const x = plotX + i * step + step / 2;
    const up = c.c >= c.o;
    const color = up ? '#0ecb81' : '#f6465d';

    // wick
    ctx.strokeStyle = color;
    ctx.lineWidth = 1;
    ctx.beginPath();
    ctx.moveTo(Math.round(x) + 0.5, yFor(c.h));
    ctx.lineTo(Math.round(x) + 0.5, yFor(c.l));
    ctx.stroke();

    // body
    const yO = yFor(c.o), yC = yFor(c.c);
    const top = Math.min(yO, yC);
    const bh = Math.max(1, Math.abs(yC - yO));
    ctx.fillStyle = color;
    ctx.fillRect(x - bodyW / 2, top, bodyW, bh);
  });

  /* --- last price line --- */
  const lastY = yFor(lastPrice);
  if (lastY > plotY && lastY < plotY + plotH) {
    ctx.save();
    ctx.setLineDash([4, 4]);
    ctx.strokeStyle = lastPrice >= candles[candles.length - 1].o ? '#0ecb81' : '#f6465d';
    ctx.globalAlpha = 0.6;
    ctx.beginPath();
    ctx.moveTo(plotX, Math.round(lastY) + 0.5);
    ctx.lineTo(plotX + plotW, Math.round(lastY) + 0.5);
    ctx.stroke();
    ctx.restore();

    // price tag
    const tagColor = lastPrice >= candles[candles.length - 1].o ? '#0ecb81' : '#f6465d';
    ctx.fillStyle = tagColor;
    const tagY = Math.round(lastY) - 9;
    ctx.fillRect(plotX + plotW + 2, tagY, axisW - 4, 18);
    ctx.fillStyle = '#04120c';
    ctx.font = '600 11px ui-monospace, Menlo, monospace';
    ctx.textAlign = 'center';
    ctx.fillText(fmt(lastPrice, 2), plotX + plotW + axisW / 2, lastY);
  }

  /* --- time axis --- */
  ctx.fillStyle = '#5e6673';
  ctx.font = '11px ui-monospace, Menlo, monospace';
  ctx.textAlign = 'center';
  const ticks = 5;
  for (let i = 0; i < ticks; i++) {
    const idx = Math.min(n - 1, Math.round(i * (n - 1) / (ticks - 1)));
    const d = new Date(data[idx].t);
    const x = plotX + idx * step + step / 2;
    ctx.fillText(`${pad2(d.getHours())}:${pad2(d.getMinutes())}`, x, H - padBot / 2 + 1);
  }

  /* --- crosshair --- */
  if (mouse && mouse.x > plotX && mouse.x < plotX + plotW &&
      mouse.y > plotY && mouse.y < plotY + plotH) {
    ctx.save();
    ctx.setLineDash([3, 3]);
    ctx.strokeStyle = '#3c4657';
    ctx.lineWidth = 1;

    ctx.beginPath();
    ctx.moveTo(Math.round(mouse.x) + 0.5, plotY);
    ctx.lineTo(Math.round(mouse.x) + 0.5, plotY + plotH);
    ctx.stroke();

    ctx.beginPath();
    ctx.moveTo(plotX, Math.round(mouse.y) + 0.5);
    ctx.lineTo(plotX + plotW, Math.round(mouse.y) + 0.5);
    ctx.stroke();
    ctx.restore();

    // y label
    const pv = hi - (mouse.y - plotY) / plotH * (hi - lo);
    ctx.fillStyle = '#2b3444';
    ctx.fillRect(plotX + plotW + 2, mouse.y - 9, axisW - 4, 18);
    ctx.fillStyle = '#dfe5ee';
    ctx.font = '600 11px ui-monospace, Menlo, monospace';
    ctx.textAlign = 'center';
    ctx.fillText(fmt(pv, 2), plotX + plotW + axisW / 2, mouse.y);

    // x label
    const idx = Math.max(0, Math.min(n - 1, Math.floor((mouse.x - plotX) / step)));
    const d = new Date(data[idx].t);
    const lbl = `${pad2(d.getHours())}:${pad2(d.getMinutes())}`;
    ctx.fillStyle = '#2b3444';
    ctx.fillRect(mouse.x - 22, plotY + plotH + 2, 44, 16);
    ctx.fillStyle = '#dfe5ee';
    ctx.font = '600 10.5px ui-monospace, Menlo, monospace';
    ctx.fillText(lbl, mouse.x, plotY + plotH + 10);
  }
}

canvas.addEventListener('mousemove', e => {
  const r = canvas.getBoundingClientRect();
  mouse = { x: e.clientX - r.left, y: e.clientY - r.top };
  drawChart();
});
canvas.addEventListener('mouseleave', () => { mouse = null; drawChart(); });
new ResizeObserver(resizeChart).observe(canvas.parentElement);

/* =========================================================
   ORDER BOOK
   ========================================================= */
const obAsks = $('#obAsks');
const obBids = $('#obBids');
const LEVELS = 11;
const TICK = 1;

function buildBook(price) {
  let cumAsk = 0, cumBid = 0;
  const asks = [], bids = [];

  for (let i = 1; i <= LEVELS; i++) {
    const amtA = rnd(0.04, 1.8);
    cumAsk += amtA;
    asks.push({ p: price + i * TICK, a: amtA, c: cumAsk });

    const amtB = rnd(0.04, 1.8);
    cumBid += amtB;
    bids.push({ p: price - i * TICK, a: amtB, c: cumBid });
  }

  const maxAsk = asks[asks.length - 1].c;
  const maxBid = bids[bids.length - 1].c;

  obAsks.innerHTML = asks.slice().reverse().map(l => `
    <div class="ob-row ask">
      <div class="bar" style="width:${(l.c / maxAsk * 100).toFixed(1)}%"></div>
      <span class="p">${fmt(l.p, 2)}</span>
      <span class="a">${l.a.toFixed(4)}</span>
      <span class="t">${l.c.toFixed(3)}</span>
    </div>`).join('');

  obBids.innerHTML = bids.map(l => `
    <div class="ob-row bid">
      <div class="bar" style="width:${(l.c / maxBid * 100).toFixed(1)}%"></div>
      <span class="p">${fmt(l.p, 2)}</span>
      <span class="a">${l.a.toFixed(4)}</span>
      <span class="t">${l.c.toFixed(3)}</span>
    </div>`).join('');
}

/* =========================================================
   MARKET TRADES
   ========================================================= */
const tradesEl = $('#trades');
let tradeLog = [];

function pushTrade(price) {
  const side = Math.random() > 0.5 ? 'up' : 'down';
  const amount = rnd(0.002, 0.85);
  const d = new Date();
  tradeLog.unshift({
    p: price + (Math.random() > 0.5 ? TICK : -TICK) * (Math.random() > 0.6 ? 1 : 0),
    a: amount, side,
    t: `${pad2(d.getHours())}:${pad2(d.getMinutes())}:${pad2(d.getSeconds())}`
  });
  if (tradeLog.length > 12) tradeLog.pop();

  tradesEl.innerHTML = tradeLog.map(t => `
    <div class="trade-row">
      <span class="${t.side}">${fmt(t.p, 2)}</span>
      <span class="a">${t.a.toFixed(4)}</span>
      <span class="t">${t.t}</span>
    </div>`).join('');
}

/* =========================================================
   WATCHLIST
   ========================================================= */
const watchPairs = [
  { sym: 'BTC/USDT', name: 'Bitcoin',  price: 67420.00, chg: 1.24,  dec: 2 },
  { sym: 'ETH/USDT', name: 'Ethereum', price: 3284.52,  chg: 0.86,  dec: 2 },
  { sym: 'SOL/USDT', name: 'Solana',   price: 168.42,   chg: -2.14, dec: 2 },
  { sym: 'BNB/USDT', name: 'BNB',      price: 598.20,   chg: 0.42,  dec: 2 },
  { sym: 'XRP/USDT', name: 'XRP',      price: 0.5842,   chg: -1.02, dec: 4 },
  { sym: 'DOGE/USDT',name: 'Dogecoin', price: 0.1385,   chg: 2.87,  dec: 4 },
  { sym: 'AVAX/USDT',name: 'Avalanche',price: 27.84,    chg: -0.63, dec: 2 },
  { sym: 'LINK/USDT',name: 'Chainlink',price: 14.62,    chg: 1.09,  dec: 2 }
];

watchPairs.forEach(w => {
  w.series = Array.from({ length: 28 }, () => rnd(-1, 1));
});

const wlList = $('#wlList');

function sparkline(series, positive) {
  const w = 52, h = 20, min = Math.min(...series), max = Math.max(...series);
  const range = max - min || 1;
  const pts = series.map((v, i) =>
    `${(i / (series.length - 1) * w).toFixed(1)},${(h - (v - min) / range * (h - 2) - 1).toFixed(1)}`
  ).join(' ');
  return `<svg class="spark" viewBox="0 0 ${w} ${h}" preserveAspectRatio="none">
    <polyline points="${pts}" fill="none" stroke="${positive ? '#0ecb81' : '#f6465d'}"
      stroke-width="1.4" stroke-linejoin="round" stroke-linecap="round"/>
  </svg>`;
}

function renderWatchlist() {
  wlList.innerHTML = watchPairs.map((w, i) => {
    const pos = w.chg >= 0;
    return `<div class="wl-row ${i === 0 ? 'active' : ''}" data-i="${i}">
      <div>
        <div class="wl-name">${w.sym.split('/')[0]}<span class="muted" style="font-weight:500">/${w.sym.split('/')[1]}</span></div>
        <div class="wl-sub">${w.name}</div>
      </div>
      <div class="wl-px">${fmt(w.price, w.dec)}</div>
      <div>
        <div class="wl-ch ${pos ? 'pos' : 'neg'}">${pos ? '+' : ''}${w.chg.toFixed(2)}%</div>
        ${sparkline(w.series, pos)}
      </div>
    </div>`;
  }).join('');
}
renderWatchlist();

wlList.addEventListener('click', e => {
  const row = e.target.closest('.wl-row');
  if (!row) return;
  $$('.wl-row').forEach(r => r.classList.remove('active'));
  row.classList.add('active');
});
</script>
<script>
/* =========================================================
   ORDER FORM LOGIC
   ========================================================= */
const priceInput  = $('#priceInput');
const amountInput = $('#amountInput');
const estTotalEl  = $('#estTotal');
const feeValEl    = $('#feeVal');
const slider      = $('#pctSlider');
const submitBtn   = $('#submitOrder');
const toastEl     = $('#toast');
let side = 'buy';
const AVAILABLE = 9420.15;

function setSide(s) {
  side = s;
  $$('.of-tabs button').forEach(b => b.classList.toggle('active', b.dataset.side === s));
  submitBtn.classList.toggle('sell', s === 'sell');
  submitBtn.textContent = s === 'buy' ? 'Buy BTC' : 'Sell BTC';
  updateTotals();
}

$$('.of-tabs button').forEach(b => b.addEventListener('click', () => setSide(b.dataset.side)));

function updateTotals() {
  const price  = parseFloat(priceInput.value.replace(/,/g, '')) || 0;
  const amount = parseFloat(amountInput.value) || 0;
  const total  = price * amount;
  const fee    = total * 0.0004;
  estTotalEl.textContent = fmt(total, 2) + ' USDT';
  feeValEl.textContent   = fmt(fee, 2) + ' USDT';
}

function applyPercent(pct) {
  const price = parseFloat(priceInput.value.replace(/,/g, '')) || lastPrice;
  const amount = (AVAILABLE * pct / 100) / price;
  amountInput.value = amount.toFixed(4);
  slider.value = pct;
  slider.style.background =
    `linear-gradient(90deg,var(--accent) ${pct}%,#2a323d ${pct}%)`;
  updateTotals();
}

$$('.of-pct button').forEach(b => {
  b.addEventListener('click', () => applyPercent(Number(b.dataset.pct)));
});

slider.addEventListener('input', () => applyPercent(Number(slider.value)));
priceInput.addEventListener('input', updateTotals);
amountInput.addEventListener('input', updateTotals);

function showToast(msg, ok = true) {
  toastEl.textContent = msg;
  toastEl.style.borderColor = ok ? 'rgba(14,203,129,.5)' : 'rgba(246,70,93,.5)';
  toastEl.classList.add('show');
  clearTimeout(showToast._t);
  showToast._t = setTimeout(() => toastEl.classList.remove('show'), 2200);
}

/* =========================================================
   ORDERS TABLE
   ========================================================= */
const ordersBody = $('#ordersBody');
let orderSeq = 4;

const seedOrders = [
  { pair: 'BTC/USDT', side: 'buy',  type: 'Limit',  price: 66580.00, amount: 0.2500, filled: 0.00, time: '14:32:08' },
  { pair: 'ETH/USDT', side: 'sell', type: 'Limit',  price: 3350.00,  amount: 1.8500, filled: 0.60, time: '13:58:44' },
  { pair: 'SOL/USDT', side: 'buy',  type: 'Stop',   price: 162.40,   amount: 12.500, filled: 0.00, time: '12:11:19' }
];

function orderRow(o) {
  const fillPct = Math.min(100, (o.filled / o.amount) * 100);
  return `<tr data-id="${o.id}">
    <td>${o.pair}</td>
    <td><span class="side ${o.side}">${o.side.toUpperCase()}</span></td>
    <td style="color:#9aa5b5">${o.type}</td>
    <td>${fmt(o.price, 2)}</td>
    <td>${o.amount.toFixed(4)}</td>
    <td>
      <div style="display:flex;align-items:center;gap:6px;justify-content:flex-end">
        <span>${fillPct.toFixed(0)}%</span>
        <span style="width:38px;height:3px;background:#252d38;border-radius:3px;overflow:hidden;display:inline-block">
          <span style="display:block;width:${fillPct}%;height:100%;background:${o.side === 'buy' ? '#0ecb81' : '#f6465d'}"></span>
        </span>
      </div>
    </td>
    <td style="color:#7f8a9a">${o.time}</td>
    <td style="text-align:right"><button class="cancel" data-cancel="${o.id}">Cancel</button></td>
  </tr>`;
}

let orders = seedOrders.map((o, i) => ({ ...o, id: 'ord-' + (i + 1) }));
function renderOrders() {
  ordersBody.innerHTML = orders.map(orderRow).join('');
  $('#openCount').textContent = orders.length;
}
renderOrders();

ordersBody.addEventListener('click', e => {
  const btn = e.target.closest('[data-cancel]');
  if (!btn) return;
  const id = btn.dataset.cancel;
  orders = orders.filter(o => o.id !== id);
  renderOrders();
  showToast('Order cancelled', false);
});

submitBtn.addEventListener('click', () => {
  const price  = parseFloat(priceInput.value.replace(/,/g, '')) || lastPrice;
  const amount = parseFloat(amountInput.value) || 0;
  if (amount <= 0) { showToast('Enter a valid amount', false); return; }

  const d = new Date();
  orders.unshift({
    id: 'ord-' + (++orderSeq),
    pair: SYMBOL,
    side,
    type: 'Limit',
    price,
    amount,
    filled: 0,
    time: `${pad2(d.getHours())}:${pad2(d.getMinutes())}:${pad2(d.getSeconds())}`
  });
  if (orders.length > 8) orders.pop();
  renderOrders();
  showToast(`${side === 'buy' ? 'Buy' : 'Sell'} order placed · ${amount.toFixed(4)} BTC @ ${fmt(price, 2)}`);
});

/* =========================================================
   LIVE DATA LOOP
   ========================================================= */
const lastPriceEl = $('#lastPrice');
const priceChgEl  = $('#priceChg');
const obLastEl    = $('#obLast');
let tickCount = 0;

function updateHeader(price) {
  lastPriceEl.textContent = fmt(price, 2);
  const chg = ((price - open24h) / open24h) * 100;
  const pos = chg >= 0;
  priceChgEl.textContent = `${pos ? '+' : ''}${chg.toFixed(2)}%`;
  priceChgEl.className = 'price-chg mono ' + (pos ? 'up' : 'down');
  obLastEl.textContent = fmt(price, 2);
  obLastEl.className = 'last mono ' + (pos ? 'up' : 'down');

  $('#h24').textContent = fmt(price * 1.0102, 2);
  $('#l24').textContent = fmt(price * 0.9785, 2);
  $('#v24').textContent = (18.42 + Math.random() * 0.3).toFixed(2) + 'K BTC';

  lastPriceEl.classList.remove('flash-up', 'flash-down');
  void lastPriceEl.offsetWidth;
  lastPriceEl.classList.add(pos ? 'flash-up' : 'flash-down');
  setTimeout(() => lastPriceEl.classList.remove('flash-up', 'flash-down'), 260);
}

function tick() {
  /* --- price walk --- */
  const last = candles[candles.length - 1];
  const drift = (Math.random() - 0.5) * last.c * 0.0009;
  lastPrice = Math.round((last.c + drift) * 100) / 100;

  last.c = lastPrice;
  last.h = Math.max(last.h, lastPrice);
  last.l = Math.min(last.l, lastPrice);
  last.v += rnd(1, 9);

  tickCount++;

  /* --- new candle every 7 ticks --- */
  if (tickCount % 7 === 0) {
    const now = Date.now();
    candles.push({
      t: now, o: lastPrice, h: lastPrice, l: lastPrice, c: lastPrice, v: rnd(10, 40)
    });
    if (candles.length > 300) candles.shift();
  }

  /* --- refresh UI --- */
  updateHeader(lastPrice);
  buildBook(lastPrice);
  if (tickCount % 2 === 0) pushTrade(lastPrice);
  drawChart();

  /* --- watchlist drift --- */
  watchPairs.forEach((w, i) => {
    if (i === 0) {
      w.price = lastPrice;
      w.chg = ((lastPrice - open24h) / open24h) * 100;
    } else {
      const d = (Math.random() - 0.5) * w.price * 0.0008;
      w.price = Math.max(0.0001, w.price + d);
      w.chg += (Math.random() - 0.5) * 0.06;
    }
    w.series.push(rnd(-1, 1));
    if (w.series.length > 28) w.series.shift();
  });

  // update only the price/change/spark cells (cheap full re-render)
  wlList.innerHTML = watchPairs.map((w, i) => {
    const pos = w.chg >= 0;
    return `<div class="wl-row ${i === 0 ? 'active' : ''}" data-i="${i}">
      <div>
        <div class="wl-name">${w.sym.split('/')[0]}<span class="muted" style="font-weight:500">/${w.sym.split('/')[1]}</span></div>
        <div class="wl-sub">${w.name}</div>
      </div>
      <div class="wl-px">${fmt(w.price, w.dec)}</div>
      <div>
        <div class="wl-ch ${pos ? 'pos' : 'neg'}">${pos ? '+' : ''}${w.chg.toFixed(2)}%</div>
        ${sparkline(w.series, pos)}
      </div>
    </div>`;
  }).join('');

  /* --- keep order-form price synced to market --- */
  if (document.activeElement !== priceInput) {
    priceInput.value = lastPrice.toFixed(2);
    updateTotals();
  }
}

/* =========================================================
   BOOT
   ========================================================= */
resizeChart();
buildBook(lastPrice);
updateHeader(lastPrice);
for (let i = 0; i < 12; i++) pushTrade(lastPrice);
applyPercent(25);
setInterval(tick, 900);
</script>
</body>
</html>