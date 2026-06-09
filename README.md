<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Cowan PTV v19</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#f0f2f5;color:#1a1a2e;padding:8px;font-size:13px}
h1{font-size:14px;font-weight:700;color:#1a1a2e;margin-bottom:7px}
.row{display:flex;gap:5px;flex-wrap:wrap;align-items:flex-end;margin-bottom:5px}
label{font-size:10px;font-weight:700;letter-spacing:.05em;text-transform:uppercase;color:#666;display:block;margin-bottom:2px}
input,select{font-size:12px;background:#fff;border:1px solid #ccd;border-radius:5px;padding:5px 7px;color:#1a1a2e;outline:none}
input:focus,select:focus{border-color:#185FA5}
.btn{padding:5px 11px;font-size:12px;font-weight:600;border-radius:5px;cursor:pointer;border:none;color:#fff}
.bl{background:#185FA5}.gr{background:#1a8f60}.rd{background:#8f2020}.gy{background:#9aa;color:#fff}.or{background:#c07800}
canvas{border-radius:6px;border:1px solid #dde;display:block;box-shadow:0 2px 12px rgba(0,0,0,.10)}
#wrap{position:relative;margin-bottom:6px;overflow:hidden}
#cinfo{position:absolute;top:6px;left:6px;background:rgba(255,255,255,.95);border:1px solid #dde;border-radius:5px;padding:4px 10px;font-size:11px;color:#444;pointer-events:none;display:none;z-index:10}
#snapinfo{position:absolute;top:6px;right:6px;background:rgba(24,95,165,.9);border-radius:5px;padding:4px 10px;font-size:11px;color:#fff;pointer-events:none;display:none;z-index:10}
#zoom-bar{display:flex;gap:4px;align-items:center;margin-bottom:5px;flex-wrap:wrap}
.zb{padding:4px 10px;font-size:12px;font-weight:700;border-radius:5px;cursor:pointer;border:1px solid #ccd;background:#fff;color:#555;min-width:30px;text-align:center}
.zb:hover{background:#e8eaf0;color:#1a1a2e}
.panel{background:#fff;border:1px solid #dde;border-radius:8px;padding:8px 10px;margin-bottom:6px;box-shadow:0 1px 4px rgba(0,0,0,.05)}
.ptitle{font-size:10px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;color:#888;margin-bottom:7px;padding-bottom:4px;border-bottom:1px solid #eee;display:flex;align-items:center;gap:8px;flex-wrap:wrap}
.badge{font-size:10px;padding:2px 7px;border-radius:20px;font-weight:600}
.bg{background:#185FA5;color:#fff}.gg{background:#1a8f60;color:#fff}.ro{background:#f5a623;color:#000}.pu{background:#6b3fa0;color:#fff}
table{width:100%;border-collapse:collapse}
th{padding:5px 6px;font-size:10px;font-weight:700;color:#888;border-bottom:1px solid #eee;text-align:left;text-transform:uppercase}
td{padding:5px 6px;border-bottom:1px solid #f5f5f5;font-size:12px;vertical-align:middle;color:#1a1a2e}
tr:hover td{background:rgba(24,95,165,.04)}
.up{color:#1a8f60;font-weight:700}.dn{color:#e05050;font-weight:700}.hi{color:#c07800;font-weight:700}
.sb,.tog{padding:4px 8px;font-size:11px;font-weight:600;border-radius:4px;cursor:pointer;border:1px solid #ccd;background:#fff;color:#666}
.sb.on{background:#1a8f60;color:#fff;border-color:#1a8f60}
.tog.on{background:#185FA5;color:#fff;border-color:#185FA5}
.tog.on2{background:#6b3fa0;color:#fff;border-color:#6b3fa0}
.tog.on3{background:#b07000;color:#fff;border-color:#b07000}
.tog.on4{background:#8f2020;color:#fff;border-color:#8f2020}
#stat{padding:5px 10px;border-radius:5px;font-size:12px;margin-bottom:6px;display:none}
.pt-row{display:flex;align-items:center;gap:5px;background:#f5f7fa;border-radius:5px;padding:4px 7px;margin-bottom:3px}
.dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.tabs{display:flex;gap:4px;margin-bottom:8px;flex-wrap:wrap}
.tab{padding:5px 10px;font-size:11px;font-weight:600;border-radius:5px;cursor:pointer;border:1px solid #ccd;background:#fff;color:#666}
.tab.on{background:#185FA5;color:#fff;border-color:#185FA5}
.cb{display:none}.cb.on{display:block}
.g2{display:grid;grid-template-columns:1fr 1fr;gap:6px}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:6px}
.g4{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:6px}
.res{background:#f5f7fa;border-radius:6px;padding:8px;text-align:center;border:1px solid #eee}
.res.hi2{border:2px solid #185FA5}.res.ok{border:2px solid #1a8f60}
.rlbl{font-size:10px;color:#888;margin-bottom:2px}.rval{font-size:18px;font-weight:700;margin:0;color:#1a1a2e}.rsub{font-size:10px;color:#888;margin-top:2px}
.warn{background:#fff5f5;border-left:3px solid #c04040;border-radius:0 5px 5px 0;padding:5px 10px;font-size:11px;color:#c04040;margin-top:5px;display:none}
.info{background:#f0f6ff;border-left:3px solid #185FA5;border-radius:0 5px 5px 0;padding:5px 10px;font-size:11px;color:#185FA5;margin-bottom:7px}
.scard{border-radius:10px;padding:10px 12px;position:relative;border:2px solid}
.rev-card{background:#f5f7fa;border-radius:7px;padding:9px 11px;margin-bottom:4px;border-left:3px solid #dde}
.rev-card.strong{border-left-color:#f5a623;background:#fffbf0}
.rev-card.medium{border-left-color:#185FA5;background:#f0f6ff}
.conf-badge{float:right;font-size:10px;font-weight:700;padding:2px 6px;border-radius:20px}
.ch{background:#fff3cc;color:#c07800;border:1px solid #f5a62366}
.cm{background:#e8f0fc;color:#185FA5;border:1px solid #185FA566}
/* Balanced 45 */
#bal-panel{background:#f0faf2;border:2px solid #1a8f60;border-radius:10px;padding:10px 12px;margin-bottom:6px;display:none}
#bal-panel .ptitle{border-bottom-color:#c8e8d0;color:#1a7a40}
.bal-step{background:#e8f5ec;border-radius:6px;padding:8px 12px;margin-bottom:4px;font-size:12px;line-height:1.7;border-left:2px solid #1a8f60}
.bal-res{background:#e8f5ec;border-radius:6px;padding:8px;text-align:center}
.bal-shift-btn{padding:4px 10px;font-size:11px;font-weight:700;border-radius:5px;cursor:pointer;border:1px solid #1a8f60;background:#f0faf2;color:#1a7a40;transition:all .12s}
.bal-shift-btn:hover{background:#1a8f60;color:#fff}
.bal-shift-btn.on{background:#1a8f60;color:#fff;border-color:#1a8f60}
/* Backtest */
.bt-win{color:#1a8f60;font-weight:700}.bt-loss{color:#e05050;font-weight:700}.bt-miss{color:#c07800;font-weight:700}.bt-pend{color:#185FA5;font-weight:700}
/* Square panel */
#sq-panel{background:#fff8f0;border:2px solid #c07800;border-radius:10px;padding:10px 12px;margin-bottom:6px}
#sq-panel .ptitle{border-bottom-color:#f5d080;color:#8a5500}
.sq-btn{padding:4px 10px;font-size:11px;font-weight:700;border-radius:5px;cursor:pointer;border:1px solid #c07800;background:#fff8f0;color:#8a5500;transition:all .12s}
.sq-btn:hover,.sq-btn.on{background:#c07800;color:#fff;border-color:#c07800}
</style>
</head>
<body>
<h1>📐 Cowan PTV v19 — Live Chart · Buy/Sell Signals · HH/LL Vectors · Backtest · Future Bars</h1>
<div id="stat"></div>

<div class="row">
  <div><label>Symbol</label>
     <select id="sym" onchange="document.getElementById('csw').style.display=this.value==='CUSTOM'?'block':'none'">
      <option value="^NSEI">NIFTY 50</option>
        <option value="^NSEBANK">BANKNIFTY</option>
        <option value="^BSESN">SENSEX</option>
        <option value="RELIANCE.NS">RELIANCE</option>
        <option value="HDFCBANK.NS" selected>HDFCBANK</option>
        <option value="BHARTIARTL.NS">BHARTIARTL</option>
        <option value="ICICIBANK.NS">ICICIBANK</option>
        <option value="SBIN.NS">SBIN</option>
        <option value="TCS.NS">TCS</option>
        <option value="APLAPOLLO.NS">APL APOLLO</option>
        <option value="BAJFINANCE.NS">BAJFINANCE</option>
        <option value="LT.NS">LT</option>
        <option value="HINDUNILVR.NS">HINDUNILVR</option>
        <option value="INFY.NS">INFY</option>
        <option value="SUNPHARMA.NS">SUNPHARMA</option>
        <option value="ITC.NS">ITC</option>
        <option value="KOTAKBANK.NS">KOTAKBANK</option>
        <option value="AXISBANK.NS">AXISBANK</option>
        <option value="MARUTI.NS">MARUTI</option>
        <option value="TITAN.NS">TITAN</option>
        <option value="ULTRACEMCO.NS">ULTRACEMCO</option>
        <option value="NTPC.NS">NTPC</option>
        <option value="ONGC.NS">ONGC</option>
        <option value="POWERGRID.NS">POWERGRID</option>
        <option value="TATASTEEL.NS">TATASTEEL</option>
        <option value="M&M.NS">M&M</option>
        <option value="BAJAJFINSV.NS">BAJAJFINSV</option>
        <option value="WIPRO.NS">WIPRO</option>
        <option value="HCLTECH.NS">HCLTECH</option>
        <option value="TECHM.NS">TECHM</option>
        <option value="ADANIENT.NS">ADANIENT</option>
        <option value="ADANIPORTS.NS">ADANIPORTS</option>
        <option value="JSWSTEEL.NS">JSWSTEEL</option>
        <option value="HINDALCO.NS">HINDALCO</option>
        <option value="COALINDIA.NS">COALINDIA</option>
        <option value="BPCL.NS">BPCL</option>
        <option value="INDUSINDBK.NS">INDUSINDBK</option>
        <option value="NESTLEIND.NS">NESTLEIND</option>
        <option value="ASIANPAINT.NS">ASIANPAINT</option>
        <option value="GRASIM.NS">GRASIM</option>
        <option value="EICHERMOT.NS">EICHERMOT</option>
        <option value="HEROMOTOCO.NS">HEROMOTOCO</option>
        <option value="CIPLA.NS">CIPLA</option>
        <option value="DRREDDY.NS">DRREDDY</option>
        <option value="APOLLOHOSP.NS">APOLLOHOSP</option>
        <option value="DIVISLAB.NS">DIVISLAB</option>
        <option value="BRITANNIA.NS">BRITANNIA</option>
        <option value="TATAMOTORS.NS">TATAMOTORS</option>
        <option value="BAJAJ-AUTO.NS">BAJAJ-AUTO</option>
        <option value="SHREECEM.NS">SHREECEM</option>
        <option value="SBILIFE.NS">SBILIFE</option>
        <option value="HDFCLIFE.NS">HDFCLIFE</option>
        <option value="ICICIPRULI.NS">ICICIPRULI</option>
        <option value="PIDILITIND.NS">PIDILITIND</option>
        <option value="DABUR.NS">DABUR</option>
        <option value="GODREJCP.NS">GODREJCP</option>
        <option value="COLPAL.NS">COLPAL</option>
        <option value="AMBUJACEM.NS">AMBUJACEM</option>
        <option value="ACC.NS">ACC</option>
        <option value="SIEMENS.NS">SIEMENS</option>
        <option value="ABB.NS">ABB</option>
        <option value="BHEL.NS">BHEL</option>
        <option value="CGPOWER.NS">CGPOWER</option>
        <option value="BEL.NS">BEL</option>
        <option value="HAL.NS">HAL</option>
        <option value="BDL.NS">BDL</option>
        <option value="IRCTC.NS">IRCTC</option>
        <option value="CONCOR.NS">CONCOR</option>
        <option value="INDIGO.NS">INDIGO</option>
        <option value="DLF.NS">DLF</option>
        <option value="GODREJPROP.NS">GODREJPROP</option>
        <option value="OBEROIRLTY.NS">OBEROIRLTY</option>
        <option value="CHOLAFIN.NS">CHOLAFIN</option>
        <option value="MUTHOOTFIN.NS">MUTHOOTFIN</option>
        <option value="LICHSGFIN.NS">LICHSGFIN</option>
        <option value="PFC.NS">PFC</option>
        <option value="RECLTD.NS">RECLTD</option>
        <option value="NHPC.NS">NHPC</option>
        <option value="RVNL.NS">RVNL</option>
        <option value="IRFC.NS">IRFC</option>
        <option value="CANBK.NS">CANBK</option>
        <option value="BANKBARODA.NS">BANKBARODA</option>
        <option value="PNB.NS">PNB</option>
        <option value="FEDERALBNK.NS">FEDERALBNK</option>
        <option value="AUBANK.NS">AUBANK</option>
        <option value="IDFCFIRSTB.NS">IDFCFIRSTB</option>
        <option value="UNIONBANK.NS">UNIONBANK</option>
        <option value="LTIM.NS">LTIM</option>
        <option value="PERSISTENT.NS">PERSISTENT</option>
        <option value="MPHASIS.NS">MPHASIS</option>
        <option value="COFORGE.NS">COFORGE</option>
        <option value="LUPIN.NS">LUPIN</option>
        <option value="AUROPHARMA.NS">AUROPHARMA</option>
        <option value="TORNTPHARM.NS">TORNTPHARM</option>
        <option value="ALKEM.NS">ALKEM</option>
        <option value="ZYDUSLIFE.NS">ZYDUSLIFE</option>
        <option value="GLENMARK.NS">GLENMARK</option>
        <option value="BIOCON.NS">BIOCON</option>
        <option value="NMDC.NS">NMDC</option>
        <option value="SAIL.NS">SAIL</option>
        <option value="JINDALSTEL.NS">JINDALSTEL</option>
        <option value="GAIL.NS">GAIL</option>
        <option value="IOC.NS">IOC</option>
    </select>
  </div>
  <div id="csw" style="display:none"><label>Custom</label><input id="csym" placeholder="e.g. TATAMOTORS.NS" style="width:130px"></div>
  <div><label>Interval</label>
    <select id="intv">
      <option value="1h">1Hour</option>
      <option value="2h">2Hour</option>
      <option value="3h">3Hour</option>
      <option value="4h">4Hour</option>
      <option value="1d">Daily</option>
      <option value="1wk">Weekly</option>
      <option value="1mo">Monthly</option>
    </select>
  </div>
  <div><label>Range</label>
    <select id="rng">
      <option value="3mo">3M</option>
      <option value="6mo" selected>6M</option>
      <option value="1y">1Y</option>
      <option value="2y">2Y</option>
      <option value="5y">5Y</option>
      <option value="10y">10Y</option>
      <option value="20y">20y</option>
      <option value="max">Max</option>
    </select>
  </div>
  <div><label>Scale</label>
    <select id="scl">
      <option value="auto">Auto</option>
      <option value="arith">Arithmetic</option>
      <option value="log">Log %</option>
    </select>
  </div>
  <div><label>Hrs/day</label><input id="hpd" type="number" value="6.5" step="0.5" style="width:55px" oninput="onSettingChange()"></div>
  <div><label>Swing %</label><input id="swpct" type="number" value="3" step="0.5" min="0.5" style="width:55px" oninput="onSettingChange()"></div>
  <div><label>Swings from date</label><input id="swing-from" type="date" style="width:130px" oninput="onSettingChange()" title="Only detect swings from this date onward — leave blank for all"></div>
  <div><label>Future bars</label><input id="futbars" type="number" value="60" min="10" max="300" style="width:60px" oninput="draw()"></div>
  <button class="btn bl" onclick="loadData()">🔄 Load</button>
</div>
<!-- CHART RATIO LINES TOGGLE -->
<div class="row" style="margin-bottom:4px">
  <span style="font-size:11px;color:#444">Confluence lines on chart:</span>
  <button class="tog" id="tog-conf-chart" onclick="toggleConfChart()" style="border-color:#f5a623;color:#f5a623">📊 Ratio Lines OFF</button>
  <span style="font-size:11px;color:#333;margin-left:4px">— shows all PTV×ratio projection lines directly on chart, colour coded by swing</span>
</div>

<div id="zoom-bar">
  <span style="font-size:11px;color:#444">Zoom:</span>
  <button class="zb" onclick="zoomIn()">+</button><button class="zb" onclick="zoomOut()">-</button><button class="zb" onclick="zoomReset()">⟳</button>
  <span id="zlbl" style="font-size:11px;color:#555;min-width:80px">all</span>
  <span style="color:#222;margin:0 4px">|</span>
  <span style="font-size:11px;color:#444">Pan:</span>
  <button class="zb" onclick="panLeft()">◀</button><button class="zb" onclick="panRight()">▶</button>
  <button class="zb" onclick="panStart()">⏮</button><button class="zb" onclick="panEnd()">⏭</button>
  <button class="zb" onclick="panFuture()" style="color:#f5a623;border-color:#f5a62366">→Future</button>
  <span style="color:#222;margin:0 4px">|</span>
  <span style="font-size:11px;color:#444">Snap:</span>
  <button class="sb on" id="sb-c" onclick="setSnap('close')">Close</button>
  <button class="sb" id="sb-h" onclick="setSnap('high')">High</button>
  <button class="sb" id="sb-l" onclick="setSnap('low')">Low</button>
  <button class="sb" id="sb-e" onclick="setSnap('exact')">Exact Y</button>
  <span style="color:#222;margin:0 4px">|</span>
  <button class="tog on"  id="tog-auto" onclick="toggleAuto()">🤖 Auto ON</button>
  <button class="tog on2" id="tog-gann" onclick="toggleGann()">📐 Gann ON</button>
  <button class="tog on3" id="tog-proj" onclick="toggleProj()">🎯 Proj ON</button>
  <button class="tog" id="tog-bal" onclick="toggleBal()" style="border-color:#1a5c20;color:#3a9f50">⚖️ Balanced 45° OFF</button>
  <button class="tog" id="tog-ruler" onclick="toggleRuler()" style="border-color:#a066e0;color:#a066e0">📏 Ruler OFF</button>
  <button class="tog" id="tog-proj-move" onclick="toggleProjMove()" style="border-color:#50c8f5;color:#50c8f5">🔀 Move PTV OFF</button>
  <button class="tog" id="tog-fan" onclick="toggleFan()" style="border-color:#e08040;color:#e08040">🌟 PTV Fan OFF</button>
  <button class="tog" id="tog-ellipse" onclick="toggleEllipse()" style="border-color:#a066e0;color:#a066e0">🥚 Ellipse OFF</button>
  <button class="btn rd" style="padding:3px 8px;font-size:11px" onclick="clearManual()">🗑 Clear</button>
  <span style="font-size:11px;color:#2a2d3e">Drag=pan · Scroll=zoom · Right-click=remove last</span>
</div>

<div id="fan-controls" style="display:none;background:#fff8f3;border:1px solid #e0804066;border-radius:7px;padding:7px 10px;margin-bottom:5px">
  <span style="font-size:11px;font-weight:700;color:#e08040;margin-right:8px">🌟 PTV Fan — click A then B on chart:</span>
  <span style="font-size:11px;color:#555;margin-right:12px" id="fan-status">Click point A (anchor)</span>
  <span style="font-size:11px;color:#444;margin-right:6px">Show ratios:</span>
  <!-- Ratio checkboxes -->
 <div style="display:flex;flex-wrap:wrap;align-items:center;gap:6px;">

  <label style="font-size:11px;color:#888;cursor:pointer">
    <input type="checkbox" id="fan-r-0382" checked onchange="draw()"> 0.382
  </label>

  <label style="font-size:11px;color:#888;cursor:pointer">
    <input type="checkbox" id="fan-r-05" checked onchange="draw()"> 0.5
  </label>

  <label style="font-size:11px;color:#888;cursor:pointer">
    <input type="checkbox" id="fan-r-0618" checked onchange="draw()"> 0.618
  </label>

  <label style="font-size:11px;color:#f5a623;cursor:pointer">
    <input type="checkbox" id="fan-r-1" checked onchange="draw()"> 1×
  </label>

  <label style="font-size:11px;color:#f5a623;cursor:pointer">
    <input type="checkbox" id="fan-r-1272" onchange="draw()"> 1.272
  </label>

  <label style="font-size:11px;color:#f5a623;cursor:pointer">
    <input type="checkbox" id="fan-r-1414" checked onchange="draw()"> sqrt2
  </label>

  <label style="font-size:11px;color:#f5a623;cursor:pointer">
    <input type="checkbox" id="fan-r-1618" checked onchange="draw()"> PHI
  </label>

  <label style="font-size:11px;color:#f5a623;cursor:pointer">
    <input type="checkbox" id="fan-r-1732" checked onchange="draw()"> sqrt3
  </label>

  <label style="font-size:11px;color:#ccd;cursor:pointer">
    <input type="checkbox" id="fan-r-2" checked onchange="draw()"> 2×
  </label>

  <label style="font-size:11px;color:#ccd;cursor:pointer">
    <input type="checkbox" id="fan-r-2236" onchange="draw()"> sqrt5
  </label>

  <label style="font-size:11px;color:#ccd;cursor:pointer">
    <input type="checkbox" id="fan-r-2618" onchange="draw()"> PHI2
  </label>

  <label style="font-size:11px;color:#ccd;cursor:pointer">
    <input type="checkbox" id="fan-r-3" onchange="draw()"> 3×
  </label>

  <label style="font-size:11px;color:#ccd;cursor:pointer">
    <input type="checkbox" id="fan-r-pi" onchange="draw()"> π
  </label>

  <button onclick="clearFan()"
    style="font-size:10px;padding:2px 7px;border-radius:4px;background:#2a1515;border:1px solid #c0404066;color:#c04040;cursor:pointer">
    Clear
  </button>

</div>
</div>

<!-- ELLIPSE CONTROLS -->
<div id="ellipse-controls" style="display:none;background:#faf5ff;border:1px solid #a066e066;border-radius:7px;padding:7px 10px;margin-bottom:5px">
  <span style="font-size:11px;font-weight:700;color:#a066e0;margin-right:8px">🥚 Cowan Ellipse — click A (swing start) then B (swing end):</span>
  <span style="font-size:11px;color:#555;margin-right:12px" id="ell-status">Click point A (swing start)</span>

  <span style="font-size:11px;font-weight:700;color:#8a5500;margin-right:4px">Angle:</span>
  <button id="ell-ang-60"  onclick="setEllAngle(60)"  style="font-size:11px;padding:2px 8px;margin-right:2px;border-radius:4px;cursor:pointer;border:2px solid #a066e0;background:#a066e0;color:#fff;font-weight:700">🔺60°</button>
  <button id="ell-ang-72"  onclick="setEllAngle(72)"  style="font-size:11px;padding:2px 8px;margin-right:2px;border-radius:4px;cursor:pointer;border:2px solid #ccc;background:#fff;color:#666;font-weight:700">⭐72°</button>
  <button id="ell-ang-45"  onclick="setEllAngle(45)"  style="font-size:11px;padding:2px 8px;margin-right:2px;border-radius:4px;cursor:pointer;border:2px solid #ccc;background:#fff;color:#666;font-weight:700">◼45°</button>
  <button id="ell-ang-90"  onclick="setEllAngle(90)"  style="font-size:11px;padding:2px 8px;margin-right:2px;border-radius:4px;cursor:pointer;border:2px solid #ccc;background:#fff;color:#666;font-weight:700">⊞90°</button>
  <button id="ell-ang-all" onclick="setEllAngle('all')" style="font-size:11px;padding:2px 8px;margin-right:8px;border-radius:4px;cursor:pointer;border:2px solid #ccc;background:#fff;color:#666;font-weight:700">🔮All</button>
  <span id="ell-ang-explain" style="font-size:10px;color:#888;margin-right:10px">360°÷6=60° triangle (Cowan default)</span>

  <label style="font-size:11px;color:#888;margin-right:8px">Minor ratio:
    <select id="ell-minor-ratio" onchange="draw()" style="font-size:11px;padding:2px 5px;margin-left:4px">
      <option value="0.5">0.5×</option>
      <option value="0.618" selected>0.618 (PHI)</option>
      <option value="0.707">0.707</option>
      <option value="0.75">0.75</option>
      <option value="1">1× (circle)</option>
    </select>
  </label>
  <label style="font-size:11px;color:#888;margin-right:8px">Direction:
    <select id="ell-mated-rot" onchange="draw()" style="font-size:11px;padding:2px 5px;margin-left:4px">
      <option value="-1" selected>Clockwise (uptrend)</option>
      <option value="1">Counter-clockwise (downtrend)</option>
      <option value="both">Both directions</option>
    </select>
  </label>
  <label style="font-size:11px;color:#888;margin-right:8px"><input type="checkbox" id="ell-show-mated" checked onchange="draw()"> Mated ellipse</label>
  <label style="font-size:11px;color:#888;margin-right:8px"><input type="checkbox" id="ell-show-triangle" checked onchange="draw()"> Triangle</label>
  <label style="font-size:11px;color:#888;margin-right:8px"><input type="checkbox" id="ell-show-terminus" checked onchange="draw()"> Terminus T</label>
  <label style="font-size:11px;color:#888;margin-right:8px"><input type="checkbox" id="ell-show-minor-pts" checked onchange="draw()"> Minor axis pts</label>
  <label style="font-size:11px;color:#888;margin-right:8px"><input type="checkbox" id="ell-show-cross" checked onchange="draw()"> ✚ S/R Cross lines</label>
  <label style="font-size:11px;color:#888;margin-right:8px"><input type="checkbox" id="ell-show-chain" onchange="draw()"> 🔗 Chain</label>
  <span style="color:#ccc;margin:0 4px">|</span>
  <button onclick="copyEllipse()" id="ell-copy-btn" style="display:none;font-size:11px;padding:3px 10px;border-radius:4px;background:#1a8f60;border:1px solid #1a8f60;color:#fff;cursor:pointer;font-weight:700">📋 Copy Ellipse</button>
  <button onclick="pasteEllipse()" id="ell-paste-btn" style="display:none;font-size:11px;padding:3px 10px;border-radius:4px;background:#185FA5;border:1px solid #185FA5;color:#fff;cursor:pointer;font-weight:700">📌 Paste — click new A point</button>
  <span id="ell-paste-status" style="font-size:11px;color:#185FA5;margin-left:6px;font-weight:700;display:none"></span>
  <span style="color:#ccc;margin:0 4px">|</span>
  <button onclick="clearEllipse()" style="font-size:10px;padding:2px 7px;border-radius:4px;background:#faf5ff;border:1px solid #a066e066;color:#7040c0;cursor:pointer">Clear</button>
  <button onclick="continueEllipseChain()" id="ell-continue-btn" style="display:none;font-size:10px;padding:3px 10px;border-radius:4px;background:#185FA5;border:1px solid #185FA5;color:#fff;cursor:pointer;margin-left:4px;font-weight:700">▶ Continue Chain →</button>
  <button onclick="lockSFFromEllipse()" id="ell-lock-sf-btn" style="display:none;font-size:10px;padding:3px 10px;border-radius:4px;background:#c07800;border:1px solid #c07800;color:#fff;cursor:pointer;margin-left:4px;font-weight:700">🔒 Lock SF</button>
  <span id="ell-lock-sf-status" style="font-size:11px;color:#1a8f60;margin-left:8px;font-weight:700;display:none"></span>
</div>


<!-- ═══ SQUARE CHART PANEL ═══ -->
<div id="sq-panel">
  <p class="ptitle">📐 Square the Chart — Time × Price Scaling Factors
    <span class="badge ro" id="sq-active-badge" style="display:none">ACTIVE</span>
    <span style="font-size:10px;color:#888;font-weight:400;text-transform:none;letter-spacing:0">Like Timing Solution PTV tool — 1 time unit = 1 price unit for correct Gann/Cowan geometry</span>
  </p>
  <div style="display:flex;gap:12px;align-items:flex-end;flex-wrap:wrap;margin-bottom:8px">
    <div>
      <label style="color:#8a5500">Time Scale (1 bar = X price pts)</label>
      <input id="sq-time" type="number" value="1" min="0.0001" step="any" style="width:100px;border-color:#c07800;font-weight:700;font-size:14px" oninput="updateSquare()">
    </div>
    <div>
      <label style="color:#8a5500">Price Scale (1 price pt = X time units)</label>
      <input id="sq-price" type="number" value="1" min="0.0001" step="any" style="width:100px;border-color:#c07800;font-weight:700;font-size:14px" oninput="updateSquare()">
    </div>
    <div>
      <label style="color:#8a5500">Snap</label>
      <div style="display:flex;gap:4px">
        <label style="font-size:11px;color:#666;cursor:pointer;display:flex;align-items:center;gap:3px"><input type="checkbox" id="sq-snap-bar" checked onchange="updateSquare()"> Bar</label>
        <label style="font-size:11px;color:#666;cursor:pointer;display:flex;align-items:center;gap:3px"><input type="checkbox" id="sq-snap-hl" checked onchange="updateSquare()"> High/Low</label>
      </div>
    </div>
    <div>
      <label style="color:#8a5500">Time Mode</label>
      <div style="display:flex;gap:4px">
        <button class="sq-btn on" id="sq-tb" onclick="setSqMode('bars')">Trading Bars</button>
        <button class="sq-btn" id="sq-cd" onclick="setSqMode('calendar')">Calendar Days</button>
      </div>
    </div>
    <button class="sq-btn" onclick="autoSquare()" style="background:#fff8f0">⚡ Auto-Square</button>
    <button class="sq-btn" onclick="resetSquare()" style="background:#fff8f0">↺ Reset 1:1</button>
  </div>
  <div style="display:flex;gap:8px;flex-wrap:wrap;align-items:center;margin-top:6px">
    <div style="background:#fff3e0;border:1px solid #f5d080;border-radius:6px;padding:6px 12px;font-size:11px">
      <span style="color:#8a5500;font-weight:700">Effective SF: </span>
      <span id="sq-eff-sf" style="color:#c07800;font-weight:700;font-size:14px">1.000</span>
      <span style="color:#aaa;margin-left:6px" id="sq-eff-explain">= Time÷Price = 1.000</span>
    </div>
    <div id="sq-angle-box" style="background:#fff3e0;border:1px solid #f5d080;border-radius:6px;padding:6px 12px;font-size:11px">
      <span style="color:#8a5500;font-weight:700">Chart PTV angle: </span>
      <span id="sq-angle" style="color:#c07800;font-weight:700;font-size:14px">45.0°</span>
      <span style="color:#aaa;margin-left:4px">(45° = perfectly squared)</span>
    </div>
    <div id="sq-visual-box" style="background:#fff8ee;border:2px solid #c07800;border-radius:6px;padding:6px 12px;font-size:11px;display:none">
      <span style="color:#8a5500;font-weight:700">✅ Visual squaring ON</span>
      <span style="color:#888;margin-left:6px">Y-axis stretched so 1 bar = 1 pt on screen</span>
    </div>
    <div style="background:#e8f5ff;border:1px solid #90c8f0;border-radius:6px;padding:6px 12px;font-size:11px;display:none" id="sq-last-swing-box">
      <span style="color:#185FA5;font-weight:700">Last swing: </span>
      <span id="sq-last-swing" style="color:#185FA5">—</span>
    </div>
  </div>
</div>

<div id="wrap"><canvas id="c"></canvas><div id="cinfo"></div><div id="snapinfo"></div></div>

<!-- RATIO HIT ALERT BAR — always visible below chart -->
<div id="ratio-hit-bar" style="display:none;margin-bottom:6px;border-radius:8px;padding:10px 14px;border:2px solid #f5a623;background:rgba(245,166,35,.12);box-shadow:0 0 18px rgba(245,166,35,.25)">
  <div style="display:flex;align-items:center;gap:10px;flex-wrap:wrap">
    <span id="rh-badge" style="font-size:13px;font-weight:700;padding:4px 12px;border-radius:20px;background:#f5a623;color:#000">⭐ RATIO HIT</span>
    <span id="rh-main" style="font-size:15px;font-weight:700;color:#f5a623">—</span>
    <span id="rh-detail" style="font-size:12px;color:#888">—</span>
    <span id="rh-signal" style="font-size:13px;font-weight:700;margin-left:auto"></span>
  </div>
</div>

<!-- ═══ PTV TRADE SETUP ═══ -->
<div class="panel" id="trade-panel">
  <p class="ptitle">
    📊 PTV Rhythm Sequence — <span style="color:#f5a623">TRADE THIS</span>
    <span class="badge ro" id="trade-alert-badge" style="display:none">🔴 IN ZONE</span>
    <span class="badge gg" id="trade-dev-badge" style="display:none">📡 Developing</span>
    <span style="margin-left:auto;display:flex;gap:5px;align-items:center">
      <label style="font-size:10px;color:#444">Tolerance</label>
      <select id="trade-tol" onchange="updateTradePanel()" style="font-size:11px;padding:2px 5px">
        <option value="3">±3%</option>
        <option value="5" selected>±5%</option>
        <option value="8">±8%</option>
        <option value="10">±10%</option>
      </select>
    </span>
  </p>

  <!-- RATIO SEQUENCE TABLE — the main thing -->
  <div style="overflow-x:auto;margin-bottom:10px">
  <table id="seq-table">
    <thead>
      <tr style="border-bottom:2px solid #252840">
        <th>Swing</th>
        <th>PTV</th>
        <th style="color:#f5a623">Ratio to prev</th>
        <th style="color:#f5a623">≈ Cowan ratio</th>
        <th style="color:#1a8f60">Predicted NEXT PTV</th>
        <th>Days</th>
        <th>Type</th>
      </tr>
    </thead>
    <tbody id="seq-body"></tbody>
    <!-- DEVELOPING ROW — shown at bottom -->
    <tfoot id="seq-foot"></tfoot>
  </table>
  </div>

  <!-- PTV RATIO MATRIX — every swing ÷ every other swing -->
  <details style="margin-bottom:8px">
    <summary style="font-size:11px;font-weight:700;color:#f5a623;cursor:pointer;padding:5px 0;user-select:none">
      ▶ PTV Ratio Matrix — every swing ÷ every other (10646÷6625=PHI · 6625÷3746=√3 · dev÷any=?)
    </summary>
    <div style="overflow-x:auto;margin-top:6px;max-height:280px;overflow-y:auto">
    <table>
      <thead><tr>
        <th>Swing A</th><th>PTV A</th>
        <th>Swing B</th><th>PTV B</th>
        <th>Direction</th>
        <th style="color:#f5a623">A÷B ratio</th>
        <th style="color:#f5a623">≈ Cowan</th>
        <th>Error%</th>
        <th style="color:#1a8f60">Price levels (↑ ↓)</th>
        <th style="color:#3a9f50">Dev÷A</th>
        <th style="color:#3a9f50">Dev÷A ≈</th>
      </tr></thead>
      <tbody id="matrix-body"></tbody>
    </table>
    </div>
  </details>

  <!-- NEXT PTV PREDICTION BOX -->
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-bottom:8px" id="pred-boxes">
    <div style="background:#f0f6ff;border:2px solid #185FA5;border-radius:8px;padding:10px 12px">
      <div style="font-size:10px;font-weight:700;color:#185FA5;text-transform:uppercase;margin-bottom:4px">📡 Developing PTV now</div>
      <div style="font-size:28px;font-weight:700;color:#f5a623" id="trade-dev-ptv">—</div>
      <div style="font-size:11px;color:#555;margin-top:2px" id="trade-dev-sub">from last swing point to today</div>
      <div style="margin-top:8px">
        <div style="font-size:10px;color:#444;margin-bottom:3px" id="trade-dev-target-label">→ nearest target: —</div>
        <div style="background:#1a1f35;border-radius:4px;height:8px;overflow:hidden">
          <div id="trade-dev-bar" style="height:8px;background:#f5a623;border-radius:4px;width:0%;transition:width .3s"></div>
        </div>
        <div style="font-size:11px;color:#f5a623;margin-top:3px;font-weight:700" id="trade-dev-pct">0%</div>
      </div>
    </div>
    <div style="background:#0a1a0a;border:2px solid #1a5c20;border-radius:8px;padding:10px 12px" id="trade-signal-box">
      <div style="font-size:10px;font-weight:700;color:#3a9f50;text-transform:uppercase;margin-bottom:4px" id="trade-sig-label">Predicted next PTVs</div>
      <div id="trade-predictions" style="font-size:12px;line-height:1.9"></div>
    </div>
  </div>

  <!-- TRADE SIGNAL when in zone -->
  <div id="trade-action-box" style="display:none;border-radius:10px;padding:12px 14px;border:2px solid #1a8f60;background:rgba(26,143,96,.08)">
    <div style="font-size:15px;font-weight:700;margin-bottom:8px" id="trade-sig-title">—</div>
    <div style="display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:8px;font-size:12px">
      <div><div style="font-size:10px;color:#555;margin-bottom:2px">Entry</div><div style="font-weight:700;font-size:14px" id="trade-entry">—</div></div>
      <div><div style="font-size:10px;color:#555;margin-bottom:2px">Stop</div><div style="font-weight:700;font-size:14px;color:#e05050" id="trade-stop">—</div></div>
      <div><div style="font-size:10px;color:#555;margin-bottom:2px">Target 1</div><div style="font-weight:700;font-size:14px;color:#1a8f60" id="trade-t1">—</div></div>
      <div><div style="font-size:10px;color:#555;margin-bottom:2px">Target 2</div><div style="font-weight:700;font-size:14px;color:#1a8f60" id="trade-t2">—</div></div>
    </div>
    <div style="font-size:11px;color:#555;margin-top:6px" id="trade-sig-reason">—</div>
  </div>

  <!-- PRICE MATCH PREDICTION — fires on exact ratio hits -->
  <div id="price-pred-box" style="display:none;margin-top:6px">
    <div style="font-size:10px;font-weight:700;color:#f5a623;text-transform:uppercase;letter-spacing:.05em;margin-bottom:6px;padding:4px 0;border-bottom:1px solid rgba(245,166,35,.2)">
      🎯 Price Match Predictions — based on exact PTV ratio hits
    </div>
    <div id="price-pred-cards" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:5px"></div>
    <div style="font-size:10px;color:#444;margin-top:5px;line-height:1.5" id="price-pred-logic">—</div>
  </div>
</div>


<!-- ═══ PTV MULTI-CONFLUENCE — The Real 4D Signal ═══ -->
<div class="panel" id="conf4d-panel">
  <p class="ptitle">
    🌐 PTV Multi-Confluence — <span style="color:#f5a623">4D Signal</span>
    <span class="badge ro" id="c4-count">0 zones</span>
    <span class="badge bg" id="c4-near-badge" style="display:none">⚡ NEAR CMP</span>
    <span style="margin-left:auto;display:flex;gap:5px;align-items:center;flex-wrap:wrap">
      <label style="font-size:10px;color:#444">Date tol</label>
      <select id="c4-dtol" onchange="updateConf4D()" style="font-size:11px;padding:2px 5px">
        <option value="3">±3d</option><option value="5" selected>±5d</option>
        <option value="10">±10d</option><option value="15">±15d</option>
      </select>
      <label style="font-size:10px;color:#444">Price tol</label>
      <select id="c4-ptol" onchange="updateConf4D()" style="font-size:11px;padding:2px 5px">
        <option value="0.5">±0.5%</option><option value="1" selected>±1%</option>
        <option value="2">±2%</option><option value="3">±3%</option>
      </select>
      <label style="font-size:10px;color:#444">Min lines</label>
      <select id="c4-minlines" onchange="updateConf4D()" style="font-size:11px;padding:2px 5px">
        <option value="2">2+</option><option value="3" selected>3+</option>
        <option value="4">4+</option><option value="5">5+</option>
      </select>
    </span>
  </p>

  <div style="background:#0a1020;border-left:3px solid #185FA5;border-radius:0 6px 6px 0;padding:7px 10px;font-size:11px;color:#6a9ddb;margin-bottom:8px;line-height:1.6">
    <b style="color:#fff">How it works:</b> Every swing PTV × every Cowan ratio = a future projection line.
    When lines from <b>multiple different swings</b> intersect at the same date AND price zone → that is the signal.
    Exactly like your chart: 3059×0.5 + 996×√3 + 304×? all pointing to the same zone = <b style="color:#f5a623">high confidence reversal</b>.
    More lines converging = stronger signal.
  </div>

  <!-- TOP CONFLUENCE ZONES — cards -->
  <div id="c4-cards" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:6px;margin-bottom:8px"></div>

  <!-- ALL PROJECTIONS TABLE — collapsible -->
  <details>
    <summary style="font-size:11px;color:#444;cursor:pointer;padding:4px 0;user-select:none">
      ▶ Show all projection lines (<span id="c4-total">0</span> lines from all swings × all ratios)
    </summary>
    <div style="overflow-x:auto;margin-top:6px;max-height:300px;overflow-y:auto">
    <table><thead><tr>
      <th>From swing</th><th>Ratio</th><th>Proj PTV</th>
      <th>Days from now</th><th>Est date</th><th>↑ Price</th><th>↓ Price</th><th>Confluence</th>
    </tr></thead>
    <tbody id="c4-body"></tbody></table>
    </div>
  </details>
</div>

<!-- ═══ DASHBOARD ROW — signals + HH/LL side by side ═══ -->
<div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-bottom:6px" id="dash-row">

  <!-- BUY/SELL SIGNALS -->
  <div class="panel" style="margin-bottom:0">
    <p class="ptitle">🎯 Buy / Sell Signals
      <span class="badge ro" id="sig-count">0</span>
      <span class="badge bg" id="sig-near-badge" style="display:none">⚡ NEAR</span>
    </p>
    <div id="sig-cards" style="display:flex;flex-direction:column;gap:5px"></div>
  </div>

  <!-- HH/LL VECTORS — compact -->
  <div class="panel" style="margin-bottom:0">
    <p class="ptitle">📐 HH · LL · HL · LH Vectors</p>
    <div style="overflow-x:auto;max-height:320px;overflow-y:auto">
    <table><thead><tr>
      <th>Type</th><th>Swing</th><th>PTV</th><th>Angle</th><th>Ratio↑</th>
    </tr></thead>
    <tbody id="hhll-body"></tbody></table>
    </div>
  </div>
</div>

<!-- ═══ BACKTEST ═══ -->
<div class="panel">
  <p class="ptitle">🔬 Virtual Money Backtest — Ratio Hit Strategy
    <span class="badge pu" id="bt-count">not run</span>
    <button class="btn or" style="padding:3px 9px;font-size:11px;margin-left:auto" onclick="runBacktest()">▶ Run Backtest</button>
  </p>
  <div style="background:#f0f6ff;border-left:3px solid #185FA5;border-radius:0 6px 6px 0;padding:7px 10px;font-size:11px;color:#6a9ddb;margin-bottom:8px;line-height:1.6">
    <b style="color:#fff">Strategy:</b> Walk through every candle. When developing PTV ÷ any completed swing = Cowan ratio (within tolerance) → enter trade.
    Exit at Target % or Stop Loss %. Track virtual P&amp;L with equity curve.
  </div>
  <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(145px,1fr));gap:6px;margin-bottom:8px">
    <div><label>Ratio tolerance</label>
      <select id="bt-rtol" style="width:100%">
        <option value="2">±2% tight</option><option value="3" selected>±3%</option>
        <option value="5">±5% loose</option><option value="8">±8% wide</option>
      </select>
    </div>
    <div><label>Stop loss %</label>
      <select id="bt-sl" style="width:100%">
        <option value="0.5">0.5%</option><option value="1" selected>1%</option>
        <option value="1.5">1.5%</option><option value="2">2%</option>
      </select>
    </div>
    <div><label>Target %</label>
      <select id="bt-tp" style="width:100%">
        <option value="2">2%</option><option value="3" selected>3%</option>
        <option value="4">4%</option><option value="5">5%</option><option value="7">7%</option>
      </select>
    </div>
    <div><label>Capital / trade</label>
      <select id="bt-cap" style="width:100%">
        <option value="10000">10,000</option><option value="50000">50,000</option>
        <option value="100000" selected>1,00,000</option><option value="500000">5,00,000</option>
      </select>
    </div>
    <div><label>Direction</label>
      <select id="bt-dir" style="width:100%">
        <option value="both" selected>Both BUY+SELL</option>
        <option value="buy">BUY only</option><option value="sell">SELL only</option>
      </select>
    </div>
    <div><label>Ratios to use</label>
      <select id="bt-minq" style="width:100%">
        <option value="key" selected>Key only (PHI,√2,π...)</option>
        <option value="all">All Cowan ratios</option>
      </select>
    </div>
  </div>
  <div id="bt-stats" style="display:none;margin-bottom:8px">
    <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(115px,1fr));gap:5px;margin-bottom:8px">
      <div class="res ok"><p class="rlbl">Win Rate</p><p class="rval" id="bt-wr" style="color:#1a8f60">—</p><p class="rsub" id="bt-wr-sub">—</p></div>
      <div class="res"><p class="rlbl">Wins</p><p class="rval bt-win" id="bt-wins">—</p></div>
      <div class="res"><p class="rlbl">Losses</p><p class="rval bt-loss" id="bt-losses">—</p></div>
      <div class="res"><p class="rlbl">Trades</p><p class="rval" id="bt-total">—</p><p class="rsub" id="bt-pend-lbl">—</p></div>
      <div class="res hi2"><p class="rlbl">Net P&amp;L ₹</p><p class="rval" id="bt-pnl">—</p></div>
      <div class="res"><p class="rlbl">Best ratio</p><p class="rval" id="bt-best" style="color:#f5a623;font-size:12px">—</p></div>
      <div class="res"><p class="rlbl">Expectancy</p><p class="rval" id="bt-rr" style="color:#185FA5">—</p></div>
      <div class="res"><p class="rlbl">Max drawdown</p><p class="rval" id="bt-dd" style="color:#e05050">—</p></div>
    </div>
    <canvas id="bt-equity-canvas" height="70" style="width:100%;display:block;background:#f0f2f5;border-radius:6px;margin-bottom:6px"></canvas>
    <div id="bt-analysis" style="background:#f5f7fa;border-radius:6px;padding:7px 10px;font-size:11px;color:#ccd;line-height:1.9"></div>
  </div>
  <details>
    <summary style="font-size:11px;color:#444;cursor:pointer;padding:4px 0;user-select:none">▶ Show trade log</summary>
    <div style="overflow-x:auto;max-height:400px;overflow-y:auto;margin-top:6px">
    <table><thead><tr>
      <th>#</th><th>Date</th><th>Dir</th><th>Ratio</th><th>Err%</th>
      <th>Entry ₹</th><th>SL ₹</th><th>Target ₹</th><th>Exit ₹</th>
      <th>Result</th><th>P&amp;L ₹</th><th>Running ₹</th>
    </tr></thead>
    <tbody id="bt-body"></tbody></table>
    </div>
  </details>
</div>

<!-- ═══ AUTO ANALYSIS + UPCOMING ═══ -->
<div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-bottom:6px">
  <div class="panel" style="margin-bottom:0">
    <p class="ptitle">🤖 Auto PTV Swings <span class="badge gg" id="ap-count">0</span> <span id="auto-sum" style="font-weight:400;font-size:10px;color:#444;margin-left:4px"></span></p>
    <div style="overflow-x:auto;max-height:280px;overflow-y:auto">
    <table><thead><tr><th>Swing</th><th>PTV</th><th>Angle</th><th>Ratio↑</th><th>Type</th></tr></thead>
    <tbody id="auto-table"></tbody></table>
    </div>
  </div>
  <div class="panel" style="margin-bottom:0">
    <p class="ptitle">⚡ Upcoming Reversals <span class="badge ro" id="rev-count">0</span></p>
    <div id="rev-cards" style="max-height:280px;overflow-y:auto"></div>
  </div>
</div>

<!-- ═══ ALL FORWARD LEVELS — compact ═══ -->
<div class="panel">
  <p class="ptitle">📋 Forward Levels
    <span style="font-weight:400;font-size:10px;color:#444;margin-left:4px">Strong + Near only visible — rest in collapse</span>
  </p>
  <div id="levels-tags" style="margin-bottom:7px;display:flex;flex-wrap:wrap;gap:3px"></div>
  <!-- Strong + Near only table -->
  <div style="overflow-x:auto">
  <table>
    <thead><tr><th>Ratio</th><th>PTV</th><th>Days</th><th>Est Date</th><th>↑</th><th>↓</th><th>Strength</th></tr></thead>
    <tbody id="levels-table"></tbody>
  </table>
  </div>
  <!-- All levels collapsible -->
  <details style="margin-top:6px">
    <summary style="font-size:11px;color:#444;cursor:pointer;padding:4px 0;user-select:none">▶ Show all levels (<span id="levels-all-count">0</span> total)</summary>
    <div style="overflow-x:auto;margin-top:6px;max-height:300px;overflow-y:auto">
    <table>
      <thead><tr><th>Ratio</th><th>PTV</th><th>Days</th><th>Est Date</th><th>↑ Price</th><th>↓ Price</th><th>Strength</th></tr></thead>
      <tbody id="levels-table-all"></tbody>
    </table>
    </div>
  </details>
</div>

<!-- ═══ BALANCED 45° PANEL ═══ -->
<div id="bal-panel">
  <p class="ptitle">⚖️ Balanced 45° Scale <span class="badge" style="background:#1a5c20;color:#fff">SEPARATE — does not affect original calcs</span></p>
  <div class="info" style="background:#edf7f0;border-color:#1a5c20;color:#5adf80;margin-bottom:8px">This method is from your screenshot. Completely independent from everything else above.</div>

  <!-- DECIMAL SHIFT — the key new control -->
  <div style="background:#edf7f0;border:1.5px solid #1a5c20;border-radius:8px;padding:8px 12px;margin-bottom:8px">
    <p style="font-size:11px;font-weight:700;color:#3a9f50;margin-bottom:6px">🔢 Decimal Shift — smaller PTV = more accurate ratio matching</p>
    <div style="display:flex;gap:6px;align-items:center;flex-wrap:wrap;margin-bottom:6px">
      <span style="font-size:11px;color:#555">Scale factor ÷</span>
      <button class="bal-shift-btn on" id="bsb-1" onclick="setBalShift(1)">÷1 (raw)</button>
      <button class="bal-shift-btn" id="bsb-10" onclick="setBalShift(10)">÷10</button>
      <button class="bal-shift-btn" id="bsb-100" onclick="setBalShift(100)">÷100</button>
      <button class="bal-shift-btn" id="bsb-1000" onclick="setBalShift(1000)">÷1000</button>
      <span style="font-size:11px;color:#444;margin-left:4px">or custom:</span>
      <input id="bal-custom-shift" type="number" value="1" min="1" step="1" style="width:70px;border-color:#1a5c20" oninput="setBalShiftCustom()">
    </div>
    <div style="font-size:11px;color:#555;line-height:1.6" id="bal-shift-explain">
      Raw SF=394.7 → PTV=10,607 — too big.<br>
      ÷10 → SF=39.47 → PTV=766 &nbsp; ÷100 → SF=3.947 → PTV=76.6 ✓ &nbsp; ÷1000 → SF=0.395 → PTV=20.4
    </div>
  </div>

  <div class="g4" style="margin-bottom:8px">
    <div><label style="color:#3a9f50">Price component</label><input id="bal-p" type="number" value="7500" oninput="calcBal()" style="border-color:#1a5c20"></div>
    <div><label style="color:#3a9f50">Time (days)</label><input id="bal-t" type="number" value="19" oninput="calcBal()" style="border-color:#1a5c20"></div>
    <div><label style="color:#3a9f50">Starting price</label><input id="bal-sp" type="number" value="49954" oninput="calcBal()" style="border-color:#1a5c20"></div>
    <div class="bal-res"><p class="rlbl" style="color:#3a9f50">Raw ratio (bigger÷smaller)</p><p class="rval" id="bal-ratio" style="color:#3a9f50">—</p><p class="rsub" id="bal-ratio-sub">—</p></div>
  </div>
  <div id="bal-steps" style="margin-bottom:8px"></div>
  <div class="g4" style="margin-bottom:8px">
    <div class="bal-res" style="border:1.5px solid #1a5c20;border-radius:6px"><p class="rlbl" style="color:#3a9f50">Effective SF</p><p class="rval" id="bal-eff-sf" style="color:#3a9f50">—</p><p class="rsub" id="bal-eff-sf-sub">raw ÷ shift</p></div>
    <div class="bal-res" style="border:1.5px solid #1a5c20;border-radius:6px"><p class="rlbl" style="color:#3a9f50">Balanced component</p><p class="rval" id="bal-bp" style="color:#3a9f50">—</p><p class="rsub" id="bal-bt">— days</p></div>
    <div class="bal-res" style="border:2px solid #f5a623;border-radius:6px"><p class="rlbl" style="color:#f5a623">Balanced PTV</p><p class="rval" id="bal-ptv" style="color:#f5a623">—</p><p class="rsub" id="bal-ptv-sub">= component × √2</p></div>
    <div class="bal-res" style="border:1.5px solid #1a5c20;border-radius:6px"><p class="rlbl" style="color:#3a9f50">Angle</p><p class="rval" id="bal-ang" style="color:#3a9f50">45.00°</p></div>
  </div>
  <p style="font-size:10px;font-weight:700;color:#3a9f50;margin-bottom:6px;text-transform:uppercase">All ratio projections (⭐ = from screenshot)</p>
  <div style="overflow-x:auto"><table>
    <thead><tr style="border-bottom:1px solid #1a5c20">
      <th style="color:#3a9f50">Ratio</th><th style="color:#3a9f50">PTV×ratio</th>
      <th style="color:#3a9f50">Component</th><th style="color:#3a9f50">Real days</th>
      <th style="color:#3a9f50">Price ↑</th><th style="color:#3a9f50">Price ↓</th>
    </tr></thead>
    <tbody id="bal-table"></tbody>
  </table></div>
</div>

<!-- ═══ MANUAL POINTS ═══ -->
<div class="panel">
  <p class="ptitle">📍 Manual points</p>
  <div id="pts-list"><p style="color:#333;font-size:11px">Click chart to add</p></div>
</div>

<!-- ═══ CALCULATOR ═══ -->
<div class="panel">
  <p class="ptitle">🧮 Full Calculator (original scale)</p>
  <div class="tabs">
    <button class="tab on" id="tb1" onclick="sw(1)">Date→Price</button>
    <button class="tab" id="tb2" onclick="sw(2)">Price→Date</button>
    <button class="tab" id="tb3" onclick="sw(3)">Reversal</button>
    <button class="tab" id="tb4" onclick="sw(4)">All Ratios</button>
    <button class="tab" id="tb5" onclick="sw(5)">Trig</button>
    <button class="tab" id="tb6" onclick="sw(6)">Ratio Check</button>
    <button class="tab" id="tb7" onclick="sw(7)">π Cycle</button>
  </div>
  <div class="cb on" id="cb1">
    <div class="g4" style="margin-bottom:7px">
      <div><label>From</label><select id="m1f" onchange="c1()"><option value="last">Last swing</option><option value="first">First</option><option value="custom">Custom</option></select></div>
      <div id="m1cw" style="display:none"><label>Custom price</label><input id="m1cp" type="number" value="0" oninput="c1()"></div>
      <div><label>Op</label><select id="m1op" onchange="c1()"><option value="m">× Multiply</option><option value="d">÷ Divide</option></select></div>
      <div><label>Ratio</label><select id="m1rat" onchange="c1()"></select></div>
      <div><label>Days forward</label><input id="m1d" type="number" value="30" oninput="c1()"></div>
      <div><label>Direction</label><select id="m1dir" onchange="c1()"><option value="1">Up ↑</option><option value="-1">Down ↓</option></select></div>
    </div>
    <div class="g4">
      <div class="res hi2"><p class="rlbl">Next PTV</p><p class="rval" id="m1np" style="color:#185FA5">—</p><p class="rsub" id="m1np2">—</p></div>
      <div class="res"><p class="rlbl">From price</p><p class="rval" id="m1fv" style="font-size:13px">—</p></div>
      <div class="res"><p class="rlbl">Price move</p><p class="rval" id="m1mv">—</p></div>
      <div class="res ok"><p class="rlbl">Price target</p><p class="rval" id="m1tg">—</p><p class="rsub" id="m1sb">—</p></div>
    </div>
    <div class="warn" id="m1w">Days too large for this PTV.</div>
  </div>
  <div class="cb" id="cb2">
    <div class="g4" style="margin-bottom:7px">
      <div><label>From</label><select id="m2f" onchange="c2()"><option value="last">Last swing</option><option value="first">First</option><option value="custom">Custom</option></select></div>
      <div id="m2cw" style="display:none"><label>Custom price</label><input id="m2cp" type="number" value="0" oninput="c2()"></div>
      <div><label>Op</label><select id="m2op" onchange="c2()"><option value="m">× Multiply</option><option value="d">÷ Divide</option></select></div>
      <div><label>Ratio</label><select id="m2rat" onchange="c2()"></select></div>
      <div><label>Target price</label><input id="m2tg" type="number" value="59044" oninput="c2()"></div>
    </div>
    <div class="g4">
      <div class="res hi2"><p class="rlbl">Next PTV</p><p class="rval" id="m2np" style="color:#185FA5">—</p></div>
      <div class="res"><p class="rlbl">From</p><p class="rval" id="m2fv" style="font-size:13px">—</p></div>
      <div class="res"><p class="rlbl">% / pts</p><p class="rval" id="m2pc">—</p></div>
      <div class="res ok"><p class="rlbl">Days</p><p class="rval" id="m2dy">—</p><p class="rsub" id="m2sb">—</p></div>
    </div>
    <div class="warn" id="m2w">Price > PTV. Use larger ratio.</div>
  </div>
  <div class="cb" id="cb3">
    <div class="info">Same angle → price AND date together (Chart I.B method)</div>
    <div class="g4" style="margin-bottom:7px">
      <div><label>From</label><select id="m3f" onchange="c3()"><option value="last">Last swing</option><option value="first">First</option><option value="custom">Custom</option></select></div>
      <div id="m3cw" style="display:none"><label>Custom price</label><input id="m3cp" type="number" value="0" oninput="c3()"></div>
      <div><label>Op</label><select id="m3op" onchange="c3()"><option value="m">× Multiply</option><option value="d">÷ Divide</option></select></div>
      <div><label>Ratio</label><select id="m3rat" onchange="c3()"></select></div>
      <div><label>Direction</label><select id="m3dir" onchange="c3()"><option value="1">Up ↑</option><option value="-1">Down ↓</option></select></div>
    </div>
    <div class="g4">
      <div class="res hi2"><p class="rlbl">Next PTV</p><p class="rval" id="m3np" style="color:#185FA5">—</p></div>
      <div class="res"><p class="rlbl">Angle</p><p class="rval" id="m3ag" style="font-size:13px">—</p></div>
      <div class="res ok"><p class="rlbl">Days</p><p class="rval" id="m3dy">—</p></div>
      <div class="res ok"><p class="rlbl">Price</p><p class="rval" id="m3pr">—</p><p class="rsub" id="m3pc">—</p></div>
    </div>
  </div>
  <div class="cb" id="cb4">
    <div class="g3" style="margin-bottom:7px">
      <div><label>From</label><select id="m4f" onchange="c4()"><option value="last">Last</option><option value="first">First</option><option value="custom">Custom</option></select></div>
      <div id="m4cw" style="display:none"><label>Custom price</label><input id="m4cp" type="number" value="0" oninput="c4()"></div>
      <div><label>Direction</label><select id="m4dir" onchange="c4()"><option value="1">Up ↑</option><option value="-1">Down ↓</option></select></div>
      <div><label>Days elapsed</label><input id="m4el" type="number" value="0" oninput="c4()"></div>
      <div><label>Target price</label><input id="m4tg" type="number" value="59044" oninput="c4()"></div>
    </div>
    <div style="overflow-x:auto"><table>
      <thead><tr><th>Op</th><th>Ratio</th><th>PTV</th><th>Days</th><th>From NOW</th><th>Price ↑↓</th><th>Days→Target</th></tr></thead>
      <tbody id="t4"></tbody>
    </table></div>
  </div>
  <div class="cb" id="cb5">
    <div class="info">Sin(ang)×PTV=price | Cos(ang)×PTV=hrs | Days=hrs÷hpd</div>
    <div class="g4" style="margin-bottom:7px">
      <div><label>PTV</label><input id="trp" type="number" value="124" oninput="ct()"></div>
      <div><label>Angle °</label><input id="tra" type="number" value="45" step="0.1" oninput="ct()"></div>
      <div><label>Start price</label><input id="trb" type="number" value="57456" oninput="ct()"></div>
      <div><label>Direction</label><select id="trd" onchange="ct()"><option value="1">Up ↑</option><option value="-1">Down ↓</option></select></div>
    </div>
    <div class="g4">
      <div class="res"><p class="rlbl">Price move</p><p class="rval" id="trpc" style="font-size:14px">—</p></div>
      <div class="res"><p class="rlbl">Hours</p><p class="rval" id="trhr" style="font-size:14px">—</p></div>
      <div class="res ok"><p class="rlbl">Days</p><p class="rval" id="trdy">—</p></div>
      <div class="res ok"><p class="rlbl">Price target</p><p class="rval" id="trpr">—</p></div>
    </div>
  </div>
  <div class="cb" id="cb6">
    <div class="info">Divide two PTVs → find which Cowan ratio connects them</div>
    <div class="g3" style="margin-bottom:7px">
      <div><label>PTV A</label><input id="rca" type="number" value="124" oninput="cr()"></div>
      <div><label>PTV B</label><input id="rcb" type="number" value="175.4" oninput="cr()"></div>
      <div class="res hi2"><p class="rlbl">B÷A</p><p class="rval" id="rcd" style="color:#185FA5">—</p><p class="rsub" id="rcm">—</p></div>
    </div>
    <div style="overflow-x:auto"><table>
      <thead><tr><th>Op</th><th>Ratio</th><th>Result</th><th>Diff</th><th>Error%</th><th>Match</th></tr></thead>
      <tbody id="t6"></tbody>
    </table></div>
  </div>
  <div class="cb" id="cb7">
    <div class="g4" style="margin-bottom:7px">
      <div><label>Base PTV</label><input id="pib" type="number" value="124" oninput="cpi()"></div>
      <div class="res"><p class="rlbl">×π</p><p class="rval" id="pi1" style="color:#185FA5;font-size:15px">—</p></div>
      <div class="res"><p class="rlbl">×√10</p><p class="rval" id="pi2" style="color:#185FA5;font-size:15px">—</p></div>
      <div class="res"><p class="rlbl">÷π</p><p class="rval" id="pi3" style="color:#f5a623;font-size:15px">—</p></div>
    </div>
    <div style="overflow-x:auto"><table>
      <thead><tr><th>Ratio</th><th>Value</th><th>PTV×</th><th>PTV÷</th></tr></thead>
      <tbody id="t7"></tbody>
    </table></div>
  </div>
</div>

<script>
  
// ============================================================
// GLOBALS
// ============================================================
let candles=[],manualPts=[],autoSwings=[];
let showAuto=true,showGann=true,showProj=true,showBal=false,showConfChart=false;
let snapMode='close';
let viewStart=0,viewEnd=0;
let isDragging=false,dragStartX=0,dragStartVS=0;
// PTV RULER
let rulerMode=false,rulerActive=false;
let rulerStartX=0,rulerStartY=0,rulerEndX=0,rulerEndY=0;
let rulerStartPrice=0,rulerStartBar=0;
// PTV FAN — mark A+B, radiate ratio lines from A through space
let fanMode=false;
let fanStep=0;       // 0=idle 1=A placed 2=both placed
let fanAPrice=0,fanADate=null;   // anchor A — stored as PRICE + DATE (fixed in space)
let fanBPrice=0,fanBDate=null;   // point B — stored as PRICE + DATE
let fanLivePrice=0,fanLiveDate=null;
// Keep bar refs only for initial placement, derive from date on redraw
let fanABar=0,fanBBar=0;

// COWAN ELLIPSE STATE
let ellMode=false;
let ellAngle=60;
// Copy/paste state
let ellCopied=null;   // stored {ptv, angle, minorR, SF} from copied ellipse
let ellPasteMode=false; // true = waiting for click to place pasted ellipse
let ellPasteBar=0, ellPastePrice=0; // where user clicked for paste A point
let ellPasteAngle=0;  // current rotation angle for paste (radians), mouse-controlled // mated angle system: 45, 60, 72, 90, or 'all'

function setEllAngle(a){
  ellAngle=a;
  const explains={45:'360°÷8=45° square (Gann)',60:'360°÷6=60° triangle (Cowan default)',72:'360°÷5=72° pentagon',90:'360°÷4=90° right angle','all':'All 4 angle systems'};
  [45,60,72,90,'all'].forEach(v=>{
    const el=document.getElementById('ell-ang-'+(v==='all'?'all':v));
    if(!el) return;
    if(v===a){ el.style.background='#a066e0';el.style.borderColor='#a066e0';el.style.color='#fff'; }
    else      { el.style.background='#fff';  el.style.borderColor='#ccc';   el.style.color='#666';}
  });
  const ex=document.getElementById('ell-ang-explain');
  if(ex) ex.textContent=explains[a]||'';
  draw();
}
let ellStep=0;        // 0=idle 1=A placed 2=both placed
let ellABar=0,ellAPrice=0,ellADate=null;
let ellBBar=0,ellBPrice=0,ellBDate=null;
let ellLiveBar=0,ellLivePrice=0;

// ============================================================
// SQUARE CHART — Time × Price Scaling
// ============================================================
let sqTimeScale=1;   // 1 bar = sqTimeScale price pts
let sqPriceScale=1;  // 1 price pt = sqPriceScale time units
let sqMode='bars';   // 'bars' or 'calendar'
let sqEffSF=1;       // effective scale factor = sqTimeScale / sqPriceScale

function updateSquare(){
  sqTimeScale=parseFloat(document.getElementById('sq-time').value)||1;
  sqPriceScale=parseFloat(document.getElementById('sq-price').value)||1;
  sqEffSF=sqTimeScale/sqPriceScale;
  document.getElementById('sq-eff-sf').textContent=sqEffSF.toFixed(4);
  document.getElementById('sq-eff-explain').textContent=
    '= Time('+sqTimeScale+') ÷ Price('+sqPriceScale+') = '+sqEffSF.toFixed(4);
  // Update angle display using last swing if available
  const pts=getActivePts();
  if(pts.length>=2){
    const p1=pts[pts.length-2],p2=pts[pts.length-1];
    const days=Math.abs(p2.idx-p1.idx);
    const pp=Math.abs(p2.price-p1.price);
    const tUnits=days*sqTimeScale;
    const pUnits=pp/sqPriceScale;
    const ang=Math.atan2(pUnits,tUnits)*180/Math.PI;
    document.getElementById('sq-angle').textContent=ang.toFixed(1)+'°';
    document.getElementById('sq-angle').style.color=Math.abs(ang-45)<5?'#1a8f60':'#c07800';
    document.getElementById('sq-last-swing').textContent=
      days+'bars × '+pp.toFixed(0)+'pts → T='+tUnits.toFixed(1)+' P='+pUnits.toFixed(1);
    document.getElementById('sq-last-swing-box').style.display='block';
  }
  const active=sqTimeScale!==1||sqPriceScale!==1;
  document.getElementById('sq-active-badge').style.display=active?'inline':'none';
  // Show visual squaring confirmation
  document.getElementById('sq-visual-box').style.display=active?'flex':'none';
  // Update angle box color
  const angleBox=document.getElementById('sq-angle-box');
  if(angleBox) angleBox.style.borderColor=active?'#1a8f60':'#f5d080';
  draw();
}

function setSqMode(m){
  sqMode=m;
  document.getElementById('sq-tb').classList.toggle('on',m==='bars');
  document.getElementById('sq-cd').classList.toggle('on',m==='calendar');
  draw();
}

function autoSquare(){
  // Auto-compute scale factors so last swing is at 45°
  // i.e. price_diff / time_diff (in bars) = scale factor
  const pts=getActivePts();
  if(pts.length<2){alert('Place at least 2 points on the chart first (or let auto-swings run)');return;}
  const p1=pts[pts.length-2],p2=pts[pts.length-1];
  const days=Math.abs(p2.idx-p1.idx)||1;
  const pp=Math.abs(p2.price-p1.price)||1;
  // For 45°: tUnits === pUnits
  // tUnits = days * sqTimeScale
  // pUnits = pp / sqPriceScale
  // Set sqTimeScale=pp/days, sqPriceScale=1
  const sf=(pp/days).toFixed(4);
  document.getElementById('sq-time').value=sf;
  document.getElementById('sq-price').value=1;
  updateSquare();
  // Also sync to balanced mode SF
  if(typeof balScaleFactor!=='undefined'){
    balScaleFactor=parseFloat(sf);
  }
}

function resetSquare(){
  document.getElementById('sq-time').value=1;
  document.getElementById('sq-price').value=1;
  updateSquare();
}

// Get the effective scale factor for ellipse/PTV geometry
// Priority: if sq panel has non-1 values, use those; else fall back to balScaleFactor
function getGeomSF(){
  if(sqTimeScale!==1||sqPriceScale!==1) return sqEffSF;
  if(typeof showBal!=='undefined'&&showBal&&typeof balScaleFactor!=='undefined'&&balScaleFactor>0) return balScaleFactor;
  return 1;
}

// PTV PROJECTOR -- free-hand move any PTV to future
// PTV FREE-DRAW PROJECTOR -- two click mode
// Click pt1 -> click pt2 -> PTV calculated -> all ratios projected from pt2
let projMoveMode=false;
let projStep=0;         // 0=idle 1=pt1 placed waiting for pt2 2=both placed
let projPt1Bar=0,projPt1Price=0;   // first click (start of PTV)
let projPt2Bar=0,projPt2Price=0;   // second click (end of PTV = origin of projections)
let projLiveBar=0,projLivePrice=0; // mouse position while drawing
// keep these for compat
let projMoveSelected=false,projMovePTV=0,projMoveAng=0,projMoveLabel='';
let touchStartX=0,touchStartVS=0;
let balPTV=0,balComponent=0,balRatioFactor=1,balWhichSmaller='time';

const COLORS=['#185FA5','#f5a623','#1a8f60','#c04040','#a66af5','#50c8f5','#e08040','#f5e050'];
const NAMES='ABCDEFGHIJKLMNOPQRSTUVWXYZ';
const RATS=[
  [0.25,'0.25'],[0.33,'0.33'],[0.382,'0.382'],[0.5,'0.5'],[0.618,'0.618'],
  [0.707,'1/sqrt2'],[1,'1'],[1.333,'4/3'],[1.382,'1.382'],[1.414,'sqrt2'],
  [1.5,'1.5'],[1.618,'PHI'],[1.732,'sqrt3'],[2,'2'],[2.236,'sqrt5'],
  [2.5,'2.5'],[3,'3'],[3.14159,'\u03c0'],[4,'4'],[5,'5']
];
const BAL_RATS=[[0.382,'0.382'],[0.5,'0.5'],[0.618,'0.618'],[1,'1'],[1.382,'1.382'],[1.5,'1.5'],[1.618,'PHI'],[2,'2'],[2.236,'sqrt5'],[3,'3'],[3.14159,'\u03c0']];
const SS_RATS=new Set([0.382,0.5,0.618,1,1.382,1.5,1.618,2]);
const GANN_ANGLES=[[75,'2x1','rgba(255,140,40,.5)'],[63.4,'1x1','rgba(255,220,40,.75)'],[45,'45deg','rgba(40,220,255,.85)'],[33.7,'1x2','rgba(120,255,120,.5)'],[21.8,'1x3','rgba(180,120,255,.38)']];
const KEY_RATS=[0.5,0.618,1,1.414,1.618,1.732,2,2.236,3,3.14,4];
const SIG_RATS=[0.618,1,1.414,1.618,2,2.236]; // ratios used for signal generation

// ============================================================
// SCALE
// ============================================================
function isLog(){
  const s=document.getElementById('scl').value;
  if(s==='log') return true; if(s==='arith') return false;
  if(candles.length<5) return false;
  return Math.abs(candles[candles.length-1].c-candles[0].c)/(candles.length*(parseFloat(document.getElementById('hpd').value)||6.5))>10;
}
function pd(a,b){
  if(showBal) return Math.abs(b-a); // balanced always uses raw points
  return isLog()?Math.abs(Math.log(Math.max(b,.01)/Math.max(a,.01)))*100:Math.abs(b-a);
}
function pf(base,v,dir){
  if(showBal) return base+dir*v; // balanced: v is already in real price units
  return isLog()?base*Math.exp(dir*v/100):base+dir*v;
}
function fm(v){
  if(showBal) return v.toFixed(2)+' pts (bal)';
  return v.toFixed(2)+(isLog()?'%':' pts');
}
function fmDate(d){return d?d.toLocaleDateString('en-IN',{day:'2-digit',month:'short',year:'2-digit'}):'';}

// ============================================================
// SWING DETECTION
// ============================================================
function detectSwings(){
  if(candles.length<5) return [];
  const minPct=parseFloat(document.getElementById('swpct').value)||3;

  // Get from-date filter
  const fromDateVal=document.getElementById('swing-from').value;
  let fromIdx=0;
  if(fromDateVal){
    const fromMs=new Date(fromDateVal).getTime();
    // Find first candle on or after that date
    for(let i=0;i<candles.length;i++){
      if(candles[i].t.getTime()>=fromMs){fromIdx=i;break;}
    }
  }

  const sw=[];
  let lastDir=0,lastIdx=fromIdx,lastPrice=candles[fromIdx].c;
  for(let i=fromIdx+1;i<candles.length;i++){
    const c=candles[i];
    const up=(c.h-lastPrice)/Math.max(lastPrice,.01)*100;
    const dn=(lastPrice-c.l)/Math.max(lastPrice,.01)*100;
    if(lastDir>=0&&dn>=minPct){if(lastDir>0)sw.push({idx:lastIdx,price:candles[lastIdx].h,type:'H'});lastDir=-1;lastIdx=i;lastPrice=c.l;}
    else if(lastDir<=0&&up>=minPct){if(lastDir<0)sw.push({idx:lastIdx,price:candles[lastIdx].l,type:'L'});lastDir=1;lastIdx=i;lastPrice=c.h;}
    else{if(lastDir>=0&&c.h>lastPrice){lastPrice=c.h;lastIdx=i;}if(lastDir<0&&c.l<lastPrice){lastPrice=c.l;lastIdx=i;}}
  }
  if(lastDir>0)sw.push({idx:lastIdx,price:candles[lastIdx].h,type:'H'});
  else sw.push({idx:lastIdx,price:candles[lastIdx].l,type:'L'});
  sw.forEach((s,i)=>s.label=NAMES[i%26]);
  return sw;
}

// ============================================================
// PTV MATH
// ============================================================
// -- BALANCED 45deg PTV ENGINE ------------------------------
// When showBal=true, ALL calcPTV calls use this method:
// 1. raw price change and days
// 2. bigger / smaller = scaleFactor
// 3. multiply smaller by scaleFactor -> both equal -> 45deg
// 4. PTV = balanced_component x sqrt2
// 5. angle = always 45deg
// Also stores the scaleFactor globally so draw() can scale the chart

let balScaleFactor=1; // used by draw() to scale Y axis
let balDecimalShift=1; // /1, /10, /100, /1000 -- user controlled

function setBalShift(n){
  balDecimalShift=n;
  // Update button states
  [1,10,100,1000].forEach(v=>{
    const b=document.getElementById('bsb-'+v);
    if(b) b.classList.toggle('on',v===n);
  });
  document.getElementById('bal-custom-shift').value=n;
  calcBal();
  if(showBal) draw();
}
function setBalShiftCustom(){
  const v=parseFloat(document.getElementById('bal-custom-shift').value)||1;
  balDecimalShift=Math.max(0.001,v);
  [1,10,100,1000].forEach(x=>{
    const b=document.getElementById('bsb-'+x);
    if(b) b.classList.remove('on');
  });
  calcBal();
  if(showBal) draw();
}

function calcPTV_balanced(a,b){
  const dayDiff=Math.abs(b.idx-a.idx);
  if(dayDiff===0) return{ptv:0,ang:45,pp:0,hrs:0,dayDiff:0,scaleFactor:1,effSF:1,balanced:0,priceWasBigger:true};
  const rawPrice=Math.abs(b.price-a.price);
  if(rawPrice===0) return{ptv:0,ang:45,pp:0,hrs:0,dayDiff,scaleFactor:1,effSF:1,balanced:0,priceWasBigger:true};

  // Raw bigger / smaller
  const bigger=Math.max(rawPrice,dayDiff);
  const smaller=Math.min(rawPrice,dayDiff);
  const rawSF=bigger/smaller;

  // Apply decimal shift -- divide scale factor by shift value
  const effSF=rawSF/balDecimalShift;

  // Balanced components using effective SF
  const priceWasBigger=rawPrice>=dayDiff;
  const bPrice=priceWasBigger ? rawPrice/balDecimalShift : rawPrice*effSF;
  const bTime= priceWasBigger ? dayDiff*effSF           : dayDiff/balDecimalShift;

  // Both components should now be equal = balanced value
  const balanced=priceWasBigger ? rawPrice/balDecimalShift : dayDiff/balDecimalShift;
  const ptv=Math.sqrt(bPrice*bPrice+bTime*bTime); // ~= balanced x sqrt2

  return{ptv,ang:45,pp:bPrice,hrs:bTime,dayDiff,
    scaleFactor:rawSF,effSF,priceWasBigger,
    rawPrice,balanced,bPrice,bTime};
}

function calcPTV(a,b){
  if(showBal){
    const r=calcPTV_balanced(a,b);
    // Update global scale factor from the most recent swing pair
    balScaleFactor=r.effSF||r.scaleFactor;
    return r;
  }
  // ORIGINAL method -- unchanged
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const dayDiff=Math.abs(b.idx-a.idx);
  const hrs=dayDiff*hpd;
  const pp=pd(a.price,b.price);
  const ptv=Math.sqrt(pp*pp+hrs*hrs);
  return{ptv,ang:Math.atan2(pp,hrs)*180/Math.PI,pp,hrs,dayDiff,scaleFactor:1};
}
function getActivePts(){return[...(showAuto?autoSwings:[]),...manualPts];}
function getLastTwo(){
  const pts=getActivePts();if(pts.length<2) return null;
  return{prev:pts[pts.length-2],last:pts[pts.length-1],ptv:calcPTV(pts[pts.length-2],pts[pts.length-1])};
}
function getFromPrice(fId,cpId){
  const f=document.getElementById(fId).value;
  if(f==='custom') return parseFloat(document.getElementById(cpId).value)||0;
  if(f==='first'){const pts=getActivePts();return pts.length?pts[0].price:0;}
  const pts=getActivePts();return pts.length?pts[pts.length-1].price:0;
}
function getNP(opId,ratId){
  const lt=getLastTwo();if(!lt) return 0;
  const op=document.getElementById(opId).value,r=parseFloat(document.getElementById(ratId).value)||1;
  return op==='m'?lt.ptv.ptv*r:lt.ptv.ptv/r;
}

// ============================================================
// HH / LL / HL / LH VECTORS
// ============================================================
function computeHHLLVectors(pts_override){
  const pts=(pts_override||getActivePts()).filter(p=>p.type==='H'||p.type==='L');
  if(pts.length<2) return{hh:[],ll:[],hl:[],lh:[]};
  const highs=pts.filter(p=>p.type==='H');
  const lows=pts.filter(p=>p.type==='L');
  function pairs(arr,type){
    const res=[];
    for(let i=0;i<arr.length-1;i++){
      const{ptv,ang,dayDiff}=calcPTV(arr[i],arr[i+1]);
      res.push({from:arr[i],to:arr[i+1],ptv,ang,dayDiff,type,label:arr[i].label+'->'+arr[i+1].label});
    }
    return res;
  }
  const hl=[],lh=[];
  for(let i=0;i<pts.length-1;i++){
    const a=pts[i],b=pts[i+1];
    const{ptv,ang,dayDiff}=calcPTV(a,b);
    if(a.type==='H'&&b.type==='L') hl.push({from:a,to:b,ptv,ang,dayDiff,type:'HL',label:a.label+'->'+b.label});
    if(a.type==='L'&&b.type==='H') lh.push({from:a,to:b,ptv,ang,dayDiff,type:'LH',label:a.label+'->'+b.label});
  }
  return{hh:pairs(highs,'HH'),ll:pairs(lows,'LL'),hl,lh};
}

// ============================================================
// SIGNAL ENGINE -- BUY / SELL
// Uses HH,LL,HL,LH vectors -- projects forward with KEY ratios
// Clusters by date(3d) + price(1.5%)
// BUY = LL/LH pointing to LOW zone; SELL = HH/HL pointing to HIGH zone
// ============================================================
function computeSignals(pts_override, maxDaysAhead){
  const pts=pts_override||getActivePts();
  if(pts.length<2||!candles.length) return[];
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const lastCandle=candles.length-1;
  const{hh,ll,hl,lh}=computeHHLLVectors(pts);
  const allVecs=[...hh,...ll,...hl,...lh];
  if(!allVecs.length) return[];
  const maxDays=maxDaysAhead||120;
  const cands=[];

  allVecs.forEach(vec=>{
    SIG_RATS.forEach(rv=>{
      const np=vec.ptv*rv;
      const nH=Math.cos(vec.ang*Math.PI/180)*np;
      const nP=Math.sin(vec.ang*Math.PI/180)*np;
      const daysFwd=nH/hpd;
      const daysFromNow=vec.to.idx+daysFwd-lastCandle;
      if(daysFromNow<-5||daysFromNow>maxDays) return;
      [1,-1].forEach(dir=>{
        const price=pf(vec.to.price,nP,dir);
        if(price<=0) return;
        // BUY signals from LOW projections (LL vectors going to next LOW, LH going up then proj LOW)
        // SELL signals from HIGH projections
        const signalType=dir>0?'SELL':'BUY';
        cands.push({vecType:vec.type,vecLabel:vec.label,rv,np,daysFromNow,price,signalType,dir,vecObj:vec});
      });
    });
  });

  // Cluster
  const clusters=[];
  cands.forEach(c=>{
    let matched=false;
    for(const cl of clusters){
      if(cl.signalType!==c.signalType) continue;
      const dd=Math.abs(cl.avgDays-c.daysFromNow);
      const pd2=Math.abs(cl.avgPrice-c.price)/Math.max(cl.avgPrice,1)*100;
      if(dd<=3&&pd2<1.5){
        cl.items.push(c);
        cl.avgDays=(cl.avgDays*(cl.items.length-1)+c.daysFromNow)/cl.items.length;
        cl.avgPrice=(cl.avgPrice*(cl.items.length-1)+c.price)/cl.items.length;
        const vt=new Set(cl.items.map(x=>x.vecType));
        const ul=new Set(cl.items.map(x=>x.vecLabel));
        cl.vecTypes=[...vt];cl.uniqueVecs=ul.size;
        cl.score=cl.items.length+vt.size*3+ul.size*2;
        matched=true;break;
      }
    }
    if(!matched) clusters.push({
      signalType:c.signalType,items:[c],avgDays:c.daysFromNow,avgPrice:c.price,
      vecTypes:[c.vecType],uniqueVecs:1,score:1
    });
  });

  return clusters.sort((a,b)=>b.score-a.score);
}

// ============================================================
// NEXT PTV WATCH ENGINE
// Logic:
// 1. Get all completed swings + their PTVs
// 2. For each swing, compute ratio-based targets for the NEXT swing
// 3. Measure the DEVELOPING swing's current PTV (last completed point -> current candle)
// 4. Compare developing PTV against each target  show progress %
// 5. When developing PTV is within tolerance of a target -> ALERT
// 

// 
// PTV TRADE PANEL  Base PTV + Rhythm + Developing
// 

// 
// PTV RHYTHM SEQUENCE ENGINE
// What the chart shows: consecutive PTV ratios form a pattern
// 2304 -> 3970 (xsqrt3) -> 5914 (x1.5) -> next = ?
// Find the ratio sequence, predict next PTV, watch developing
// 

// All Cowan ratios for matching
const SEQ_RATS=[
  [0.25,'0.25'],[0.33,'0.33'],[0.382,'0.382'],[0.5,'0.5'],[0.618,'0.618'],
  [0.707,'1/sqrt2'],[0.75,'0.75'],[0.786,'0.786'],[1,'1x'],[1.272,'1.272'],
  [1.333,'4/3'],[1.382,'1.382'],[1.414,'sqrt2'],[1.5,'1.5'],
  [1.618,'PHI'],[1.732,'sqrt3'],[2,'2x'],
  [2.236,'sqrt5'],[2.5,'2.5'],[2.618,'PHI'],[3,'3x'],[3.14159,'\u03c0'],
  [4,'4x'],[5,'5x'],[6.283,'2pi']
];

function nearestCowanRatio(r){
  // Use ABSOLUTE error (not relative) so pi(3.14) and PHI(2.618)
  // don't get confused when ratio is near 3.14
  let best=SEQ_RATS[0],bd=999;
  SEQ_RATS.forEach(([v,n])=>{
    const d=Math.abs(r-v); // absolute difference
    if(d<bd){bd=d;best=[v,n];}
  });
  // Return both absolute and percentage error
  const absErr=bd;
  const pctErr=(bd/best[0])*100;
  return{rv:best[0],rn:best[1],err:pctErr,absErr};
}

function updateTradePanel(){
  // KEY FIX: if manual points exist, use ONLY manual points
  // Auto swings pick up tiny wiggles (PTV 116) that pollute the matrix
  // Manual points = YOUR meaningful swings (PTV 598, 1891 etc)
  const allPts=getActivePts().filter(p=>p.type==='H'||p.type==='L'||p.type==='M');
  const manualOnly=manualPts.filter(p=>p.type==='H'||p.type==='L'||p.type==='M');
  // Use manual if 2+ manual points placed, else fall back to auto
  const pts=manualOnly.length>=2?allPts:allPts;
  // But for swings table -- separate auto from manual clearly
  const swingPts=manualOnly.length>=2
    ?[...manualPts].sort((a,b)=>a.idx-b.idx)  // manual only for swing table
    :allPts;  // auto if no manual
  const tol=parseFloat(document.getElementById('trade-tol').value)||5;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const lastCandle=candles.length-1;
  const currentPrice=candles[lastCandle]?.c||0;

  const seqBody=document.getElementById('seq-body');
  const seqFoot=document.getElementById('seq-foot');
  seqBody.innerHTML=''; seqFoot.innerHTML='';

  if(swingPts.length<2){
    seqBody.innerHTML='<tr><td colspan="7" style="color:#444;padding:8px;font-size:12px">Need 2+ points. Place manual points (click chart) to define YOUR swings, or load data with Auto ON.</td></tr>';
    document.getElementById('trade-dev-ptv').textContent='';
    document.getElementById('trade-alert-badge').style.display='none';
    document.getElementById('trade-dev-badge').style.display='none';
    document.getElementById('trade-action-box').style.display='none';
    document.getElementById('price-pred-box').style.display='none';
    return;
  }

  // -- BUILD SEQUENCE from swingPts -------------------------
  const swings=[];
  for(let i=0;i<swingPts.length-1;i++){
    const r=calcPTV(swingPts[i],swingPts[i+1]);
    swings.push({ptv:r.ptv,ang:r.ang,dayDiff:r.dayDiff,
      label:swingPts[i].label+'->'+swingPts[i+1].label,
      from:swingPts[i],to:swingPts[i+1],type:swingPts[i+1].type});
  }

  // -- RENDER SEQUENCE TABLE ---------------------------------
  swings.forEach((sw,i)=>{
    const prev=i>0?swings[i-1]:null;
    const ratio=prev?sw.ptv/prev.ptv:null;
    const match=ratio?nearestCowanRatio(ratio):null;
    const isMatch=match&&match.err<=tol;

    // Predicted next PTV using this swing's ratio
    const predictedNext=match?sw.ptv*match.rv:null;

    const tr=document.createElement('tr');
    const typeCol=sw.type==='H'?'#1a8f60':'#c04040';
    const ratCol=isMatch?'#f5a623':'#555';
    const isLast=i===swings.length-1;
    if(isLast) tr.style.cssText='background:rgba(24,95,165,.1);outline:1px solid rgba(24,95,165,.3)';

    const errSpan=isMatch?'<span style="font-size:9px;color:#555;margin-left:4px">('+match.err.toFixed(1)+'% err)</span>':'';
    const ratSpan=match?'<span style="padding:2px 8px;border-radius:20px;background:'+(isMatch?'rgba(245,166,35,.15)':'transparent')+';border:'+(isMatch?'1px solid rgba(245,166,35,.4)':'none')+'">'+match.rn+'</span>':'--';
    const predSpan=predictedNext?'<span style="color:#1a8f60">'+predictedNext.toFixed(2)+'</span> <span style="font-size:10px;color:#444">('+sw.ptv.toFixed(1)+'x'+(match?match.rn:'?')+')</span>':'--';
    tr.innerHTML=
      '<td style="font-weight:700;color:'+typeCol+';font-size:12px">'+sw.label+'</td>'
      +'<td style="font-weight:700;color:#ccd;font-size:14px">'+sw.ptv.toFixed(2)+'</td>'
      +'<td style="font-weight:700;color:'+ratCol+';font-size:14px">'+(ratio?ratio.toFixed(4):'--')+' '+errSpan+'</td>'
      +'<td style="font-weight:700;font-size:13px;color:'+(isMatch?'#f5a623':'#333')+'">'+ratSpan+'</td>'
      +'<td style="font-weight:700;color:#1a8f60;font-size:13px">'+predSpan+'</td>'
      +'<td style="font-size:11px;color:#666">'+sw.dayDiff+'d</td>'
      +'<td><span style="font-size:10px;padding:1px 5px;border-radius:3px;background:'+typeCol+'22;color:'+typeCol+'">'+(sw.type==='H'?'HIGH':'LOW')+'</span></td>';
    seqBody.appendChild(tr);
  });

  //  DEVELOPING SWING 
  // Use LAST point from swingPts (manual if available) not auto
  const lastSw=swingPts[swingPts.length-1];
  const lastSwingObj=swings[swings.length-1];
  const devDays=lastCandle-lastSw.idx;
  const devPPraw=Math.abs(currentPrice-lastSw.price);
  const devHrs=devDays*hpd;
  let devPTV=0;
  if(devDays>0){
    const devPP=showBal?devPPraw/balDecimalShift:devPPraw;
    const devT=showBal?devDays*(balScaleFactor||1):devHrs;
    devPTV=Math.sqrt(devPP*devPP+devT*devT);
  }
  const devDir=currentPrice>=lastSw.price?'UP UP':'DN DOWN';

  document.getElementById('trade-dev-badge').style.display=devPTV>0?'inline':'none';
  document.getElementById('trade-dev-ptv').textContent=devPTV>0?devPTV.toFixed(2):'--';
  document.getElementById('trade-dev-sub').textContent=devPTV>0?
    `${devDays}d from ${lastSw.label} (${lastSw.type==='H'?'High':'Low'} ${Math.round(lastSw.price).toLocaleString('en-IN')}) ${devDir}`:'no data';

  //  PREDICT NEXT PTV from last completed swing 
  // Look at the ratio pattern in last 3 swings
  const lastRatio=swings.length>=2?swings[swings.length-1].ptv/swings[swings.length-2].ptv:null;
  const prevRatio=swings.length>=3?swings[swings.length-2].ptv/swings[swings.length-3].ptv:null;
  const lastMatch=lastRatio?nearestCowanRatio(lastRatio):null;
  const prevMatch=prevRatio?nearestCowanRatio(prevRatio):null;

  // Predicted next PTVs: use last ratio, prev ratio, and common alternating
  const predictions=[];
  const curPTV=swings[swings.length-1]?.ptv||0;
  if(curPTV>0){
    // From last ratio
    if(lastMatch) predictions.push({label:`Same ratio x${lastMatch.rn}`,ptv:curPTV*lastMatch.rv,reason:'continuing pattern',priority:1});
    // From prev ratio (alternating pattern)
    if(prevMatch&&(!lastMatch||prevMatch.rn!==lastMatch.rn))
      predictions.push({label:`Alternate x${prevMatch.rn}`,ptv:curPTV*prevMatch.rv,reason:'alternating pattern',priority:2});
    // Standard Cowan: x1 (equal), xsqrt2, xPHI
    [[1,'1x'],[1.414,'sqrt2'],[1.618,'PHI'],[1.732,'sqrt3'],[2,'2x']].forEach(([rv,rn])=>{
      if(!predictions.find(p=>Math.abs(p.ptv-curPTV*rv)/Math.max(curPTV*rv,1)<0.05))
        predictions.push({label:`x${rn}`,ptv:curPTV*rv,reason:'standard ratio',priority:3});
    });
  }
  predictions.sort((a,b)=>a.priority-b.priority||a.ptv-b.ptv);

  // Find nearest prediction to developing PTV
  const nearest=devPTV>0?predictions.reduce((best,p)=>{
    const d=Math.abs(devPTV-p.ptv)/p.ptv*100;
    return d<best.diff?{...p,diff:d}:best;
  },{...predictions[0],diff:999}):null;
  const inZone=nearest&&nearest.diff<=tol&&devPTV>0;

  // Progress bar
  if(nearest&&devPTV>0){
    const pct=Math.min(devPTV/nearest.ptv*100,100);
    document.getElementById('trade-dev-bar').style.width=pct+'%';
    document.getElementById('trade-dev-bar').style.background=inZone?'#f5a623':pct>=80?'#1a8f60':'#185FA5';
    document.getElementById('trade-dev-target-label').textContent=
      `-> nearest: ${nearest.label} = ${nearest.ptv.toFixed(2)} (${nearest.diff.toFixed(1)}% away)`;
    document.getElementById('trade-dev-pct').textContent=(devPTV/nearest.ptv*100).toFixed(1)+'%';
  }

  // Predictions box
  document.getElementById('trade-sig-label').textContent=
    `Predicted next PTVs (from ${lastSwingObj?.ptv.toFixed(2)||'?'})`;
  document.getElementById('trade-predictions').innerHTML=predictions.slice(0,6).map(p=>{
    const isNearest=nearest&&Math.abs(p.ptv-nearest.ptv)<0.1;
    const pct=devPTV>0?(devPTV/p.ptv*100).toFixed(0)+'%':'--';
    const bar=devPTV>0?`<div style="display:inline-block;width:${Math.min(devPTV/p.ptv*100,100)}%;max-width:80px;height:3px;background:${isNearest?'#f5a623':'#1a5c20'};border-radius:2px;margin-left:6px;vertical-align:middle"></div>`:'';
    return `<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:3px;${isNearest?'background:rgba(245,166,35,.12);border-radius:4px;padding:1px 4px':''}">
      <span style="color:${isNearest?'#f5a623':'#3a9f50'};font-weight:${isNearest?'700':'400'}">${p.label}</span>
      <span style="font-weight:700;color:${isNearest?'#f5a623':'#ccd'}">${p.ptv.toFixed(2)}</span>
      <span style="font-size:10px;color:#444">${pct}${bar}</span>
    </div>`;
  }).join('');

  //  DEVELOPING ROW in table 
  if(devPTV>0){
    const devMatch=nearestCowanRatio(devPTV/lastSwingObj.ptv);
    const devIsMatch=devMatch.err<=tol;
    const tr=document.createElement('tr');
    tr.style.cssText='background:rgba(245,166,35,.12);outline:2px solid rgba(245,166,35,.4)';
    const devRatioSpan='<span style="font-size:9px;color:#555;margin-left:3px">('+devMatch.err.toFixed(1)+'%)</span>';
    const devRatLabel=devIsMatch
      ?'<span style="padding:2px 8px;border-radius:20px;background:rgba(245,166,35,.2);border:1px solid rgba(245,166,35,.5);color:#f5a623">'+devMatch.rn+' </span>'
      :'<span style="color:#555">'+devMatch.rn+' ('+devMatch.err.toFixed(1)+'% off)</span>';
    tr.innerHTML=
      '<td style="font-weight:700;color:#f5a623;font-size:12px"> '+lastSw.label+'->? (dev)</td>'
      +'<td style="font-weight:700;color:#f5a623;font-size:16px">'+devPTV.toFixed(2)+'</td>'
      +'<td style="font-weight:700;color:'+(devIsMatch?'#f5a623':'#555')+';font-size:13px">'+(devPTV/lastSwingObj.ptv).toFixed(4)+' '+(devIsMatch?devRatioSpan:'')+'</td>'
      +'<td style="font-weight:700;font-size:13px">'+devRatLabel+'</td>'
      +'<td colspan="3" style="font-size:11px;color:'+(inZone?'#f5a623':'#555')+'">'
      +(inZone?' IN RATIO ZONE -- watch for reversal':'-> developing, '+devDays+'d so far, '+devDir)+'</td>';
    seqFoot.appendChild(tr);
  }

  //  ACTION BOX 
  document.getElementById('trade-alert-badge').style.display=inZone?'inline':'none';
  const actionBox=document.getElementById('trade-action-box');
  if(inZone){
    const isBuy=lastSw.type==='H'; // developing downswing -> BUY at bottom
    const col=isBuy?'#1a8f60':'#c04040';
    actionBox.style.display='block';
    actionBox.style.borderColor=col;
    actionBox.style.background=col+'0d';
    document.getElementById('trade-sig-title').textContent=
      `${isBuy?' BUY':' SELL'}  Developing PTV ${devPTV.toFixed(2)} ~= ${nearest.label} (${nearest.err.toFixed(1)}% from zone)`;
    document.getElementById('trade-sig-title').style.color=col;
    document.getElementById('trade-entry').textContent=Math.round(currentPrice).toLocaleString('en-IN');
    document.getElementById('trade-entry').style.color=col;
    const stop=isBuy?Math.round(Math.min(currentPrice,lastSw.price)*0.995):Math.round(Math.max(currentPrice,lastSw.price)*1.005);
    document.getElementById('trade-stop').textContent=stop.toLocaleString('en-IN');
    // Targets: next PTV x same ratio from current price
    const t1ptv=nearest.ptv*(lastMatch?.rv||1);
    const t1price=isBuy?Math.round(currentPrice+t1ptv*(showBal?balDecimalShift:1)):Math.round(currentPrice-t1ptv*(showBal?balDecimalShift:1));
    document.getElementById('trade-t1').textContent=t1price.toLocaleString('en-IN');
    const t2ptv=nearest.ptv*1.618;
    const t2price=isBuy?Math.round(currentPrice+t2ptv*(showBal?balDecimalShift:1)):Math.round(currentPrice-t2ptv*(showBal?balDecimalShift:1));
    document.getElementById('trade-t2').textContent=t2price.toLocaleString('en-IN');
    document.getElementById('trade-sig-reason').textContent=
      `Ratio pattern: ${prevMatch?prevMatch.rn+' -> ':''}${lastMatch?lastMatch.rn+' -> ':''}${nearest.rn} (developing). `+
      `Last ${swings.length} swings confirm this rhythm.`;
  } else {
    actionBox.style.display='none';
  }

  //  PRICE MATCH PREDICTIONS 
  // When dev PTV div any historical PTV = exact Cowan ratio (within tol%)
  // -> The developing swing is "complete" by Cowan geometry
  // -> Predict entry price + next PTV targets in REAL PRICE
  const predBox=document.getElementById('price-pred-box');
  const predCards=document.getElementById('price-pred-cards');
  predCards.innerHTML='';

  const pricePreds=[];
  swings.forEach((sw,si)=>{
    if(devPTV<=0||sw.ptv<=0) return;
    const devRatio=devPTV/sw.ptv;
    const match=nearestCowanRatio(devRatio);
    if(match.err>tol) return; // only exact hits

    // Dev PTV matches sw.ptv x ratio -> developing swing is at a ratio zone of sw
    const isBuy=lastSw.type==='H'; // coming down from high = buy expected
    const entryPrice=currentPrice;
    const hpd2=parseFloat(document.getElementById('hpd').value)||6.5;

    // Next PTV prediction: use same ratio (rhythm) or alternate
    // Convert PTV back to real price move:
    // For original mode: price component = sin(ang) x PTV
    // For balanced mode: price component = PTV/sqrt2 x decimalShift
    function ptvToPrice(ptv2, ang2){
      if(showBal) return (ptv2/Math.SQRT2)*balDecimalShift;
      return Math.sin(ang2*Math.PI/180)*ptv2;
    }

    // Most likely next PTV = same PTV as current historical swing (rhythm)
    // OR same ratio continues: next = devPTV x ratio
    const nextPTV1=sw.ptv; // next swing = same as sw (rhythm repeat)
    const nextPTV2=devPTV*match.rv; // next = dev x ratio (continuation)
    const nextPTV3=sw.ptv*match.rv; // next = sw x ratio (escalation)

    const devAng2=Math.atan2(Math.abs(currentPrice-lastSw.price),
      (lastCandle-lastSw.idx)*(showBal?1:hpd2))*180/Math.PI;

    const pm1=ptvToPrice(nextPTV1,devAng2);
    const pm2=ptvToPrice(nextPTV2,devAng2);
    const pm3=ptvToPrice(nextPTV3,devAng2);

    pricePreds.push({
      swingLabel:sw.label,swingPTV:sw.ptv,
      devPTV,ratio:devRatio,match,
      isBuy,entryPrice,
      targets:[
        {label:'Rhythm repeat (x'+match.rn+')',ptv:nextPTV1,
          up:Math.round(entryPrice+pm1),dn:Math.round(entryPrice-pm1),priority:1},
        {label:'Continuation (devx'+match.rn+')',ptv:nextPTV2,
          up:Math.round(entryPrice+pm2),dn:Math.round(entryPrice-pm2),priority:2},
        {label:'Escalation (swx'+match.rn+')',ptv:nextPTV3,
          up:Math.round(entryPrice+pm3),dn:Math.round(entryPrice-pm3),priority:3},
      ],
      err:match.err
    });
  });

  // Sort by best match accuracy
  pricePreds.sort((a,b)=>a.err-b.err);

  if(pricePreds.length>0){
    predBox.style.display='block';
    let logicLines=[];

    pricePreds.slice(0,4).forEach(pred=>{
      const isBuy=pred.isBuy;
      const col=isBuy?'#1a8f60':'#c04040';
      const card=document.createElement('div');
      card.style.cssText=`background:${col}0d;border:2px solid ${col}66;border-radius:9px;padding:9px 11px;position:relative`;

      // Signal strength badge
      const strength=pred.err<1?' EXACT':pred.err<3?' STRONG':'~ GOOD';
      const strengthCol=pred.err<1?'#f5a623':pred.err<3?'#1a8f60':'#555';

      card.innerHTML=`
        <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:6px">
          <div style="font-size:12px;font-weight:700;color:${col}">${isBuy?' BUY':' SELL'} SIGNAL</div>
          <span style="font-size:10px;font-weight:700;padding:2px 7px;border-radius:20px;
            background:${strengthCol}22;color:${strengthCol};border:1px solid ${strengthCol}44">${strength} ${pred.err.toFixed(2)}%</span>
        </div>
        <div style="font-size:11px;color:#888;margin-bottom:6px">
          Dev PTV <b style="color:#f5a623">${pred.devPTV.toFixed(2)}</b>
          div ${pred.swingLabel}(<b>${pred.swingPTV.toFixed(2)}</b>)
          = <b style="color:#f5a623">${pred.ratio.toFixed(4)}</b>
          ~= <b style="color:#f5a623">${pred.match.rn}</b>
        </div>
        <div style="font-size:11px;color:#555;margin-bottom:5px">
          Entry: <b style="color:${col};font-size:13px">${Math.round(pred.entryPrice).toLocaleString('en-IN')}</b>
        </div>
        <div style="font-size:10px;font-weight:700;color:#444;text-transform:uppercase;margin-bottom:4px">Price targets:</div>
        ${pred.targets.map(t=>`
          <div style="display:flex;justify-content:space-between;align-items:center;
            padding:2px 5px;border-radius:4px;margin-bottom:2px;background:rgba(255,255,255,.02)">
            <span style="font-size:10px;color:#555">${t.label}</span>
            <span>
              <b style="color:#1a8f60;font-size:11px">UP${t.up.toLocaleString('en-IN')}</b>
              <span style="color:#333;margin:0 4px"></span>
              <b style="color:#c04040;font-size:11px">DN${t.dn.toLocaleString('en-IN')}</b>
            </span>
          </div>`).join('')}`;
      predCards.appendChild(card);

      logicLines.push(`${pred.swingLabel}(${pred.swingPTV.toFixed(1)}) x ${pred.match.rn} = ${(pred.swingPTV*pred.match.rv).toFixed(1)} ~= dev(${pred.devPTV.toFixed(1)}) [err ${pred.err.toFixed(2)}%]`);
    });

    document.getElementById('price-pred-logic').innerHTML=
      `<b style="color:#444">Ratio proof:</b> `+logicLines.join(' &nbsp;&nbsp; ')+
      `<br><b style="color:#444">Logic:</b> Dev swing PTV matches historical swing PTV by an exact Cowan ratio -> `+
      `Cowan geometry says this is a natural turning point -> `+
      `<b style="color:#f5a623">enter at current price, target = next swing's price move</b>`;
  } else {
    predBox.style.display='none';
  }

  //  PTV RATIO MATRIX 
  // BOTH directions: AdivB AND BdivA  always show both
  // 1885div598=pi AND 598div1885=1/pi  both valid Cowan relationships
  const matBody=document.getElementById('matrix-body');
  matBody.innerHTML='';
  const matRows=[];
  const seenPairs=new Set();

  for(let i=0;i<swings.length;i++){
    for(let j=i+1;j<swings.length;j++){ // j starts at i+1 to avoid full duplicates
      if(swings[i].ptv<=0||swings[j].ptv<=0) continue;

      // BOTH directions
      [[i,j],[j,i]].forEach(([a,b])=>{
        const ratio=swings[a].ptv/swings[b].ptv;
        if(ratio<0.15||ratio>8) return;
        const match=nearestCowanRatio(ratio);
        const devRatioA=devPTV>0?devPTV/swings[a].ptv:null;
        const devMatchA=devRatioA?nearestCowanRatio(devRatioA):null;
        const isGood=match.err<=tol;
        const devGood=devMatchA&&devMatchA.err<=tol;
        if(isGood||devGood||match.err<=12){
          matRows.push({
            a,b,ratio,match,isGood,
            devRatioA,devMatchA,devGood,
            sa:swings[a],sb:swings[b],
            dir: swings[a].ptv>swings[b].ptv ? 'bigdivsmall' : 'smalldivbig'
          });
        }
      });
    }
  }

  // Sort: exact hits first
  matRows.sort((a,b)=>{
    const sA=(a.match.err<2?100:a.match.err<5?50:0)+(a.devGood?30:0)+(a.isGood?10:0);
    const sB=(b.match.err<2?100:b.match.err<5?50:0)+(b.devGood?30:0)+(b.isGood?10:0);
    return sB-sA||a.match.err-b.match.err;
  });

  //  SUMMARY ROW 
  const exactHits=matRows.filter(r=>r.match.err<3&&r.isGood);
  if(exactHits.length){
    const sumRow=document.createElement('tr');
    sumRow.innerHTML='<td colspan="11" style="padding:5px 6px;border-bottom:2px solid rgba(245,166,35,.3);background:rgba(245,166,35,.04)">'
      +'<span style="font-size:10px;font-weight:700;color:#f5a623;margin-right:8px">* EXACT MATCHES:</span>'
      +exactHits.slice(0,6).map(r=>{
        const ac=COLORS[r.a%COLORS.length],bc=COLORS[r.b%COLORS.length];
        return '<span style="font-size:10px;padding:2px 8px;border-radius:20px;margin-right:4px;background:rgba(245,166,35,.12);border:1px solid rgba(245,166,35,.4);color:#f5a623">'
          +'<span style="color:'+ac+'">'+r.sa.label+'('+r.sa.ptv.toFixed(0)+')</span> div '
          +'<span style="color:'+bc+'">'+r.sb.label+'('+r.sb.ptv.toFixed(0)+')</span>'
          +' = <b>'+r.match.rn+'</b>'
          +' <span style="color:#888;font-size:9px">'+r.match.err.toFixed(2)+'%  '+r.dir+'</span></span>';
      }).join('')+'</td>';
    matBody.appendChild(sumRow);
  } else if(!matRows.filter(r=>r.isGood).length){
    const noRow=document.createElement('tr');
    noRow.innerHTML='<td colspan="11" style="color:#444;padding:6px;font-size:12px">No matches within '+tol+'%. Try wider tolerance.</td>';
    matBody.appendChild(noRow);
  }

  //  TABLE ROWS 
  matRows.filter(r=>r.isGood||r.devGood).slice(0,60).forEach(r=>{
    const tr=document.createElement('tr');
    const ac=COLORS[r.a%COLORS.length],bc=COLORS[r.b%COLORS.length];
    const isExact=r.match.err<2,isClose=r.match.err<5;
    if(isExact&&r.devGood) tr.style.cssText='background:rgba(245,166,35,.18);outline:1px solid rgba(245,166,35,.5)';
    else if(isExact) tr.style.cssText='background:rgba(245,166,35,.12);outline:1px solid rgba(245,166,35,.25)';
    else if(r.devGood) tr.style.background='rgba(26,143,96,.1)';
    else if(isClose) tr.style.background='rgba(24,95,165,.06)';

    // Price levels from end point of swing A
    const priceMoveA=showBal
      ?(r.sa.ptv/Math.SQRT2*balDecimalShift)
      :Math.sin((r.sa.ang||45)*Math.PI/180)*r.sa.ptv;
    const endPriceA=r.sa.to?r.sa.to.price:0;
    const lvlUp=endPriceA>0?Math.round(endPriceA+priceMoveA):0;
    const lvlDn=endPriceA>0?Math.round(endPriceA-priceMoveA):0;

    const dirLabel=r.dir==='bigdivsmall'
      ?'<span style="font-size:9px;padding:1px 5px;border-radius:10px;background:rgba(245,166,35,.15);color:#f5a623">BIGdivsmall</span>'
      :'<span style="font-size:9px;padding:1px 5px;border-radius:10px;background:rgba(24,95,165,.2);color:#7ab5e0">smalldivBIG</span>';

    tr.innerHTML=
      '<td><span style="font-size:10px;font-weight:700;padding:1px 7px;border-radius:3px;background:'+ac+'22;color:'+ac+';border:1px solid '+ac+'44">'+r.sa.label+'</span></td>'
      +'<td style="font-weight:700;color:#ccd;font-size:12px">'+r.sa.ptv.toFixed(2)+'</td>'
      +'<td><span style="font-size:10px;font-weight:700;padding:1px 7px;border-radius:3px;background:'+bc+'22;color:'+bc+';border:1px solid '+bc+'44">'+r.sb.label+'</span></td>'
      +'<td style="font-weight:700;color:#ccd;font-size:12px">'+r.sb.ptv.toFixed(2)+'</td>'
      +'<td>'+dirLabel+'</td>'
      +'<td style="font-weight:700;color:'+(r.isGood?'#ccd':'#555')+';font-size:13px">'+r.ratio.toFixed(4)+'</td>'
      +'<td>'+(r.isGood
        ?'<span style="padding:2px 10px;border-radius:20px;background:rgba(245,166,35,.2);color:#f5a623;border:1px solid rgba(245,166,35,.4);font-weight:700;font-size:12px">'+r.match.rn+'</span>'
        :'<span style="color:#444;font-size:11px">'+r.match.rn+'</span>')+'</td>'
      +'<td><span style="font-weight:700;font-size:11px;color:'+(isExact?'#1a8f60':isClose?'#f5a623':'#555')+'">'+r.match.err.toFixed(2)+'%</span></td>'
      +'<td style="font-size:12px">'+(endPriceA>0
        ?'<b style="color:#1a8f60">UP'+lvlUp.toLocaleString('en-IN')+'</b> <span style="color:#333"></span> <b style="color:#c04040">DN'+lvlDn.toLocaleString('en-IN')+'</b>'
        :'')+'</td>'
      +'<td style="font-weight:700;color:'+(r.devGood?'#3a9f50':'#444')+';font-size:11px">'+(r.devRatioA?r.devRatioA.toFixed(4):'')+'</td>'
      +'<td>'+(r.devGood
        ?'<span style="padding:2px 8px;border-radius:20px;background:rgba(26,143,96,.25);color:#1a8f60;border:1px solid rgba(26,143,96,.4);font-weight:700">'+r.devMatchA.rn+' * '+r.devMatchA.err.toFixed(1)+'%</span>'
        :'<span style="color:#333;font-size:10px">'+(r.devMatchA?r.devMatchA.rn+' ('+r.devMatchA.err.toFixed(0)+'%)':'')+'</span>')+'</td>';
    matBody.appendChild(tr);
  });
}

function updateRatioHitBar(){
  const bar=document.getElementById('ratio-hit-bar');
  if(!candles.length){bar.style.display='none';return;}

  const lastCandle=candles.length-1;
  const currentPrice=candles[lastCandle]?.c||0;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;

  // Get swing points  prefer manual
  const manualOnly=manualPts.filter(p=>p.type==='H'||p.type==='L'||p.type==='M');
  const swingPts2=manualOnly.length>=2
    ?[...manualPts].sort((a,b)=>a.idx-b.idx)
    :getActivePts().filter(p=>p.type==='H'||p.type==='L');

  if(swingPts2.length<1){bar.style.display='none';return;}

  // Measure dev PTV from last swing point -> today
  const lastSw=swingPts2[swingPts2.length-1];
  const devDays=lastCandle-lastSw.idx;
  const devPP=Math.abs(currentPrice-lastSw.price);
  const devHrs=devDays*hpd;
  const devPTV=devDays>0?Math.sqrt(devPP*devPP+devHrs*devHrs):0;

  if(devPTV<=0){bar.style.display='none';return;}

  // Build all swings
  const swings2=[];
  for(let i=0;i<swingPts2.length-1;i++){
    const r=calcPTV(swingPts2[i],swingPts2[i+1]);
    if(r.ptv>0) swings2.push({
      ptv:r.ptv,
      label:swingPts2[i].label+'->'+swingPts2[i+1].label
    });
  }

  if(!swings2.length){bar.style.display='none';return;}

  // Find BEST ratio hit  lowest absolute error
  let bestHit=null,bestAbsErr=999;
  swings2.forEach(sw=>{
    const ratio=devPTV/sw.ptv;
    if(ratio<0.1||ratio>10) return;
    const match=nearestCowanRatio(ratio);
    if(match.absErr<bestAbsErr){
      bestAbsErr=match.absErr;
      bestHit={sw,match,ratio,devPTV};
    }
  });

  if(!bestHit){bar.style.display='none';return;}

  const m=bestHit.match;
  const isExact=m.err<2;
  const isGood=m.err<5;
  const isWeak=m.err<10;

  if(!isWeak){bar.style.display='none';return;}

  // Update bar
  bar.style.display='block';
  const col=isExact?'#f5a623':isGood?'#1a8f60':'#185FA5';
  bar.style.borderColor=col;
  bar.style.background=col+'18';
  bar.style.boxShadow=isExact?`0 0 20px ${col}44`:'none';

  const badge=document.getElementById('rh-badge');
  badge.style.background=col;
  badge.style.color=isExact?'#000':'#fff';
  badge.textContent=isExact?'* EXACT HIT':isGood?' RATIO HIT':'~ NEAR RATIO';

  document.getElementById('rh-main').style.color=col;
  document.getElementById('rh-main').textContent=
    `dev PTV ${devPTV.toFixed(2)}  /  ${bestHit.sw.label} (${bestHit.sw.ptv.toFixed(2)})  =  ${bestHit.ratio.toFixed(4)}  ~=  ${m.rn}`;

  document.getElementById('rh-detail').textContent=
    `Error: ${m.err.toFixed(3)}%    Abs diff: ${m.absErr.toFixed(4)}    ${devDays}d developing    from ${Math.round(lastSw.price).toLocaleString('en-IN')}`;

  // Direction signal
  const isBuy=currentPrice<lastSw.price; // coming down = buy expected
  const sigEl=document.getElementById('rh-signal');
  sigEl.textContent=isGood?(isBuy?' Watch for BUY reversal':' Watch for SELL reversal'):'';
  sigEl.style.color=isBuy?'#1a8f60':'#c04040';
}

function updateWatchPanel(){ updateTradePanel(); }

// 
// 4D PTV MULTI-CONFLUENCE ENGINE
// Every swing PTV x every Cowan ratio = projection line
// Find where MULTIPLE lines from DIFFERENT swings hit same zone
// More lines = stronger signal (like your 3059x0.5 + 996xsqrt3 chart)
// 
function updateConf4D(){
  const pts=getActivePts().filter(p=>p.type==='H'||p.type==='L');
  const dtol=parseFloat(document.getElementById('c4-dtol').value)||5;
  const ptol=parseFloat(document.getElementById('c4-ptol').value)||1;
  const minLines=parseInt(document.getElementById('c4-minlines').value)||3;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const lastCandle=candles.length-1;
  const lastDate=candles[lastCandle]?.t;
  const currentPrice=candles[lastCandle]?.c||0;

  const ALL_RATS=[
    [0.25,'0.25'],[0.382,'0.382'],[0.5,'0.5'],[0.618,'0.618'],[0.707,'1/sqrt2'],
    [1,'1x'],[1.272,'1.272'],[1.382,'1.382'],[1.414,'sqrt2'],[1.5,'1.5'],
    [1.618,'PHI'],[1.732,'sqrt3'],[2,'2x'],[2.236,'sqrt5'],[2.618,'PHI'],
    [3,'3x'],[3.14159,'\u03c0'],[4,'4x']
  ];

  if(pts.length<2){
    document.getElementById('c4-cards').innerHTML='<p style="color:#444;font-size:12px;padding:6px">Need 2+ swing points.</p>';
    document.getElementById('c4-count').textContent='0 zones';
    document.getElementById('c4-near-badge').style.display='none';
    return;
  }

  //  GENERATE ALL PROJECTION LINES 
  // From every consecutive swing pair
  const allLines=[];
  for(let i=0;i<pts.length-1;i++){
    const p1=pts[i],p2=pts[i+1];
    const{ptv,ang,dayDiff}=calcPTV(p1,p2);
    if(ptv<=0) continue;
    const swingLabel=p1.label+'->'+p2.label;
    const swingColor=COLORS[i%COLORS.length];

    ALL_RATS.forEach(([rv,rn])=>{
      const np=ptv*rv;
      let daysFwd,upP,dnP;
      if(showBal){
        const comp=np/Math.SQRT2;
        const sf=balScaleFactor||1;
        const pwb=p2.price>=p1.price;
        daysFwd=pwb?comp/sf:comp;
        const rpm=pwb?comp*balDecimalShift:comp/sf;
        upP=p2.price+rpm; dnP=p2.price-rpm;
      } else {
        const nH=Math.cos(ang*Math.PI/180)*np;
        const nP=Math.sin(ang*Math.PI/180)*np;
        daysFwd=nH/hpd;
        upP=pf(p2.price,nP,1); dnP=pf(p2.price,nP,-1);
      }
      const targetDayIdx=p2.idx+daysFwd;
      const daysFromNow=targetDayIdx-lastCandle;
      if(daysFromNow<-10||daysFromNow>180) return; // skip distant past/future
      let estDate='';
      if(lastDate&&daysFromNow>0){
        const ms=lastDate.getTime()+daysFromNow*24*60*60*1000;
        estDate=fmDate(new Date(ms));
      }
      [upP,dnP].forEach((price,di)=>{
        if(price<=0) return;
        allLines.push({
          swingLabel,swingIdx:i,swingColor,
          rv,rn,np,daysFromNow,targetDayIdx,
          price,estDate,dir:di===0?'UP':'DN',
          isFuture:daysFromNow>-5
        });
      });
    });
  }

  document.getElementById('c4-total').textContent=allLines.length;

  //  CLUSTER LINES: same date (dtol days) + same price (ptol%) 
  const futurLines=allLines.filter(l=>l.isFuture);
  const zones=[];
  futurLines.forEach(l=>{
    let matched=false;
    for(const z of zones){
      const dd=Math.abs(z.avgDays-l.daysFromNow);
      const pd2=Math.abs(z.avgPrice-l.price)/Math.max(z.avgPrice,1)*100;
      if(dd<=dtol&&pd2<=ptol){
        z.lines.push(l);
        z.avgDays=(z.avgDays*(z.lines.length-1)+l.daysFromNow)/z.lines.length;
        z.avgPrice=(z.avgPrice*(z.lines.length-1)+l.price)/z.lines.length;
        // Track unique swings contributing
        const uSwings=new Set(z.lines.map(x=>x.swingLabel));
        const uRatios=new Set(z.lines.map(x=>x.rn));
        z.uniqueSwings=uSwings.size;
        z.uniqueRatios=uRatios.size;
        // Score: more unique swings = exponentially stronger
        // Same as Cowan: 3 swings converging > 5 lines from 1 swing
        z.score=z.lines.length + z.uniqueSwings*4 + z.uniqueRatios*2;
        matched=true; break;
      }
    }
    if(!matched) zones.push({
      lines:[l],avgDays:l.daysFromNow,avgPrice:l.price,
      uniqueSwings:1,uniqueRatios:1,score:1
    });
  });

  // Filter by minLines and sort by score
  const goodZones=zones.filter(z=>z.lines.length>=minLines)
    .sort((a,b)=>b.score-a.score);

  document.getElementById('c4-count').textContent=goodZones.length+' zones';

  // Near CMP check
  const nearZones=goodZones.filter(z=>Math.abs(z.avgPrice-currentPrice)/currentPrice*100<3);
  document.getElementById('c4-near-badge').style.display=nearZones.length?'inline':'none';

  //  ZONE CARDS 
  const cardsDiv=document.getElementById('c4-cards');
  cardsDiv.innerHTML='';

  if(!goodZones.length){
    cardsDiv.innerHTML=`<div style="color:#444;font-size:12px;padding:8px;grid-column:1/-1">
      No zones with ${minLines}+ lines converging. Try: wider date/price tolerance, or reduce Min lines to 2.
    </div>`;
  }

  goodZones.slice(0,9).forEach(z=>{
    const isStrong=z.uniqueSwings>=3||z.score>=12;
    const isMed=z.uniqueSwings>=2||z.score>=7;
    const isNear=Math.abs(z.avgPrice-currentPrice)/currentPrice*100<3;
    const borderCol=isStrong?'#f5a623':isMed?'#185FA5':'#252840';
    const bgCol=isStrong?'rgba(245,166,35,.08)':isMed?'rgba(24,95,165,.08)':'rgba(255,255,255,.02)';

    let estDate='';
    if(lastDate&&z.avgDays>0){const ms=lastDate.getTime()+z.avgDays*24*60*60*1000;estDate=fmDate(new Date(ms));}

    // Unique swings in this zone
    const uSwings=[...new Set(z.lines.map(l=>l.swingLabel))];
    const uRatios=[...new Set(z.lines.map(l=>l.rn))];
    const distPct=(Math.abs(z.avgPrice-currentPrice)/currentPrice*100).toFixed(1);
    const isAbove=z.avgPrice>currentPrice;

    const card=document.createElement('div');
    card.style.cssText=`background:${bgCol};border:2px solid ${borderCol};border-radius:10px;padding:10px 12px;position:relative;${isNear?'box-shadow:0 0 14px '+borderCol+'55':''}`;
    card.innerHTML=`
      ${isNear?`<div style="position:absolute;top:-1px;right:8px;font-size:10px;font-weight:700;padding:2px 8px;border-radius:0 0 6px 6px;background:#f5a623;color:#000"> NEAR CMP ${distPct}%</div>`:''}
      <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:6px;margin-top:${isNear?'8px':'0'}">
        <div>
          <span style="font-size:10px;font-weight:700;padding:2px 8px;border-radius:20px;background:${borderCol}22;color:${borderCol};border:1px solid ${borderCol}55">
            ${isStrong?'* STRONG':isMed?' MEDIUM':' WATCH'}
          </span>
          <span style="font-size:10px;color:#444;margin-left:6px">${z.lines.length} lines  ${z.uniqueSwings} swings  ${z.uniqueRatios} ratios</span>
        </div>
        <span style="font-size:11px;font-weight:700;color:#f5a623">score ${z.score}</span>
      </div>
      <div style="font-size:14px;font-weight:700;color:#ccd;margin-bottom:3px">
        ${isAbove?'UP':'DN'} <span style="color:${isAbove?'#1a8f60':'#c04040'}">${Math.round(z.avgPrice).toLocaleString('en-IN')}</span>
        <span style="font-size:11px;color:#555;font-weight:400;margin-left:6px">${distPct}% ${isAbove?'above':'below'} CMP</span>
      </div>
      <div style="font-size:12px;color:#666;margin-bottom:6px">
        Day +${z.avgDays.toFixed(0)} &nbsp;&nbsp; ${estDate}
      </div>
      <!-- Which swings contribute -- the key Cowan info -->
      <div style="margin-bottom:4px;font-size:10px;color:#444;font-weight:700;text-transform:uppercase">Contributing swings:</div>
      <div style="display:flex;flex-wrap:wrap;gap:3px;margin-bottom:4px">
        ${uSwings.map((s,si)=>{
          const col=COLORS[z.lines.find(l=>l.swingLabel===s)?.swingIdx%COLORS.length]||'#555';
          const ratiosForSwing=[...new Set(z.lines.filter(l=>l.swingLabel===s).map(l=>l.rn))].join(',');
          return `<span style="font-size:10px;font-weight:700;padding:2px 8px;border-radius:20px;background:${col}22;color:${col};border:1px solid ${col}44">${s} x${ratiosForSwing}</span>`;
        }).join('')}
      </div>
      <div style="font-size:10px;color:#333">
        Ratios: ${uRatios.slice(0,8).join('  ')}${uRatios.length>8?'':''}
      </div>`;
    cardsDiv.appendChild(card);
  });

  //  ALL LINES TABLE 
  const tbody=document.getElementById('c4-body');tbody.innerHTML='';
  // Sort by convergence score desc then days asc
  const linesSorted=[...futurLines].sort((a,b)=>{
    const za=zones.find(z=>z.lines.includes(a));
    const zb=zones.find(z=>z.lines.includes(b));
    const sa=za?za.score:0, sb=zb?zb.score:0;
    return sb-sa||a.daysFromNow-b.daysFromNow;
  });
  linesSorted.slice(0,100).forEach(l=>{
    const zone=zones.find(z=>z.lines.includes(l));
    const zScore=zone?zone.score:1;
    const tr=document.createElement('tr');
    const isGood=zone&&zone.lines.length>=minLines;
    if(isGood) tr.style.background=zScore>=12?'rgba(245,166,35,.07)':zScore>=7?'rgba(24,95,165,.05)':'';
    const sc=zScore>=12?'#f5a623':zScore>=7?'#185FA5':'#333';
    tr.innerHTML=`
      <td><span style="font-size:10px;font-weight:700;padding:1px 6px;border-radius:3px;background:${l.swingColor}22;color:${l.swingColor};border:1px solid ${l.swingColor}44">${l.swingLabel}</span></td>
      <td style="font-weight:700;color:#888">${l.rn}</td>
      <td class="hi">${l.np.toFixed(2)}</td>
      <td style="font-size:11px">${l.daysFromNow.toFixed(0)}d</td>
      <td style="font-size:11px;color:#444">${l.estDate}</td>
      <td style="${l.dir==='UP'?'color:#1a8f60':'color:#c04040'};font-weight:700">${l.dir}${Math.round(l.price).toLocaleString('en-IN')}</td>
      <td style="font-size:10px;color:#555">${isGood?zone.lines.length+' lines':'-'}</td>
      <td><span style="font-size:10px;padding:1px 6px;border-radius:3px;background:${sc}22;color:${sc};border:1px solid ${sc}44">${isGood?'score '+zScore:'single'}</span></td>`;
    tbody.appendChild(tr);
  });
}

function updateSignalsPanel(){
  const signals=computeSignals();
  const currentPrice=candles.length?candles[candles.length-1].c:0;
  const lastDate=candles.length?candles[candles.length-1].t:null;
  const lastCandle=candles.length-1;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;

  // Always update HH/LL table
  const{hh,ll,hl,lh}=computeHHLLVectors();
  const tb=document.getElementById('hhll-body');tb.innerHTML='';
  // group same-type vectors to show ratio between consecutive same-type
  const byType={HH:hh,LL:ll,HL:hl,LH:lh};
  Object.entries(byType).forEach(([type,vecs])=>{
    vecs.forEach((vec,i)=>{
      const col=type==='HH'?'#f5a623':type==='LL'?'#185FA5':type==='LH'?'#1a8f60':'#c04040';
      const prevSame=i>0?vecs[i-1]:null;
      const ratio=prevSame?(vec.ptv/prevSame.ptv).toFixed(3)+' ~='+nearestRatioLabel(vec.ptv/prevSame.ptv):'';
      const tr=document.createElement('tr');
      tr.innerHTML=`<td><span style="font-size:10px;font-weight:700;padding:1px 6px;border-radius:20px;background:${col}22;color:${col};border:1px solid ${col}44">${type}</span></td>
        <td style="font-weight:700;color:${col};font-size:11px">${vec.label}</td>
        <td class="hi">${vec.ptv.toFixed(2)}</td>
        <td style="font-size:11px">${vec.ang.toFixed(1)}deg</td>
        <td class="hi">${ratio}</td>`;
      tb.appendChild(tr);
    });
  });

  // Signal cards
  const panel=document.getElementById('sig-cards');panel.innerHTML='';
  document.getElementById('sig-count').textContent=signals.length+' signals';

  if(!signals.length||!candles.length){
    panel.innerHTML=`<div style="font-size:12px;color:#555;padding:8px;grid-column:1/-1">
      ${!candles.length?'Load chart data first.':'No signals generated. Try: 1) Load more data (6M or 1Y range) 2) Reduce Swing % to get more swings detected 3) Check HH/LL table above has entries'}
    </div>`;
    document.getElementById('sig-near-badge').style.display='none';
    return;
  }

  let anyNear=false;
  signals.slice(0,12).forEach(sig=>{
    const isBuy=sig.signalType==='BUY';
    const strength=sig.score>=8?'STRONG':sig.score>=5?'MODERATE':'WATCH';
    const col=isBuy?'#1a8f60':'#c04040';
    const distPct=Math.abs(sig.avgPrice-currentPrice)/Math.max(currentPrice,1)*100;
    const isNear=distPct<3&&sig.avgDays>-5;
    if(isNear) anyNear=true;
    let estDate='';
    if(lastDate&&sig.avgDays>0){const ms=lastDate.getTime()+sig.avgDays*24*60*60*1000;estDate=fmDate(new Date(ms));}
    const vtypes=[...new Set(sig.items.map(x=>x.vecType))];
    const vlabels=[...new Set(sig.items.map(x=>x.vecLabel))];
    const card=document.createElement('div');
    card.className='scard';
    card.style.cssText=`border-color:${col}${isNear?'':'66'};background:${col}0d;${isNear?'box-shadow:0 0 16px '+col+'44':''}`;
    card.innerHTML=`
      <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:6px">
        <div style="font-size:14px;font-weight:700;color:${col}">${isBuy?' BUY ZONE':' SELL ZONE'}</div>
        <div style="display:flex;gap:4px;align-items:center">
          ${isNear?`<span style="font-size:10px;font-weight:700;padding:2px 8px;border-radius:20px;background:#f5a62333;color:#f5a623;border:1px solid #f5a62366"> NEAR CMP</span>`:''}
          <span style="font-size:11px;font-weight:700;padding:3px 10px;border-radius:20px;background:${col}22;color:${col};border:1px solid ${col}55">${strength}</span>
        </div>
      </div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:4px;font-size:12px;margin-bottom:6px">
        <div>Price: <b style="color:${col};font-size:14px">${Math.round(sig.avgPrice).toLocaleString('en-IN')}</b></div>
        <div>Days: <b>+${Math.round(sig.avgDays)}d</b> ${estDate?'<span style="font-size:10px;color:#555">('+estDate+')</span>':''}</div>
        <div>${distPct.toFixed(1)}% ${sig.avgPrice>currentPrice?'above':'below'} CMP</div>
        <div>Score: <b style="color:${col}">${sig.score}</b>  ${sig.uniqueVecs} vectors</div>
      </div>
      <div style="margin-bottom:5px">Types: ${vtypes.map(t=>`<span style="font-size:10px;font-weight:700;padding:2px 7px;border-radius:3px;background:${col}22;color:${col};border:1px solid ${col}44;margin-right:2px">${t}</span>`).join('')}</div>
      <div style="display:flex;flex-wrap:wrap;gap:2px;margin-bottom:5px">${vlabels.map(l=>`<span style="font-size:10px;padding:1px 6px;border-radius:3px;background:#252840;color:#aaa">${l}</span>`).join('')}</div>
      <div style="font-size:11px;color:#555">${isBuy?'LL/LH vectors project support/LOW here':'HH/HL vectors project resistance/HIGH here'}${vtypes.length>1?'  multiple vector types agree = STRONGER signal':''}</div>`;
    panel.appendChild(card);
  });
  document.getElementById('sig-near-badge').style.display=anyNear?'inline':'none';
}

// 
// BACKTEST  PTV RATIO BASED (not price, not time)
// Logic: for each consecutive swing pair, measure actual ratio
// of PTV(B) div PTV(A). Check if it matches any Cowan ratio
// within tolerance. That's a HIT. No price/time prediction needed.
// Example: PTV=54, next PTV=108 -> ratio=2.0 -> HIT 
// 
// 
// VIRTUAL MONEY BACKTEST
// Walk every candle. At each candle, check if developing PTV
// from last detected swing div any completed swing = Cowan ratio.
// When yes -> enter trade. Track with virtual money.
// 
function runBacktest(){
  if(!candles.length||candles.length<20){
    alert('Load chart data first (6M or 1Y for meaningful results).');return;
  }

  const rTol=parseFloat(document.getElementById('bt-rtol').value)||3;
  const slPct=parseFloat(document.getElementById('bt-sl').value)||1;
  const tpPct=parseFloat(document.getElementById('bt-tp').value)||3;
  const capital=parseFloat(document.getElementById('bt-cap').value)||100000;
  const direction=document.getElementById('bt-dir').value;
  const ratQuality=document.getElementById('bt-minq').value;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const btBody=document.getElementById('bt-body');btBody.innerHTML='';

  // Key ratios only set
  const KEY_ONLY_RATS=new Set([0.5,0.618,1,1.414,1.618,1.732,2,2.236,3.14159]);

  //  DETECT SWINGS walk-forward 
  // Use auto-detected swings. For each candle, know which swings
  // have completed UP TO that candle.
  const swingPct=parseFloat(document.getElementById('swpct').value)||3;

  // Detect ALL swings first
  const allSwings=autoSwings.length?autoSwings:detectSwings();
  if(allSwings.length<3){
    alert('Need at least 3 auto-detected swings. Reduce Swing % or load more data.');return;
  }

  const trades=[];
  let runningPnL=0,equityPts=[capital];
  let maxEquity=capital,maxDD=0;
  let openTrade=null; // one trade at a time
  const seenSignals=new Set(); // dedup: same ratio+swing within 3 candles

  // Walk through every candle
  for(let ci=1;ci<candles.length;ci++){
    const candle=candles[ci];

    //  Check open trade exit 
    if(openTrade){
      const isBuy=openTrade.dir==='BUY';
      let exited=false,exitPrice=0,exitReason='';

      if(isBuy){
        if(candle.l<=openTrade.sl){exitPrice=openTrade.sl;exitReason='SL';exited=true;}
        else if(candle.h>=openTrade.tp){exitPrice=openTrade.tp;exitReason='TP';exited=true;}
      } else {
        if(candle.h>=openTrade.sl){exitPrice=openTrade.sl;exitReason='SL';exited=true;}
        else if(candle.l<=openTrade.tp){exitPrice=openTrade.tp;exitReason='TP';exited=true;}
      }

      if(exited){
        const priceMov=isBuy?exitPrice-openTrade.entry:openTrade.entry-exitPrice;
        const pnl=Math.round((priceMov/openTrade.entry)*capital);
        runningPnL+=pnl;
        equityPts.push(capital+runningPnL);
        maxEquity=Math.max(maxEquity,capital+runningPnL);
        maxDD=Math.max(maxDD,maxEquity-(capital+runningPnL));

        trades.push({...openTrade,exitPrice,exitReason,pnl,runningTotal:capital+runningPnL,
          exitDate:candle.t,result:exitReason==='TP'?'WIN':'LOSS'});
        openTrade=null;
      }
      if(openTrade) continue; // still in trade, skip signal check
    }

    //  Check for new signal 
    // Find which swings are completed UP TO candle ci
    const completedSwings=[];
    for(let si=0;si<allSwings.length-1;si++){
      if(allSwings[si+1].idx<=ci){
        const r=calcPTV(allSwings[si],allSwings[si+1]);
        if(r.ptv>0) completedSwings.push({
          ptv:r.ptv,ang:r.ang,
          label:allSwings[si].label+'->'+allSwings[si+1].label,
          endIdx:allSwings[si+1].idx,
          endType:allSwings[si+1].type
        });
      }
    }
    if(completedSwings.length<1) continue;

    // Last completed swing endpoint
    const lastSw=allSwings.filter(s=>s.idx<=ci).slice(-1)[0];
    if(!lastSw) continue;

    // Developing PTV: from lastSw.idx to ci
    const devDays=ci-lastSw.idx;
    if(devDays<2) continue; // need at least 2 days
    const devPP=Math.abs(candle.c-lastSw.price);
    const devHrs=devDays*hpd;
    const devPTV=Math.sqrt(devPP*devPP+devHrs*devHrs);
    if(devPTV<=0) continue;

    // Check dev PTV div any completed swing = Cowan ratio
    let bestHit=null,bestAbsErr=999;
    completedSwings.forEach(sw=>{
      const ratio=devPTV/sw.ptv;
      if(ratio<0.15||ratio>8) return;
      const match=nearestCowanRatio(ratio);
      // Filter by quality
      if(ratQuality==='key'&&!KEY_ONLY_RATS.has(match.rv)) return;
      if(match.err<=rTol&&match.absErr<bestAbsErr){
        bestAbsErr=match.absErr;
        bestHit={sw,match,ratio,devPTV};
      }
    });

    if(!bestHit) continue;

    // Dedup: same signal within 5 candles
    const sigKey=`${bestHit.match.rn}_${bestHit.sw.label}_${Math.round(ci/5)*5}`;
    if(seenSignals.has(sigKey)) continue;
    seenSignals.add(sigKey);

    // Determine direction: if last swing endpoint was a HIGH -> expect down -> SELL
    // if last swing endpoint was a LOW -> expect up -> BUY
    const sigDir=lastSw.type==='H'?'SELL':'BUY';

    // Filter by direction setting
    if(direction==='buy'&&sigDir!=='BUY') continue;
    if(direction==='sell'&&sigDir!=='SELL') continue;

    const entryPrice=candle.c;
    const isBuy=sigDir==='BUY';
    const sl=isBuy?entryPrice*(1-slPct/100):entryPrice*(1+slPct/100);
    const tp=isBuy?entryPrice*(1+tpPct/100):entryPrice*(1-tpPct/100);

    openTrade={
      dir:sigDir,entry:entryPrice,sl,tp,
      ratio:bestHit.match.rn,ratioErr:bestHit.match.err,
      swingLabel:bestHit.sw.label,swingPTV:bestHit.sw.ptv,
      devPTV:bestHit.devPTV,
      entryDate:candle.t,entryBar:ci
    };
  }

  // Close any open trade at last candle
  if(openTrade){
    const lastC=candles[candles.length-1];
    const exitPrice=lastC.c;
    const isBuy=openTrade.dir==='BUY';
    const priceMov=isBuy?exitPrice-openTrade.entry:openTrade.entry-exitPrice;
    const pnl=Math.round((priceMov/openTrade.entry)*capital);
    runningPnL+=pnl;
    equityPts.push(capital+runningPnL);
    trades.push({...openTrade,exitPrice,exitReason:'OPEN',pnl,
      runningTotal:capital+runningPnL,exitDate:lastC.t,result:'OPEN'});
  }

  if(!trades.length){
    document.getElementById('bt-stats').style.display='none';
    document.getElementById('bt-count').textContent='0 trades  try 5% or 8% tolerance';
    btBody.innerHTML='<tr><td colspan="12" style="color:#444;padding:8px">No signals generated. Try: wider tolerance (5%), All ratios, longer date range (1Y or 2Y).</td></tr>';
    return;
  }

  //  STATS 
  const wins=trades.filter(t=>t.result==='WIN').length;
  const losses=trades.filter(t=>t.result==='LOSS').length;
  const open=trades.filter(t=>t.result==='OPEN').length;
  const tested=wins+losses;
  const wr=tested>0?(wins/tested*100).toFixed(1):'';
  const netPnL=runningPnL;
  const netPct=(netPnL/capital*100).toFixed(2);
  const avgWin=wins>0?trades.filter(t=>t.result==='WIN').reduce((s,t)=>s+t.pnl,0)/wins:0;
  const avgLoss=losses>0?Math.abs(trades.filter(t=>t.result==='LOSS').reduce((s,t)=>s+t.pnl,0)/losses):0;
  const expectancy=tested>0?((wins/tested)*avgWin-(losses/tested)*avgLoss).toFixed(0):0;

  // Best ratio by win rate
  const byRatio={};
  trades.filter(t=>t.result!=='OPEN').forEach(t=>{
    if(!byRatio[t.ratio]) byRatio[t.ratio]={w:0,t:0};
    byRatio[t.ratio].t++;if(t.result==='WIN')byRatio[t.ratio].w++;
  });
  const bestRat=Object.entries(byRatio).sort((a,b)=>b[1].w/b[1].t-a[1].w/a[1].t)[0];

  document.getElementById('bt-stats').style.display='block';
  document.getElementById('bt-wr').textContent=wr+'%';
  document.getElementById('bt-wr-sub').textContent=wins+'W / '+losses+'L';
  document.getElementById('bt-wins').textContent=wins;
  document.getElementById('bt-losses').textContent=losses;
  document.getElementById('bt-total').textContent=trades.length;
  document.getElementById('bt-pend-lbl').textContent=open+' still open';
  const pnlEl=document.getElementById('bt-pnl');
  pnlEl.textContent=(netPnL>=0?'+':'')+netPnL.toLocaleString('en-IN')+' ('+netPct+'%)';
  pnlEl.style.color=netPnL>=0?'#1a8f60':'#e05050';
  document.getElementById('bt-best').textContent=bestRat?bestRat[0]+' '+(bestRat[1].w/bestRat[1].t*100).toFixed(0)+'%':'';
  document.getElementById('bt-rr').textContent=''+Math.round(expectancy).toLocaleString('en-IN')+'/trade';
  document.getElementById('bt-dd').textContent=''+Math.round(maxDD).toLocaleString('en-IN');
  document.getElementById('bt-count').textContent=`${wr}% win rate  ${trades.length} trades  ${netPnL.toLocaleString('en-IN')} P&L`;

  // By-ratio breakdown
  const ratStr=Object.entries(byRatio).sort((a,b)=>b[1].t-a[1].t)
    .map(([k,v])=>`<b>${k}</b>: ${(v.w/v.t*100).toFixed(0)}% (${v.w}/${v.t})`).join(' &nbsp;&nbsp; ');
  document.getElementById('bt-analysis').innerHTML=
    `<b style="color:#f5a623">Win rate by ratio:</b> ${ratStr}<br>
     <b style="color:#f5a623">Settings:</b> SL ${slPct}%  TP ${tpPct}%  Tol ${rTol}%  Capital ${capital.toLocaleString('en-IN')}  ${direction}  ${ratQuality} ratios<br>
     <b style="color:#f5a623">Avg win:</b> ${Math.round(avgWin).toLocaleString('en-IN')} &nbsp;&nbsp;
     <b style="color:#f5a623">Avg loss:</b> ${Math.round(avgLoss).toLocaleString('en-IN')} &nbsp;&nbsp;
     <b style="color:#f5a623">R:R actual:</b> ${avgLoss>0?(avgWin/avgLoss).toFixed(2):''}`;

  //  EQUITY CURVE 
  const ec=document.getElementById('bt-equity-canvas');
  ec.width=ec.offsetWidth*devicePixelRatio||800;
  const ectx=ec.getContext('2d');ectx.scale(devicePixelRatio,1);
  const W2=ec.offsetWidth||800,H2=70;
  ectx.fillStyle='#f8f9fc';ectx.fillRect(0,0,W2,H2);
  const minEq=Math.min(...equityPts),maxEq=Math.max(...equityPts);
  const eRange=Math.max(maxEq-minEq,1);
  // Zero line
  const zeroY=H2-((capital-minEq)/eRange*(H2-10))-5;
  ectx.strokeStyle='rgba(0,0,0,.12)';ectx.lineWidth=1;
  ectx.beginPath();ectx.moveTo(0,zeroY);ectx.lineTo(W2,zeroY);ectx.stroke();
  // Equity line
  ectx.beginPath();
  equityPts.forEach((v,i)=>{
    const x=i/(equityPts.length-1)*W2;
    const y=H2-((v-minEq)/eRange*(H2-10))-5;
    i===0?ectx.moveTo(x,y):ectx.lineTo(x,y);
  });
  ectx.strokeStyle=netPnL>=0?'#1a8f60':'#e05050';ectx.lineWidth=2;ectx.stroke();
  // Fill under curve
  ectx.lineTo(W2,H2);ectx.lineTo(0,H2);ectx.closePath();
  ectx.fillStyle=(netPnL>=0?'rgba(26,143,96,':'rgba(224,80,80,')+'.12)';ectx.fill();
  // Labels
  ectx.fillStyle='#444';ectx.font='9px sans-serif';ectx.textAlign='left';
  ectx.fillText(''+Math.round(maxEq).toLocaleString('en-IN'),4,12);
  ectx.textAlign='right';
  ectx.fillText(''+Math.round(minEq).toLocaleString('en-IN'),W2-4,H2-4);

  //  TRADE LOG TABLE 
  trades.forEach((t,ti)=>{
    const tr=document.createElement('tr');
    const rcol=t.result==='WIN'?'#1a8f60':t.result==='LOSS'?'#e05050':t.result==='OPEN'?'#6b9fd5':'#555';
    const dcol=t.dir==='BUY'?'#1a8f60':'#c04040';
    const pnlCol=t.pnl>=0?'#1a8f60':'#e05050';
    tr.innerHTML=`
      <td style="font-size:11px;color:#555">${ti+1}</td>
      <td style="font-size:11px">${fmDate(t.entryDate)}</td>
      <td><span style="font-size:10px;font-weight:700;padding:1px 7px;border-radius:20px;background:${dcol}22;color:${dcol};border:1px solid ${dcol}44">${t.dir}</span></td>
      <td style="font-weight:700;color:#f5a623">${t.ratio}</td>
      <td style="font-size:11px;color:${t.ratioErr<2?'#1a8f60':t.ratioErr<5?'#f5a623':'#555'}">${t.ratioErr.toFixed(2)}%</td>
      <td style="font-weight:700">${Math.round(t.entry).toLocaleString('en-IN')}</td>
      <td style="color:#e05050">${Math.round(t.sl).toLocaleString('en-IN')}</td>
      <td style="color:#1a8f60">${Math.round(t.tp).toLocaleString('en-IN')}</td>
      <td style="font-size:11px;color:#666">${Math.round(t.exitPrice).toLocaleString('en-IN')}</td>
      <td><b style="color:${rcol}">${t.result}</b></td>
      <td style="font-weight:700;color:${pnlCol}">${t.pnl>=0?'+':''}${t.pnl.toLocaleString('en-IN')}</td>
      <td style="font-size:11px;color:${t.runningTotal>=capital?'#1a8f60':'#e05050'}">${t.runningTotal.toLocaleString('en-IN')}</td>`;
    btBody.appendChild(tr);
  });
}

// 
// LEVELS
// 
function nearestRatioLabel(r){
  let best=RATS[0],bd=99;
  RATS.forEach(([v,n])=>{const d=Math.abs(r-v);if(d<bd){bd=d;best=[v,n];}});
  return best[1];
}
function computeLevels(){
  const lt=getLastTwo();if(!lt) return[];
  const{ptv,last}=lt;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const lastCandle=candles.length-1;
  const levels=[];

  RATS.forEach(([rv,rn])=>{
    [['m','x'],['d','div']].forEach(([op,sym])=>{
      if(rv===1&&op==='d') return;
      const np=op==='m'?ptv.ptv*rv:ptv.ptv/rv;

      let daysFwd,upP,dnP;

      if(showBal){
        const comp=np/Math.SQRT2;
        const sf=ptv.effSF||ptv.scaleFactor||1;
        const priceWasBigger=ptv.priceWasBigger!==false;
        daysFwd=priceWasBigger ? comp/sf : comp;
        const realPriceMov=priceWasBigger ? comp*balDecimalShift : comp/sf;
        upP=last.price+realPriceMov;
        dnP=last.price-realPriceMov;
      } else {
        // Original method
        const nH=Math.cos(ptv.ang*Math.PI/180)*np;
        const nP=Math.sin(ptv.ang*Math.PI/180)*np;
        daysFwd=nH/hpd;
        upP=pf(last.price,nP,1);
        dnP=pf(last.price,nP,-1);
      }

      const targetDayIdx=last.idx+daysFwd;
      const daysFromNow=targetDayIdx-lastCandle;
      const isKey=KEY_RATS.includes(rv);
      const strength=isKey?(rv===1||rv===2||rv===1.618||rv===0.618?'strong':'medium'):'weak';
      levels.push({sym,rn,rv,np,daysFwd,targetDayIdx,daysFromNow,upP,dnP,strength,label:sym+rn,future:daysFromNow>0});
    });
  });
  levels.sort((a,b)=>a.daysFwd-b.daysFwd);
  return levels;
}
function findNearLevels(cp,levels,pct=3){return levels.filter(l=>Math.abs(l.upP-cp)/cp*100<pct||Math.abs(l.dnP-cp)/cp*100<pct);}

// 
// BALANCED 45deg  completely separate
// 
function calcBal(){
  const rawP=parseFloat(document.getElementById('bal-p').value)||0;
  const rawT=parseFloat(document.getElementById('bal-t').value)||1;
  const startP=parseFloat(document.getElementById('bal-sp').value)||1;
  const priceIsBigger=rawP>=rawT;
  const bigger=priceIsBigger?rawP:rawT,smaller=priceIsBigger?rawT:rawP;
  balRatioFactor=bigger/Math.max(smaller,.001); // raw SF
  const effSF=balRatioFactor/balDecimalShift;   // effective SF after shift
  balScaleFactor=effSF; // update global for chart scaling

  // Apply decimal shift
  const bP=priceIsBigger ? rawP/balDecimalShift : rawP*effSF;
  const bT=priceIsBigger ? rawT*effSF           : rawT/balDecimalShift;
  balComponent=bP;
  balPTV=Math.sqrt(bP*bP+bT*bT);
  const ang=Math.atan2(bP,bT)*180/Math.PI;

  document.getElementById('bal-ratio').textContent=balRatioFactor.toFixed(4);
  document.getElementById('bal-ratio-sub').textContent=(priceIsBigger?rawP+' div '+rawT:rawT+' div '+rawP)+' = '+balRatioFactor.toFixed(4);
  document.getElementById('bal-eff-sf').textContent=effSF.toFixed(6);
  document.getElementById('bal-eff-sf-sub').textContent=balRatioFactor.toFixed(2)+' div '+balDecimalShift;
  document.getElementById('bal-bp').textContent=bP.toFixed(6);
  document.getElementById('bal-bt').textContent=bT.toFixed(6)+'d';
  document.getElementById('bal-ptv').textContent=balPTV.toFixed(6);
  document.getElementById('bal-ptv-sub').textContent=bP.toFixed(4)+' x sqrt2 = '+(bP*Math.SQRT2).toFixed(6);
  document.getElementById('bal-ang').textContent=ang.toFixed(2)+'deg';

  // Explain panel
  const raw=balRatioFactor;
  const explainEl=document.getElementById('bal-shift-explain');
  if(explainEl) explainEl.innerHTML=
    `Raw SF = <b style="color:#3a9f50">${raw.toFixed(2)}</b> -> Raw PTV = <b>${(bigger*Math.SQRT2).toFixed(1)}</b>
     &nbsp;|&nbsp; /10 -> PTV~=<b>${(bigger/10*Math.SQRT2).toFixed(1)}</b>
     &nbsp;|&nbsp; /100 -> PTV~=<b style="color:#f5a623">${(bigger/100*Math.SQRT2).toFixed(2)}</b>
     &nbsp;|&nbsp; /1000 -> PTV~=<b>${(bigger/1000*Math.SQRT2).toFixed(3)}</b><br>
     <b style="color:#ccd">Active:</b> /${balDecimalShift} -> Eff SF=<b style="color:#f5a623">${effSF.toFixed(4)}</b>
     &nbsp;->&nbsp; Balanced component = <b style="color:#1a8f60">${bP.toFixed(4)}</b>
     &nbsp;->&nbsp; PTV = <b style="color:#f5a623">${balPTV.toFixed(4)}</b>`;

  document.getElementById('bal-steps').innerHTML=`
    <div class="bal-step"><b>Step 1:</b> ${priceIsBigger?'Price':'Time'} is bigger = <b>${bigger}</b>  Smaller = <b>${smaller}</b></div>
    <div class="bal-step"><b>Step 2:</b> Raw SF = ${bigger} / ${smaller} = <b style="color:#f5a623">${raw.toFixed(4)}</b>
      &nbsp;/ decimal shift ${balDecimalShift} = Eff SF <b style="color:#3a9f50">${effSF.toFixed(6)}</b></div>
    <div class="bal-step"><b>Step 3 (Balanced components):</b><br>
      ${priceIsBigger
        ?`Price = ${rawP} div ${balDecimalShift} = <b style="color:#3a9f50">${bP.toFixed(6)}</b>
          &nbsp;|&nbsp; Time = ${rawT} x ${effSF.toFixed(4)} = <b style="color:#3a9f50">${bT.toFixed(6)}</b>`
        :`Time = ${rawT} div ${balDecimalShift} = <b style="color:#3a9f50">${bT.toFixed(6)}</b>
          &nbsp;|&nbsp; Price = ${rawP} x ${effSF.toFixed(4)} = <b style="color:#3a9f50">${bP.toFixed(6)}</b>`}
      &nbsp;  Both ~= equal -> 45deg</div>
    <div class="bal-step"><b>Step 4:</b> PTV = sqrt(${bP.toFixed(4)} + ${bT.toFixed(4)}) = <b style="color:#f5a623;font-size:14px">${balPTV.toFixed(6)}</b></div>`;

  const tb2=document.getElementById('bal-table');tb2.innerHTML='';
  BAL_RATS.forEach(([rv,rn])=>{
    const np=balPTV*rv;
    const comp=np/Math.SQRT2; // each component in balanced units
    // Convert back to real values
    const realDays=priceIsBigger ? comp/effSF : comp;
    const realPM  =priceIsBigger ? comp*balDecimalShift : comp/effSF;
    const up=Math.round(startP+realPM),dn=Math.round(startP-realPM);
    const isSS=SS_RATS.has(rv);
    const tr=document.createElement('tr');
    if(isSS) tr.style.background='rgba(26,92,32,.2)';
    tr.innerHTML=`<td style="font-weight:700;color:${isSS?'#3a9f50':'#ccd'}">${rn}${isSS?' *':''}</td>
      <td style="color:#f5a623;font-weight:700">${np.toFixed(4)}</td>
      <td>${comp.toFixed(4)}</td>
      <td>${realDays.toFixed(2)}d</td>
      <td class="up">UP${up.toLocaleString('en-IN')}</td>
      <td class="dn">DN${dn.toLocaleString('en-IN')}</td>`;
    tb2.appendChild(tr);
  });
}

// 
// UPDATE ALL PANELS
// 
function updatePanels(){
  const pts=getActivePts(),lt=getLastTwo();
  const lastCandle=candles.length-1,lastDate=candles[lastCandle]?.t;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;

  // Auto table
  const tb=document.getElementById('auto-table');tb.innerHTML='';
  let prevPTV=null;
  for(let i=0;i<pts.length-1;i++){
    const{ptv,ang,pp,dayDiff}=calcPTV(pts[i],pts[i+1]);
    const ratio=prevPTV?ptv/prevPTV:null;
    const rl=ratio?ratio.toFixed(3)+'~='+nearestRatioLabel(ratio):'';
    const col=pts[i+1].type==='H'?'#1a8f60':'#c04040';
    const tr=document.createElement('tr');
    tr.innerHTML=`<td style="font-weight:700;color:${col};font-size:11px">${pts[i].label}->${pts[i+1].label}</td>
      <td style="font-weight:700;color:${col}">${ptv.toFixed(2)}</td>
      <td style="font-size:11px">${ang.toFixed(1)}deg</td>
      <td class="hi">${rl}</td>
      <td><span style="font-size:10px;padding:1px 5px;border-radius:3px;background:${col}22;color:${col}">${pts[i+1].type==='H'?'H':'L'}</span></td>`;
    tb.appendChild(tr);prevPTV=ptv;
  }
  document.getElementById('ap-count').textContent=pts.length+' pts';
  if(lt) document.getElementById('auto-sum').textContent=
    showBal
      ? `${lt.prev.label}->${lt.last.label} | BalPTV=${lt.ptv.ptv.toFixed(2)} | 45deg | SF=${balScaleFactor.toFixed(2)}`
      : `${lt.prev.label}->${lt.last.label} PTV=${lt.ptv.ptv.toFixed(2)} Angle=${lt.ptv.ang.toFixed(1)}deg ${isLog()?'Log':'Arith'}`;

  if(!lt){
    document.getElementById('rev-cards').innerHTML='';
    document.getElementById('levels-table').innerHTML='';
    document.getElementById('levels-tags').innerHTML='';
    updateSignalsPanel();
    updateWatchPanel();
    updateConf4D();
    return;
  }

  const{ptv}=lt;
  document.getElementById('trp').value=ptv.ptv.toFixed(2);
  document.getElementById('tra').value=ptv.ang.toFixed(2);
  document.getElementById('trb').value=lt.last.price.toFixed(2);
  document.getElementById('pib').value=ptv.ptv.toFixed(2);
  document.getElementById('rca').value=ptv.ptv.toFixed(2);

  if(showBal){
    document.getElementById('bal-p').value=Math.abs(lt.last.price-lt.prev.price).toFixed(2);
    document.getElementById('bal-t').value=lt.ptv.dayDiff;
    document.getElementById('bal-sp').value=lt.last.price.toFixed(2);
    calcBal();
  }

  const levels=computeLevels();
  const cp=candles[lastCandle]?.c||lt.last.price;
  const nearLevels=findNearLevels(cp,levels,3);

  // Tags
  const td2=document.getElementById('levels-tags');td2.innerHTML='';
  if(nearLevels.length){
    const h=document.createElement('span');h.style.cssText='font-size:11px;color:#f5a623;margin-right:5px;font-weight:700';
    h.textContent=' Near CMP:';td2.appendChild(h);
    nearLevels.forEach(l=>{
      const dU=Math.abs(l.upP-cp)/cp*100,dD=Math.abs(l.dnP-cp)/cp*100,isUp=dU<dD;
      const tag=document.createElement('span');
      tag.style.cssText=`display:inline-flex;align-items:center;font-size:11px;font-weight:600;padding:3px 8px;border-radius:20px;margin:2px;background:${isUp?'rgba(26,143,96,.2)':'rgba(192,64,64,.2)'};border:1px solid ${isUp?'#1a8f6066':'#c0404066'};color:${isUp?'#1a8f60':'#c04040'}`;
      tag.textContent=`${l.label} ${isUp?'UP':'DN'}${Math.round(isUp?l.upP:l.dnP).toLocaleString('en-IN')} (${(isUp?dU:dD).toFixed(1)}%)`;
      td2.appendChild(tag);
    });
  } else td2.innerHTML='<span style="font-size:11px;color:#333">No levels within 3% of CMP</span>';

  // Levels table
  // Main table: STRONG + NEAR only
  const ltb=document.getElementById('levels-table');ltb.innerHTML='';
  const ltbAll=document.getElementById('levels-table-all');ltbAll.innerHTML='';
  document.getElementById('levels-all-count').textContent=levels.length;

  const makeRow=(l,compact)=>{
    const tr=document.createElement('tr');
    const isNear=nearLevels.includes(l),isFuture=l.daysFromNow>0;
    if(isNear) tr.style.background='rgba(245,166,35,.08)';
    else if(!isFuture) tr.style.opacity='.38';
    let estDate='';
    if(lastDate&&isFuture){const ms=lastDate.getTime()+l.daysFromNow*24*60*60*1000;estDate=fmDate(new Date(ms));}
    const sc=l.strength==='strong'?'#f5a623':l.strength==='medium'?'#185FA5':'#252840';
    tr.innerHTML=`<td style="font-weight:700;font-size:11px">${l.sym}${l.rn}</td>
      <td class="hi" style="font-size:11px">${l.np.toFixed(2)}</td>
      <td style="font-size:11px">${l.daysFwd.toFixed(0)}d</td>
      <td style="font-size:10px;color:#444">${estDate}</td>
      <td class="up" style="font-size:11px">${Math.round(l.upP).toLocaleString('en-IN')}</td>
      <td class="dn" style="font-size:11px">${Math.round(l.dnP).toLocaleString('en-IN')}</td>
      <td><span style="font-size:10px;padding:1px 5px;border-radius:3px;background:${sc}22;color:${sc};border:1px solid ${sc}44">${l.strength}</span>
      ${isNear?'<span style="font-size:10px;padding:1px 5px;border-radius:3px;background:#f5a62333;color:#f5a623;margin-left:2px">NEAR</span>':''}</td>`;
    return tr;
  };

  // Main: only strong + near future levels
  levels.filter(l=>(l.strength==='strong'||nearLevels.includes(l))&&l.future).forEach(l=>ltb.appendChild(makeRow(l,true)));
  if(!ltb.children.length){
    ltb.innerHTML='<tr><td colspan="7" style="color:#444;padding:6px;font-size:11px">No strong/near levels in future. See all levels below.</td></tr>';
  }
  // All: every level
  levels.forEach(l=>ltbAll.appendChild(makeRow(l,false)));

  // Reversals
  const upcoming=levels.filter(l=>l.future&&l.strength!=='weak');
  const groups=[];
  upcoming.forEach(l=>{const g=groups.find(x=>Math.abs(x.days-l.daysFwd)<=3);if(g){g.levels.push(l);if(l.strength==='strong')g.strength='strong';}else groups.push({days:l.daysFwd,levels:[l],strength:l.strength});});
  const revDiv=document.getElementById('rev-cards');revDiv.innerHTML='';
  document.getElementById('rev-count').textContent=groups.length+' zones';
  groups.slice(0,6).forEach(g=>{
    const card=document.createElement('div');card.className=`rev-card ${g.strength}`;
    let estDate='';if(lastDate){const ms=lastDate.getTime()+g.days*24*60*60*1000;estDate=fmDate(new Date(ms));}
    const conf=g.levels.length>=3?'HIGH':g.levels.length>=2?'MEDIUM':'LOW';
    card.innerHTML=`<span class="conf-badge ${conf==='HIGH'?'ch':'cm'}" style="float:right">${conf}</span>
      <div style="font-size:12px;font-weight:700;margin-bottom:3px">Day +${g.days.toFixed(0)}  ${estDate}</div>
      <div style="font-size:11px;color:#444;margin-bottom:4px">${g.levels.length} ratios converge</div>
      <div style="display:flex;flex-wrap:wrap;gap:3px">${g.levels.map(l=>`<span style="font-size:11px;padding:2px 7px;border-radius:20px;background:#185FA522;border:1px solid #185FA544;color:#5a9fd5">${l.label} UP${Math.round(l.upP).toLocaleString('en-IN')} DN${Math.round(l.dnP).toLocaleString('en-IN')}</span>`).join('')}</div>`;
    revDiv.appendChild(card);
  });

  updateSignalsPanel();
  updateWatchPanel();
  updateConf4D();
  updateRatioHitBar();
  c1();c2();c3();c4();ct();cpi();cr();
}

// 
// CALCULATORS
// 
function c1(){
  const np=getNP('m1op','m1rat');if(!np) return;
  const fd=parseFloat(document.getElementById('m1d').value)||0;
  const dir=parseInt(document.getElementById('m1dir').value);
  const from=getFromPrice('m1f','m1cp');
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  document.getElementById('m1cw').style.display=document.getElementById('m1f').value==='custom'?'block':'none';
  document.getElementById('m1np').textContent=np.toFixed(2);document.getElementById('m1fv').textContent=Math.round(from).toLocaleString('en-IN');
  const lt=getLastTwo();document.getElementById('m1np2').textContent=lt?(lt.ptv.ptv.toFixed(2)+(document.getElementById('m1op').value==='m'?' x ':' div ')+document.getElementById('m1rat').value):'';
  const fh=fd*hpd,d2=np*np-fh*fh,w=document.getElementById('m1w');
  if(d2<0){w.style.display='block';['m1tg','m1mv'].forEach(x=>document.getElementById(x).textContent='');return;}
  w.style.display='none';
  const pm=Math.sqrt(d2),tgt=pf(from,pm,dir);
  document.getElementById('m1mv').textContent=(dir>0?'+':'-')+fm(pm);document.getElementById('m1mv').className=dir>0?'up':'dn';
  document.getElementById('m1tg').textContent=Math.round(tgt).toLocaleString('en-IN');document.getElementById('m1tg').className=dir>0?'up':'dn';
  document.getElementById('m1sb').textContent='sqrt('+np.toFixed(2)+''+fh.toFixed(1)+')='+pm.toFixed(2);
}
function c2(){
  const np=getNP('m2op','m2rat');if(!np) return;
  const tgt=parseFloat(document.getElementById('m2tg').value)||1;const from=getFromPrice('m2f','m2cp');
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  document.getElementById('m2cw').style.display=document.getElementById('m2f').value==='custom'?'block':'none';
  document.getElementById('m2np').textContent=np.toFixed(2);document.getElementById('m2fv').textContent=Math.round(from).toLocaleString('en-IN');
  const pm=pd(from,tgt);document.getElementById('m2pc').textContent=fm(pm);
  const d2=np*np-pm*pm,w=document.getElementById('m2w');
  if(d2<0){w.style.display='block';document.getElementById('m2dy').textContent='';return;}
  w.style.display='none';const hrs=Math.sqrt(d2);
  document.getElementById('m2dy').textContent=(hrs/hpd).toFixed(0)+'d';
  document.getElementById('m2sb').textContent='sqrt('+np.toFixed(2)+''+pm.toFixed(2)+')='+hrs.toFixed(1)+'h';
}
function c3(){
  const lt=getLastTwo();if(!lt) return;
  const np=getNP('m3op','m3rat'),dir=parseInt(document.getElementById('m3dir').value);
  const from=getFromPrice('m3f','m3cp');const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  document.getElementById('m3cw').style.display=document.getElementById('m3f').value==='custom'?'block':'none';
  const nH=Math.cos(lt.ptv.ang*Math.PI/180)*np,nP=Math.sin(lt.ptv.ang*Math.PI/180)*np;
  document.getElementById('m3np').textContent=np.toFixed(2);document.getElementById('m3ag').textContent=lt.ptv.ang.toFixed(2)+'deg';
  document.getElementById('m3dy').textContent=(nH/hpd).toFixed(0)+'d';
  const pr=pf(from,nP,dir);
  document.getElementById('m3pr').textContent=Math.round(pr).toLocaleString('en-IN');document.getElementById('m3pr').className=dir>0?'up':'dn';
  document.getElementById('m3pc').textContent=fm(nP);
}
function c4(){
  const lt=getLastTwo();if(!lt) return;
  const dir=parseInt(document.getElementById('m4dir').value)||1,el=parseFloat(document.getElementById('m4el').value)||0;
  const tgt=parseFloat(document.getElementById('m4tg').value)||0,from=getFromPrice('m4f','m4cp');
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5,tP=tgt>0?pd(from,tgt):null;
  document.getElementById('m4cw').style.display=document.getElementById('m4f').value==='custom'?'block':'none';
  const tb=document.getElementById('t4');tb.innerHTML='';
  RATS.forEach(([rv,rn])=>{[['m','x'],['d','div']].forEach(([op,sym])=>{
    if(rv===1&&op==='d') return;
    const np=op==='m'?lt.ptv.ptv*rv:lt.ptv.ptv/rv;
    const nH=Math.cos(lt.ptv.ang*Math.PI/180)*np,nP=Math.sin(lt.ptv.ang*Math.PI/180)*np;
    const nD=nH/hpd,fn=nD-el,pr=pf(from,nP,dir);
    let dTgt='';if(tP!==null){const d2=np*np-tP*tP;dTgt=d2>=0?(Math.sqrt(d2)/hpd).toFixed(0)+'d':'small';}
    const tr=document.createElement('tr');
    if(fn>=0&&fn<=90) tr.style.background='rgba(26,143,96,.07)';else if(fn<0) tr.style.opacity='.4';
    tr.innerHTML=`<td><b>${sym}${rn}</b></td><td class="hi">${np.toFixed(2)}</td><td>${nD.toFixed(0)}d</td>
      <td style="font-weight:700;color:${fn>=0?'#1a8f60':'#c04040'}">${fn>=0?'+'+fn.toFixed(0)+'d':'PAST'}</td>
      <td class="${dir>0?'up':'dn'}">${Math.round(pr).toLocaleString('en-IN')}</td><td>${dTgt}</td>`;
    tb.appendChild(tr);
  });});
}
function ct(){
  const ptv=parseFloat(document.getElementById('trp').value)||0,ang=parseFloat(document.getElementById('tra').value)||0;
  const base=parseFloat(document.getElementById('trb').value)||1,dir=parseInt(document.getElementById('trd').value);
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5,rad=ang*Math.PI/180;
  const pm=Math.sin(rad)*ptv,hrs=Math.cos(rad)*ptv,pr=pf(base,pm,dir);
  document.getElementById('trpc').textContent=fm(pm);document.getElementById('trhr').textContent=hrs.toFixed(1)+'h';
  document.getElementById('trdy').textContent=(hrs/hpd).toFixed(0)+'d';
  document.getElementById('trpr').textContent=Math.round(pr).toLocaleString('en-IN');document.getElementById('trpr').className=dir>0?'up':'dn';
}
function cpi(){
  const b=parseFloat(document.getElementById('pib').value)||0;
  document.getElementById('pi1').textContent=(b*Math.PI).toFixed(2);
  document.getElementById('pi2').textContent=(b*Math.sqrt(10)).toFixed(2);
  document.getElementById('pi3').textContent=(b/Math.PI).toFixed(2);
  const tb=document.getElementById('t7');tb.innerHTML='';
  RATS.forEach(([rv,rn])=>{const tr=document.createElement('tr');tr.innerHTML=`<td>${rn}</td><td>${rv}</td><td class="up">${(b*rv).toFixed(2)}</td><td class="dn">${(b/rv).toFixed(2)}</td>`;tb.appendChild(tr);});
}
function cr(){
  const a=parseFloat(document.getElementById('rca').value)||1,bv=parseFloat(document.getElementById('rcb').value)||1;
  const div=bv/a;document.getElementById('rcd').textContent=div.toFixed(4);
  document.getElementById('rcm').textContent='~= '+nearestRatioLabel(div);
  const tb=document.getElementById('t6');tb.innerHTML='';
  RATS.forEach(([rv,rn])=>{[['x',a*rv],['div',a/rv]].forEach(([sym,val])=>{
    const err=Math.abs(val-bv),ep=(err/Math.max(bv,.001)*100).toFixed(2),isM=ep<5;
    const tr=document.createElement('tr');if(isM)tr.style.background='rgba(26,143,96,.1)';
    tr.innerHTML=`<td>${sym} ${rn}</td><td>${rn}</td><td>${val.toFixed(2)}</td><td>${err.toFixed(2)}</td><td>${ep}%</td><td>${isM?'':ep<10?'~':''}</td>`;
    tb.appendChild(tr);
  });});
}

// 
// PAN / ZOOM
// 
function initView(){viewStart=0;viewEnd=candles.length-1;updateZL();}
function getFB(){return Math.max(10,Math.min(300,parseInt(document.getElementById('futbars').value)||60));}
function getTotalBars(){return candles.length+getFB();}
function getVR(){const vs=Math.max(0,Math.round(viewStart)),ve=Math.min(getTotalBars()-1,Math.round(viewEnd));return{vs,ve,count:ve-vs+1};}
function zoomIn(){const{vs,ve}=getVR(),mid=(vs+ve)/2,h=(ve-vs)/2;viewStart=Math.max(0,mid-h*.6);viewEnd=Math.min(getTotalBars()-1,mid+h*.6);updateZL();draw();}
function zoomOut(){const{vs,ve}=getVR(),mid=(vs+ve)/2,h=(ve-vs)/2;viewStart=Math.max(0,mid-h/.6);viewEnd=Math.min(getTotalBars()-1,mid+h/.6);updateZL();draw();}
function zoomReset(){initView();draw();}
function panLeft(){const{count}=getVR(),s=Math.max(1,Math.round(count*.2));viewStart=Math.max(0,viewStart-s);viewEnd=viewStart+count-1;updateZL();draw();}
function panRight(){const total=getTotalBars(),{count}=getVR(),s=Math.max(1,Math.round(count*.2));viewEnd=Math.min(total-1,viewEnd+s);viewStart=viewEnd-count+1;if(viewStart<0)viewStart=0;updateZL();draw();}
function panStart(){viewStart=0;const{count}=getVR();viewEnd=Math.min(getTotalBars()-1,count-1);updateZL();draw();}
function panEnd(){viewEnd=candles.length-1;const{count}=getVR();viewStart=Math.max(0,viewEnd-count+1);updateZL();draw();}
function panFuture(){
  // Show last 30 candles + all future bars
  const fb=getFB();viewEnd=getTotalBars()-1;viewStart=Math.max(0,candles.length-30);updateZL();draw();
}
function updateZL(){const{vs,ve,count}=getVR();document.getElementById('zlbl').textContent=count+'c ('+vs+''+ve+')';}

// 
// DRAW
// 
function draw(){
  const cv=document.getElementById('c');
  const W=Math.min(window.innerWidth-20,1850),H=650;
  cv.width=W*devicePixelRatio;cv.height=H*devicePixelRatio;
  cv.style.width=W+'px';cv.style.height=H+'px';
  const ctx=cv.getContext('2d');ctx.scale(devicePixelRatio,devicePixelRatio);
  ctx.fillStyle='#ffffff';ctx.fillRect(0,0,W,H);

  if(!candles.length){
    ctx.fillStyle='#333';ctx.font='14px sans-serif';ctx.textAlign='center';
    ctx.fillText('Press  Load to fetch data',W/2,H/2);return;
  }

  const{vs,ve,count}=getVR();
  const pad={l:78,r:90,t:28,b:38};
  const pw=W-pad.l-pad.r,ph=H-pad.t-pad.b;
  const gap=pw/count,cw=Math.max(1,gap-1);
  cv._vs=vs;cv._ve=ve;cv._count=count;cv._gap=gap;cv._pad=pad;cv._ph=ph;cv._pw=pw;

  // Price range from VISIBLE real candles  always use normal scaling
  const visReal=candles.slice(Math.min(vs,candles.length-1),Math.min(ve+1,candles.length));
  let lo=visReal.length?Math.min(...visReal.map(c=>c.l)):candles[candles.length-1].l;
  let hi=visReal.length?Math.max(...visReal.map(c=>c.h)):candles[candles.length-1].h;
  const pp2=(hi-lo)*0.08;lo-=pp2;hi+=pp2;

  cv._lo=lo;cv._hi=hi;

  const tx=i=>pad.l+(i-vs)*gap+gap/2;
  const ty=p=>pad.t+(1-(p-lo)/(hi-lo))*ph;
  const lastCandle=candles.length-1;

  // Show balanced mode indicator on chart
  if(showBal){
    ctx.fillStyle='rgba(26,142,80,.07)';
    ctx.fillRect(pad.l,pad.t,pw,ph);
    ctx.fillStyle='rgba(26,142,80,.7)';ctx.font='bold 11px sans-serif';ctx.textAlign='right';
    ctx.fillText(' BALANCED 45deg MODE  sf='+balScaleFactor.toFixed(2),pad.l+pw-4,pad.t+14);
    ctx.textAlign='left';
  }

  //  GRID 
  ctx.strokeStyle='rgba(0,0,0,.06)';ctx.lineWidth=1;
  for(let i=0;i<=6;i++){
    const y=pad.t+i/6*ph;
    ctx.beginPath();ctx.moveTo(pad.l,y);ctx.lineTo(pad.l+pw,y);ctx.stroke();
    const pv=hi-(i/6)*(hi-lo);
    ctx.fillStyle='#555';ctx.font='10px sans-serif';ctx.textAlign='right';
    ctx.fillText(pv>=1000?Math.round(pv).toLocaleString('en-IN'):pv.toFixed(2),pad.l-4,y+4);
  }
  // Vertical grid lines
  ctx.strokeStyle='rgba(0,0,0,.04)';ctx.lineWidth=1;
  const step=Math.max(1,Math.floor(count/8));
  for(let i=vs;i<=ve;i+=step){
    const x=tx(i);
    ctx.beginPath();ctx.moveTo(x,pad.t);ctx.lineTo(x,pad.t+ph);ctx.stroke();
    ctx.fillStyle='#555';ctx.font='10px sans-serif';ctx.textAlign='center';
    if(i<candles.length) ctx.fillText(candles[i].t.toLocaleDateString('en-IN',{day:'2-digit',month:'short'}),x,pad.t+ph+22);
    else ctx.fillText('+'+(i-candles.length+1)+'d',x,pad.t+ph+22);
  }

  //  FUTURE ZONE 
  const futX=tx(candles.length);
  if(futX<pad.l+pw){
    ctx.fillStyle='rgba(24,95,165,.04)';
    ctx.fillRect(futX,pad.t,pad.l+pw-futX,ph);
    ctx.strokeStyle='rgba(24,95,165,.4)';ctx.lineWidth=1.5;ctx.setLineDash([4,4]);
    ctx.beginPath();ctx.moveTo(futX,pad.t);ctx.lineTo(futX,pad.t+ph);ctx.stroke();ctx.setLineDash([]);
    ctx.fillStyle='rgba(24,95,165,.7)';ctx.font='bold 10px sans-serif';ctx.textAlign='left';
    ctx.fillText(' FUTURE',futX+4,pad.t+13);
  }

  //  CMP LINE 
  const lc=candles[lastCandle];
  const yCMP=ty(lc.c);
  ctx.strokeStyle='rgba(0,0,0,.15)';ctx.lineWidth=1;ctx.setLineDash([4,4]);
  ctx.beginPath();ctx.moveTo(pad.l,yCMP);ctx.lineTo(pad.l+pw,yCMP);ctx.stroke();ctx.setLineDash([]);
  ctx.fillStyle='rgba(24,95,165,.85)';ctx.font='bold 10px sans-serif';ctx.textAlign='left';
  ctx.fillText('CMP '+Math.round(lc.c).toLocaleString('en-IN'),pad.l+pw+2,yCMP+4);

  //  CANDLES 
  candles.forEach((c,i)=>{
    if(i<vs||i>ve) return;
    const x=tx(i),isUp=c.c>=c.o,col=isUp?'#1a8f60':'#c04040';
    ctx.strokeStyle=col;ctx.lineWidth=1;
    ctx.beginPath();ctx.moveTo(x,ty(c.h));ctx.lineTo(x,ty(c.l));ctx.stroke();
    const cy1=ty(Math.max(c.o,c.c)),cy2=ty(Math.min(c.o,c.c));
    ctx.fillStyle=col;ctx.fillRect(x-cw/2,cy1,cw,Math.max(1,cy2-cy1));
  });

  const pts=getActivePts();
  const lt=getLastTwo();
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;

  //  GANN ANGLES 
  // Balanced mode: ONLY 1x2 (26.6deg) and 2x1 (63.4deg)  the true Gann angles at correct scale
  // Normal mode: all 5 angles from last 2 points only
  if(showGann&&pts.length>0){
    const gannPts=pts.slice(-2);
    // In balanced mode, use only the 2x1 and 1x2 angles
    const activeAngles=showBal?[
      [63.4,'2x1','rgba(255,220,40,.85)'],   // 2 price : 1 time
      [26.6,'1x2','rgba(120,255,120,.7)']    // 1 price : 2 time
    ]:GANN_ANGLES;

    gannPts.forEach(pt=>{
      if(pt.idx<vs-5||pt.idx>ve+5) return;
      const gx0=tx(pt.idx),gy0=ty(pt.price);
      activeAngles.forEach(([deg,lbl,col])=>{
        [1,-1].forEach(dir=>{
          const extBars=(ve-Math.min(pt.idx,ve)+100);
          const dxFull=extBars*gap;
          const endX=Math.min(gx0+dxFull,pad.l+pw);
          const frac=(endX-gx0)/Math.max(dxFull,.001);
          const endY=gy0+dxFull*Math.tan(deg*Math.PI/180)*dir*(-1)*frac;
          if(endY<pad.t-100||endY>pad.t+ph+100) return;
          ctx.strokeStyle=col;ctx.lineWidth=showBal?1.5:1;ctx.setLineDash([4,4]);
          ctx.beginPath();ctx.moveTo(gx0,gy0);ctx.lineTo(endX,endY);ctx.stroke();ctx.setLineDash([]);
          if(dir===1&&endY>pad.t&&endY<pad.t+ph&&endX<pad.l+pw-3){
            ctx.fillStyle=col;ctx.font=showBal?'bold 9px sans-serif':'9px sans-serif';ctx.textAlign='left';
            ctx.fillText(lbl,endX-20,endY-3);
          }
        });
      });
    });
  }

  //  PROJECTIONS  only from last completed swing 
  if(showProj&&lt){
    const{ptv,last}=lt;
    const px0=tx(last.idx),py0=ty(last.price);
    const levels=computeLevels();
    const nearLevels=findNearLevels(candles[lastCandle].c,levels,3);

    levels.filter(l=>l.future).forEach(l=>{
      const xe=tx(l.targetDayIdx);
      if(xe>pad.l+pw+10) return;
      const alpha=l.strength==='strong'?.65:l.strength==='medium'?.4:.18;
      const isNear=nearLevels.includes(l);
      const lw=isNear?2.5:l.strength==='strong'?1.5:.8;
      const da=l.strength==='strong'?[6,3]:l.strength==='medium'?[4,4]:[2,6];

      [[l.upP,'rgba(26,143,96,'],[l.dnP,'rgba(192,64,64,']].forEach(([pr,rgb])=>{
        const yp=ty(pr);if(yp<pad.t||yp>pad.t+ph) return;
        const col=isNear?`rgba(245,166,35,${alpha})`:rgb+alpha+')';
        ctx.strokeStyle=col;ctx.lineWidth=lw;ctx.setLineDash(da);
        ctx.beginPath();ctx.moveTo(px0,py0);ctx.lineTo(Math.min(xe,pad.l+pw),yp);ctx.stroke();ctx.setLineDash([]);
        ctx.strokeStyle=col.replace(alpha,alpha*.3);ctx.lineWidth=.8;ctx.setLineDash([2,6]);
        ctx.beginPath();ctx.moveTo(pad.l,yp);ctx.lineTo(Math.min(xe,pad.l+pw),yp);ctx.stroke();ctx.setLineDash([]);
        if(xe>=pad.l+pw-5){
          ctx.fillStyle=col;ctx.font=(isNear?'bold ':'')+' 9px sans-serif';ctx.textAlign='left';
          ctx.fillText(l.label+(pr===l.upP?'UP':'DN')+Math.round(pr).toLocaleString('en-IN'),pad.l+pw+2,yp+(pr===l.dnP?10:0));
        }
        if(isNear){ctx.beginPath();ctx.arc(Math.min(xe,pad.l+pw),yp,4,0,2*Math.PI);ctx.fillStyle=col;ctx.fill();}
      });

      if(l.strength==='strong'&&xe>=pad.l&&xe<=pad.l+pw){
        ctx.strokeStyle='rgba(245,166,35,.18)';ctx.lineWidth=1;ctx.setLineDash([3,5]);
        ctx.beginPath();ctx.moveTo(xe,pad.t);ctx.lineTo(xe,pad.t+ph);ctx.stroke();ctx.setLineDash([]);
        ctx.fillStyle='rgba(245,166,35,.5)';ctx.font='9px sans-serif';ctx.textAlign='center';
        ctx.fillText(l.label,xe,pad.t+11);
      }
    });
  }

  //  BUY/SELL SIGNALS in future zone 
  if(showProj&&candles.length){
    const signals=computeSignals();
    signals.slice(0,8).forEach(sig=>{
      const isBuy=sig.signalType==='BUY';
      const col=isBuy?'#1a8f60':'#c04040';
      const xe=tx(lastCandle+sig.avgDays);
      const yp=ty(sig.avgPrice);
      if(xe<pad.l-10||xe>pad.l+pw+10) return;
      const strength=sig.score>=8?'STRONG':sig.score>=5?'MOD':'WATCH';

      ctx.strokeStyle=col+(sig.score>=8?'cc':'88');ctx.lineWidth=sig.score>=8?2:1.5;ctx.setLineDash([5,3]);
      ctx.beginPath();ctx.moveTo(xe,pad.t);ctx.lineTo(xe,pad.t+ph);ctx.stroke();ctx.setLineDash([]);

      if(yp>=pad.t&&yp<=pad.t+ph){
        ctx.strokeStyle=col+'55';ctx.lineWidth=1;ctx.setLineDash([3,6]);
        ctx.beginPath();ctx.moveTo(pad.l,yp);ctx.lineTo(Math.min(xe,pad.l+pw),yp);ctx.stroke();ctx.setLineDash([]);
      }

      if(yp>=pad.t-20&&yp<=pad.t+ph+20){
        const xe2=Math.max(pad.l+4,Math.min(xe,pad.l+pw-4));
        ctx.fillStyle=col;ctx.beginPath();
        if(isBuy){ctx.moveTo(xe2-8,yp+18);ctx.lineTo(xe2+8,yp+18);ctx.lineTo(xe2,yp+4);}
        else{ctx.moveTo(xe2-8,yp-18);ctx.lineTo(xe2+8,yp-18);ctx.lineTo(xe2,yp-4);}
        ctx.closePath();ctx.fill();
        const slbl=`${isBuy?'BUY':'SELL'} ${strength}`;
        const slw=slbl.length*5+8;
        ctx.fillStyle='rgba(255,255,255,.93)';ctx.fillRect(xe2-slw/2,isBuy?yp+19:yp-31,slw,13);
        ctx.fillStyle=col;ctx.font='bold 9px sans-serif';ctx.textAlign='center';ctx.fillText(slbl,xe2,isBuy?yp+29:yp-21);
        if(xe>=pad.l+pw-10){
          ctx.fillStyle=col+'cc';ctx.font='bold 9px sans-serif';ctx.textAlign='left';
          ctx.fillText((isBuy?'BUY':'SELL')+' '+Math.round(sig.avgPrice).toLocaleString('en-IN'),pad.l+pw+2,yp+(isBuy?-4:14));
        }
      }
    });
  }

  //  DEV PTV RATIO HIT ALERT ON CHART 
  // When dev PTV div any swing = exact Cowan ratio -> show BIG alert
  if(pts.length>=2&&candles.length){
    const swingPts2=manualPts.length>=2?[...manualPts].sort((a,b)=>a.idx-b.idx):pts;
    const lastSw2=swingPts2[swingPts2.length-1];
    const devDays2=lastCandle-lastSw2.idx;
    const devPP2=Math.abs(lc.c-lastSw2.price);
    const hpd2=parseFloat(document.getElementById('hpd').value)||6.5;
    const devHrs2=devDays2*hpd2;
    const devPTV2=devDays2>0?Math.sqrt(devPP2*devPP2+devHrs2*devHrs2):0;

    if(devPTV2>0){
      // Check all swings
      const swings2=[];
      for(let i=0;i<swingPts2.length-1;i++){
        const r=calcPTV(swingPts2[i],swingPts2[i+1]);
        if(r.ptv>0) swings2.push({ptv:r.ptv,label:swingPts2[i].label+'->'+swingPts2[i+1].label});
      }

      let bestHit=null,bestErr=999;
      swings2.forEach(sw=>{
        if(sw.ptv<=0) return;
        const ratio=devPTV2/sw.ptv;
        if(ratio<0.1||ratio>10) return;
        const match=nearestCowanRatio(ratio);
        if(match.err<bestErr){bestErr=match.err;bestHit={sw,match,ratio,devPTV:devPTV2};}
      });

      if(bestHit&&bestErr<=8){
        // Draw alert box near CMP line
        const yCMP2=ty(lc.c);
        const alertX=pad.l+pw-180;
        const isExact=bestErr<2;
        const bgCol=isExact?'rgba(245,166,35,.25)':'rgba(24,95,165,.2)';
        const borderCol=isExact?'#f5a623':'#185FA5';
        const textCol=isExact?'#f5a623':'#7ab5e0';

        // Alert box
        ctx.fillStyle=bgCol;
        roundRect(ctx,alertX,yCMP2-40,175,55,7);
        ctx.fill();
        ctx.strokeStyle=borderCol;ctx.lineWidth=isExact?2:1.5;
        ctx.stroke();

        // Glowing effect for exact
        if(isExact){
          ctx.shadowColor='#f5a623';ctx.shadowBlur=12;
          ctx.strokeStyle='#f5a623aa';ctx.lineWidth=1;
          ctx.stroke();
          ctx.shadowBlur=0;
        }

        ctx.fillStyle=textCol;ctx.font='bold 10px sans-serif';ctx.textAlign='left';
        ctx.fillText(isExact?'* EXACT RATIO HIT':' RATIO HIT',alertX+8,yCMP2-26);
        ctx.font='bold 11px sans-serif';
        ctx.fillText(`dev(${bestHit.devPTV.toFixed(1)}) / ${bestHit.sw.label}(${bestHit.sw.ptv.toFixed(1)})`,alertX+8,yCMP2-12);
        ctx.fillStyle='#f5a623';ctx.font='bold 12px sans-serif';
        ctx.fillText(`= ${bestHit.ratio.toFixed(4)} ~= ${bestHit.match.rn}  (${bestErr.toFixed(1)}%)`,alertX+8,yCMP2+5);

        // Arrow pointing to CMP
        ctx.fillStyle=borderCol;ctx.beginPath();
        ctx.moveTo(alertX+87,yCMP2+8);ctx.lineTo(alertX+80,yCMP2+16);ctx.lineTo(alertX+94,yCMP2+16);
        ctx.closePath();ctx.fill();
      }
    }
  }

  //  DEVELOPING SWING (dashed yellow from last swing -> now) 
  if(pts.length>=1&&candles.length){
    const lastSw=pts[pts.length-1];
    const devDays=lastCandle-lastSw.idx;
    const devPP=pd(lastSw.price,lc.c);
    const devHrs=devDays*hpd;
    const devPTV=devDays>0?Math.sqrt(devPP*devPP+devHrs*devHrs):0;
    if(devPTV>0&&lastSw.idx>=vs-5&&lastSw.idx<=ve){
      const dsx=tx(lastSw.idx),dsy=ty(lastSw.price);
      const dex=tx(lastCandle),dey=ty(lc.c);
      ctx.strokeStyle='rgba(245,166,35,.7)';ctx.lineWidth=2;ctx.setLineDash([6,3]);
      ctx.beginPath();ctx.moveTo(dsx,dsy);ctx.lineTo(dex,dey);ctx.stroke();ctx.setLineDash([]);
      const dmx=(dsx+dex)/2,dmy=(dsy+dey)/2-12;
      const dlbl=`dev PTV ${devPTV.toFixed(1)}`;
      ctx.font='bold 9px sans-serif';ctx.textAlign='center';
      ctx.fillStyle='rgba(255,255,255,.9)';ctx.fillRect(dmx-dlbl.length*2.8,dmy-10,dlbl.length*5.6,12);
      ctx.fillStyle='rgba(245,166,35,.9)';ctx.fillText(dlbl,dmx,dmy);
    }
  }

  //  CONFLUENCE RATIO LINES ON CHART 
  // Draws every PTV x ratio as a line from each swing point
  // Colour = swing colour, thin dashed, convergence zones glow
  if(showConfChart&&pts.length>=1){
    const hpd2=parseFloat(document.getElementById('hpd').value)||6.5;
    const CHART_RATS=[
      [0.382,'0.382'],[0.5,'0.5'],[0.618,'0.618'],[1,'1x'],
      [1.414,'sqrt2'],[1.618,'PHI'],[1.732,'sqrt3'],[2,'2x'],
      [2.236,'sqrt5'],[3,'3x'],[3.14159,'\u03c0']
    ];

    // First pass: compute all lines to find convergence zones
    const confLines=[];
    for(let ci=0;ci<pts.length-1;ci++){
      const p1=pts[ci],p2=pts[ci+1];
      const{ptv:bPTV,ang:bAng}=calcPTV(p1,p2);
      if(bPTV<=0) continue;
      const swCol=COLORS[ci%COLORS.length];
      CHART_RATS.forEach(([rv,rn])=>{
        const np=bPTV*rv;
        const nH=Math.cos(bAng*Math.PI/180)*np;
        const nP=Math.sin(bAng*Math.PI/180)*np;
        const daysFwd=nH/hpd2;
        const targetBar=p2.idx+daysFwd;
        const upP=pf(p2.price,nP,1);
        const dnP=pf(p2.price,nP,-1);
        const daysFromNow=targetBar-lastCandle;
        if(daysFromNow<-5||daysFromNow>getFB()+10) return;
        confLines.push({p2,bPTV,rv,rn,np,targetBar,upP,dnP,swCol,swIdx:ci,daysFromNow});
      });
    }

    // Find convergence  same bar (3) + same price (1.5%)
    const confMap=new Map();
    confLines.forEach(l=>{
      [l.upP,l.dnP].forEach(pr=>{
        const bKey=Math.round(l.targetBar/3)*3;
        const pKey=Math.round(pr/pr*100);
        confLines.forEach(l2=>{
          if(l2===l) return;
          [l2.upP,l2.dnP].forEach(pr2=>{
            const barDiff=Math.abs(l.targetBar-l2.targetBar);
            const priceDiff=Math.abs(pr-pr2)/Math.max(pr,1)*100;
            if(barDiff<=3&&priceDiff<=1.5&&l.swIdx!==l2.swIdx){
              const key=Math.round(l.targetBar)+'_'+Math.round(pr/100)*100;
              confMap.set(key,(confMap.get(key)||0)+1);
            }
          });
        });
      });
    });

    // Draw lines  thicker and brighter if they converge with another swing
    confLines.forEach(l=>{
      const x0=tx(l.p2.idx),xe=tx(l.targetBar);
      if(xe<pad.l-20) return;
      const alpha=0.25;
      const col=l.swCol;

      [l.upP,l.dnP].forEach((pr,di)=>{
        const y0=ty(l.p2.price),ye=ty(pr);
        if(ye<pad.t-10||ye>pad.t+ph+10) return;

        // Check if this line is part of a convergence
        const key=Math.round(l.targetBar)+'_'+Math.round(pr/100)*100;
        const isConv=confMap.get(key)>0;
        const lw=isConv?2:0.8;
        const a=isConv?0.6:0.2;

        ctx.strokeStyle=col;ctx.globalAlpha=a;
        ctx.lineWidth=lw;
        ctx.setLineDash(isConv?[6,3]:[2,6]);
        ctx.beginPath();ctx.moveTo(x0,y0);ctx.lineTo(Math.min(xe,pad.l+pw+50),ye);ctx.stroke();
        ctx.setLineDash([]);ctx.globalAlpha=1;

        // At convergence point  draw a glowing dot
        if(isConv&&xe>=pad.l-5&&xe<=pad.l+pw+20){
          const dotX=Math.min(xe,pad.l+pw);
          ctx.beginPath();ctx.arc(dotX,ye,isConv?5:3,0,2*Math.PI);
          ctx.fillStyle=col;ctx.globalAlpha=0.8;ctx.fill();ctx.globalAlpha=1;
          // Label ratio near end
          if(xe<=pad.l+pw-5){
            ctx.fillStyle=col;ctx.globalAlpha=0.9;
            ctx.font='bold 8px sans-serif';ctx.textAlign='left';
            ctx.fillText(l.rn,dotX+4,ye+(di===0?-3:9));
            ctx.globalAlpha=1;
          }
        } else if(xe<=pad.l+pw-5&&a>0.15){
          // Label ratio for all lines near end
          ctx.fillStyle=col;ctx.globalAlpha=Math.max(0.15,a*0.7);
          ctx.font='bold 8px sans-serif';ctx.textAlign='left';
          ctx.fillText(l.rn,Math.min(xe,pad.l+pw-5)+2,ye+(di===0?-2:8));
          ctx.globalAlpha=1;
        }
      });
    });

    // Draw convergence zone highlights  golden circles where lines meet
    const drawnZones=new Set();
    confLines.forEach(l=>{
      [l.upP,l.dnP].forEach(pr=>{
        const key=Math.round(l.targetBar)+'_'+Math.round(pr/100)*100;
        if(!confMap.get(key)||drawnZones.has(key)) return;
        drawnZones.add(key);
        const xe=tx(l.targetBar),ye=ty(pr);
        if(xe<pad.l||xe>pad.l+pw+10||ye<pad.t||ye>pad.t+ph) return;
        // Glowing ring
        const score=confMap.get(key);
        const r=8+score*3;
        ctx.beginPath();ctx.arc(xe,ye,r,0,2*Math.PI);
        ctx.strokeStyle=score>=3?'rgba(245,166,35,.7)':'rgba(255,255,255,.3)';
        ctx.lineWidth=score>=3?2:1;ctx.stroke();
        if(score>=3){
          ctx.fillStyle='rgba(245,166,35,.15)';ctx.fill();
          ctx.fillStyle='rgba(245,166,35,.9)';ctx.font='bold 9px sans-serif';ctx.textAlign='center';
          ctx.fillText(score+'x',xe,ye+3);
        }
      });
    });
  }

  //  PTV VECTORS between all swing points 
  for(let vi=0;vi<pts.length-1;vi++){
    const vp1=pts[vi],vp2=pts[vi+1];
    if(vp2.idx<vs||vp1.idx>ve) continue;
    const{ptv,ang}=calcPTV(vp1,vp2);
    const vx1=tx(vp1.idx),vy1=ty(vp1.price);
    const vx2=tx(vp2.idx),vy2=ty(vp2.price);
    const vcol=vp2.type==='H'?'#1a8f60':vp2.type==='L'?'#c04040':COLORS[vi%COLORS.length];

    // Right-angle dashes
    ctx.strokeStyle='rgba(0,0,0,.08)';ctx.lineWidth=1;ctx.setLineDash([2,4]);
    ctx.beginPath();ctx.moveTo(vx1,vy1);ctx.lineTo(vx2,vy1);ctx.lineTo(vx2,vy2);ctx.stroke();ctx.setLineDash([]);

    // Vector line
    ctx.strokeStyle=vcol;ctx.lineWidth=2;
    ctx.beginPath();ctx.moveTo(vx1,vy1);ctx.lineTo(vx2,vy2);ctx.stroke();

    // Arrowhead
    const vdx=vx2-vx1,vdy=vy2-vy1,vlen=Math.sqrt(vdx*vdx+vdy*vdy)||1;
    const vux=vdx/vlen,vuy=vdy/vlen;
    ctx.fillStyle=vcol;ctx.beginPath();ctx.moveTo(vx2,vy2);
    ctx.lineTo(vx2-9*vux+4*vuy,vy2-9*vuy-4*vux);
    ctx.lineTo(vx2-9*vux-4*vuy,vy2-9*vuy+4*vux);
    ctx.closePath();ctx.fill();

    // PTV label
    const vmx=(vx1+vx2)/2,vmy=(vy1+vy2)/2-10;
    const vlbl=`PTV${ptv.toFixed(1)} ${ang.toFixed(0)}deg`;
    ctx.font='bold 9px sans-serif';ctx.textAlign='center';
    ctx.fillStyle='rgba(255,255,255,.85)';ctx.fillRect(vmx-vlbl.length*2.8,vmy-10,vlbl.length*5.6,12);
    ctx.fillStyle=vcol;ctx.fillText(vlbl,vmx,vmy);

    // Angle arc
    const vr=14,va2=Math.atan2(vy1-vy2,vx2-vx1);
    ctx.strokeStyle=vcol;ctx.lineWidth=1.5;
    ctx.beginPath();ctx.arc(vx1,vy1,vr,Math.min(0,va2),Math.max(0,va2));ctx.stroke();
  }

  //  POINT DOTS & LABELS 
  pts.forEach((pt,i)=>{
    if(pt.idx<vs-2||pt.idx>ve+2) return;
    const px=tx(pt.idx),py=ty(pt.price);
    const pcol=pt.type==='H'?'#1a8f60':pt.type==='L'?'#c04040':COLORS[i%COLORS.length];
    ctx.beginPath();ctx.arc(px,py,7,0,2*Math.PI);
    ctx.fillStyle='#0a0c14';ctx.fill();
    ctx.strokeStyle=pcol;ctx.lineWidth=2.5;ctx.stroke();
    ctx.fillStyle=pcol;ctx.font='bold 10px sans-serif';ctx.textAlign='center';
    ctx.fillText(pt.label,px,py+(pt.type==='H'?-14:18));
    ctx.font='9px sans-serif';ctx.fillStyle='rgba(0,0,0,.45)';
    ctx.fillText(Math.round(pt.price).toLocaleString('en-IN'),px,py+(pt.type==='H'?17:-14));
  });

  //  AXES 
  ctx.strokeStyle='#1a1f35';ctx.lineWidth=1.5;
  ctx.beginPath();ctx.moveTo(pad.l,pad.t);ctx.lineTo(pad.l,pad.t+ph);ctx.lineTo(pad.l+pw,pad.t+ph);ctx.stroke();

  // -- SNAP CROSSHAIR -- exact high/low/close preview before clicking
  if(cv._snapPrice!=null&&!fanMode&&!projMoveMode&&!rulerMode){
    const sp=cv._snapPrice,sx=cv._snapX||pad.l,sc=cv._snapCol||'rgba(255,255,255,.6)';
    const syp=ty(sp);
    if(syp>=pad.t&&syp<=pad.t+ph){
      // Horizontal dashed line at snap price
      ctx.strokeStyle=sc;ctx.lineWidth=1.5;ctx.setLineDash([4,3]);
      ctx.beginPath();ctx.moveTo(pad.l,syp);ctx.lineTo(pad.l+pw,syp);ctx.stroke();
      ctx.setLineDash([]);
      // Vertical line at hovered candle
      ctx.strokeStyle=sc;ctx.lineWidth=1;ctx.setLineDash([2,4]);
      ctx.beginPath();ctx.moveTo(sx,pad.t);ctx.lineTo(sx,pad.t+ph);ctx.stroke();
      ctx.setLineDash([]);
      // Price label on left axis
      const plbl=sp.toFixed(2);
      const lw2=plbl.length*6+8;
      ctx.fillStyle=sc;ctx.fillRect(pad.l-lw2-3,syp-8,lw2,15);
      ctx.fillStyle='rgba(0,0,0,.9)';ctx.font='bold 10px sans-serif';ctx.textAlign='right';
      ctx.fillText(plbl,pad.l-5,syp+4);
      // Diamond at intersection
      ctx.fillStyle=sc;
      ctx.beginPath();ctx.moveTo(sx,syp-6);ctx.lineTo(sx+6,syp);
      ctx.lineTo(sx,syp+6);ctx.lineTo(sx-6,syp);ctx.closePath();ctx.fill();
    }
  }

  //  PTV FAN OVERLAY
  if(fanMode&&fanStep>=1){
    drawFanOverlay(ctx,pad,ph,pw,vs,gap,lo,hi,lastCandle);
  }

  //  COWAN ELLIPSE OVERLAY
  if(ellMode&&ellStep>=1){
    drawEllipseOverlay(ctx,pad,ph,pw,vs,gap,lo,hi);
  }

  //  PTV PROJECTOR OVERLAY
  if(projMoveMode&&projStep>=1){
    drawProjMoveOverlay(ctx,pad,tx,ty,ph,pw,vs,gap,lo,hi,lastCandle);
  }

  //  RULER OVERLAY
  if(rulerActive&&rulerMode){
    drawRulerOverlay(ctx,pad,tx,ty,pw,ph);
  }

  updatePanels();
}

// Pixel helpers for use inside event handlers (tx/ty are draw-scope)
function tx2(bar,vs,gap,pad){return pad.l+(bar-vs)*gap+gap/2;}
function ty2(price,lo,hi,ph,pad){return pad.t+(1-(price-lo)/(hi-lo))*ph;}


function drawRulerOverlay(ctx,pad,tx,ty,pw,ph){
  const x0=rulerStartX,y0=rulerStartY;
  const x1=rulerEndX,y1=rulerEndY;
  if(!x0&&!x1) return;

  const cv=document.getElementById('c');
  const lo=cv._lo||0,hi=cv._hi||1;
  const vs=cv._vs||0,gap=cv._gap||1;
  const phh=cv._ph||420;

  // Convert pixel coords to price/bar
  const startPrice=hi-(y0-pad.t)/phh*(hi-lo);
  const endPrice=hi-(y1-pad.t)/phh*(hi-lo);
  const startBar=vs+(x0-pad.l-gap/2)/gap;
  const endBar=vs+(x1-pad.l-gap/2)/gap;

  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const dayDiff=Math.abs(endBar-startBar);
  const hrs=dayDiff*hpd;
  const priceChange=Math.abs(endPrice-startPrice);
  const pp=isLog()?Math.abs(Math.log(Math.max(endPrice,.01)/Math.max(startPrice,.01)))*100:priceChange;
  const ptv=Math.sqrt(pp*pp+hrs*hrs);
  const ang=Math.atan2(pp,hrs)*180/Math.PI;
  const isUp=endPrice>=startPrice;
  // Use the actual start price as the base for projections
  const basePrice=startPrice;

  //  RULER LINE 
  ctx.strokeStyle='rgba(160,102,224,.9)';ctx.lineWidth=2;
  ctx.beginPath();ctx.moveTo(x0,y0);ctx.lineTo(x1,y1);ctx.stroke();

  // Right-angle dashes
  ctx.strokeStyle='rgba(160,102,224,.35)';ctx.lineWidth=1;ctx.setLineDash([3,4]);
  ctx.beginPath();ctx.moveTo(x0,y0);ctx.lineTo(x1,y0);ctx.lineTo(x1,y1);ctx.stroke();
  ctx.setLineDash([]);

  // Dots
  [x0,x1].forEach((x,i)=>{
    const y=i===0?y0:y1;
    ctx.beginPath();ctx.arc(x,y,5,0,2*Math.PI);
    ctx.fillStyle='rgba(160,102,224,.9)';ctx.fill();
  });

  // Angle arc
  const a2=Math.atan2(y0-y1,x1-x0);
  ctx.strokeStyle='rgba(160,102,224,.55)';ctx.lineWidth=1.5;
  ctx.beginPath();ctx.arc(x0,y0,20,Math.min(0,a2),Math.max(0,a2));ctx.stroke();
  ctx.fillStyle='rgba(160,102,224,.7)';ctx.font='bold 9px sans-serif';ctx.textAlign='left';
  ctx.fillText(ang.toFixed(1)+'deg',x0+22,y0+4);

  //  RATIO ROWS 
  // Key ratios to show
  const RULER_RATS=[
    [0.382,'0.382'],[0.5,'0.5'],[0.618,'0.618'],[0.707,'1/sqrt2'],
    [1,'1x'],[1.414,'sqrt2'],[1.618,'PHI'],[1.732,'sqrt3'],
    [2,'2x'],[2.236,'sqrt5'],[2.618,'PHI'],[3,'3x'],[3.14159,'\u03c0']
  ];

  // For each ratio: compute next PTV, time component, price component
  const ratRows=[];
  RULER_RATS.forEach(([rv,rn])=>{
    const np=ptv*rv;
    const nH=Math.cos(ang*Math.PI/180)*np; // time component (hrs)
    const nP=Math.sin(ang*Math.PI/180)*np; // price component (scaled)
    const nDays=(nH/hpd).toFixed(1);
    // Convert price component back to real price
    let upPrice,dnPrice;
    if(isLog()){
      upPrice=basePrice*Math.exp(nP/100);
      dnPrice=basePrice*Math.exp(-nP/100);
    } else {
      upPrice=basePrice+nP;
      dnPrice=basePrice-nP;
    }
    ratRows.push({rv,rn,np,nDays,upPrice,dnPrice});
  });

  //  MAIN INFO BOX 
  const rowH=14;
  const headerH=56; // PTV + angle + days + price delta
  const sepH=8;
  const colHeaderH=16;
  const tableH=ratRows.length*rowH+2;
  const boxW=310;
  const boxH=headerH+sepH+colHeaderH+tableH+10;

  // Position: right of cursor, flip if needed
  let bx=x1+14,by=y1-boxH/2;
  if(bx+boxW>pad.l+pw+60) bx=x1-boxW-14;
  if(bx<pad.l) bx=pad.l+2;
  if(by<pad.t) by=pad.t+4;
  if(by+boxH>pad.t+ph) by=pad.t+ph-boxH-4;

  // Background
  ctx.fillStyle='rgba(255,255,255,.96)';
  ctx.strokeStyle='rgba(160,102,224,.75)';ctx.lineWidth=1.5;
  roundRect(ctx,bx,by,boxW,boxH,8);
  ctx.fill();ctx.stroke();

  //  HEADER: PTV + stats 
  let cy=by+14;
  // PTV large
  ctx.fillStyle='#a066e0';ctx.font='bold 15px sans-serif';ctx.textAlign='left';
  ctx.fillText('PTV = '+ptv.toFixed(2),bx+8,cy);

  // Angle badge
  const angBadge=ang.toFixed(1)+'deg';
  ctx.fillStyle='rgba(160,102,224,.25)';
  roundRect(ctx,bx+boxW-52,cy-12,48,16,4);ctx.fill();
  ctx.fillStyle='#a066e0';ctx.font='bold 10px sans-serif';ctx.textAlign='center';
  ctx.fillText(angBadge,bx+boxW-28,cy);ctx.textAlign='left';

  cy+=14;
  ctx.fillStyle='#666';ctx.font='10px sans-serif';
  ctx.fillText(`Days: ${dayDiff.toFixed(1)}    Hrs: ${hrs.toFixed(0)}`,bx+8,cy);

  cy+=13;
  const pStr=isLog()?pp.toFixed(3)+'%':Math.round(priceChange).toLocaleString('en-IN')+' pts';
  ctx.fillStyle=isUp?'#1a8f60':'#c04040';ctx.font='bold 11px sans-serif';
  ctx.fillText(`${isUp?'UP UP  ':'DN DOWN  '}Price = ${pStr}`,bx+8,cy);

  cy+=13;
  ctx.fillStyle='#444';ctx.font='10px sans-serif';
  ctx.fillText(`From: ${Math.round(basePrice).toLocaleString('en-IN')}    To: ${Math.round(endPrice).toLocaleString('en-IN')}`,bx+8,cy);

  // Separator
  cy+=sepH+2;
  ctx.strokeStyle='rgba(160,102,224,.3)';ctx.lineWidth=1;
  ctx.beginPath();ctx.moveTo(bx+6,cy);ctx.lineTo(bx+boxW-6,cy);ctx.stroke();
  cy+=4;

  // Column headers
  ctx.fillStyle='#555';ctx.font='bold 9px sans-serif';ctx.textAlign='left';
  ctx.fillText('RATIO',bx+8,cy+10);ctx.textAlign='right';
  ctx.fillText('PTV',bx+68,cy+10);
  ctx.fillText('DAYS',bx+110,cy+10);
  ctx.fillText('UP PRICE',bx+195,cy+10);
  ctx.fillText('DN PRICE',bx+boxW-6,cy+10);
  ctx.textAlign='left';
  cy+=colHeaderH;

  // Thin separator under headers
  ctx.strokeStyle='rgba(0,0,0,.08)';ctx.lineWidth=1;
  ctx.beginPath();ctx.moveTo(bx+6,cy-2);ctx.lineTo(bx+boxW-6,cy-2);ctx.stroke();

  //  RATIO ROWS 
  // Find which ratio is nearest to actual PTV drawn
  let nearestRatIdx=0,nearestRatErr=999;
  ratRows.forEach((r,i)=>{const e=Math.abs(r.np-ptv)/Math.max(ptv,1)*100;if(e<nearestRatErr){nearestRatErr=e;nearestRatIdx=i;}});

  ratRows.forEach((r,i)=>{
    const ry=cy+i*rowH;
    // Highlight x1 (same PTV) and key ratios
    const isKey=r.rv===1||r.rv===1.618||r.rv===0.618||r.rv===2||r.rv===1.414;
    if(isKey){
      ctx.fillStyle='rgba(160,102,224,.08)';
      ctx.fillRect(bx+4,ry-10,boxW-8,rowH);
    }

    // Ratio label
    ctx.fillStyle=isKey?'#c4a0f0':'#888';
    ctx.font=isKey?'bold 10px sans-serif':'10px sans-serif';
    ctx.textAlign='left';ctx.fillText(r.rn,bx+8,ry);

    // PTV value
    ctx.fillStyle=isKey?'#f5a623':'#666';
    ctx.textAlign='right';ctx.fillText(r.np.toFixed(1),bx+68,ry);

    // Days
    ctx.fillStyle='#555';
    ctx.fillText(r.nDays+'d',bx+110,ry);

    // Up price — exact decimals for small prices, rounded for large
    const upFmt=r.upPrice>=1000?Math.round(r.upPrice).toLocaleString('en-IN'):r.upPrice.toFixed(2);
    ctx.fillStyle='#1a8f60';
    ctx.fillText(upFmt,bx+205,ry);

    // Down price — exact decimals
    const dnFmt=r.dnPrice>=1000?Math.round(r.dnPrice).toLocaleString('en-IN'):r.dnPrice.toFixed(2);
    ctx.fillStyle='#c04040';
    ctx.fillText(dnFmt,bx+boxW-4,ry);
  });

  // Show nearest ratio match at bottom
  const nr=ratRows[nearestRatIdx];
  if(nr){
    cy+=ratRows.length*rowH+4;
    ctx.strokeStyle='rgba(245,166,35,.3)';ctx.lineWidth=1;
    ctx.beginPath();ctx.moveTo(bx+6,cy-2);ctx.lineTo(bx+boxW-6,cy-2);ctx.stroke();
    ctx.fillStyle='#f5a623';ctx.font='bold 9px sans-serif';ctx.textAlign='left';
    ctx.fillText('Nearest: '+nr.rn+' PTV='+nr.np.toFixed(2)+
      '  UP='+nr.upPrice>=1000?Math.round(nr.upPrice).toLocaleString('en-IN'):nr.upPrice.toFixed(2)+'  DN='+nr.dnPrice>=1000?Math.round(nr.dnPrice).toLocaleString('en-IN'):nr.dnPrice.toFixed(2),bx+6,cy+6);
  }

  ctx.textAlign='left'; // reset
}

function roundRect(ctx,x,y,w,h,r){
  ctx.beginPath();ctx.moveTo(x+r,y);ctx.lineTo(x+w-r,y);
  ctx.quadraticCurveTo(x+w,y,x+w,y+r);ctx.lineTo(x+w,y+h-r);
  ctx.quadraticCurveTo(x+w,y+h,x+w-r,y+h);ctx.lineTo(x+r,y+h);
  ctx.quadraticCurveTo(x,y+h,x,y+h-r);ctx.lineTo(x,y+r);
  ctx.quadraticCurveTo(x,y,x+r,y);ctx.closePath();
}

function toggleProjMove(){
  projMoveMode=!projMoveMode;
  projStep=0; projMoveSelected=false;
  const b=document.getElementById('tog-proj-move');
  b.classList.toggle('on',projMoveMode);
  b.textContent=projMoveMode?' PTV Draw ON':' Move PTV OFF';
  b.style.borderColor='#50c8f5';
  b.style.color=projMoveMode?'#fff':'#50c8f5';
  document.getElementById('c').style.cursor=projMoveMode?'crosshair':'default';
  if(projMoveMode){
    const si=document.getElementById('snapinfo');si.style.display='block';
    si.textContent=' Click POINT 1 (start of PTV) anywhere on chart';
    setTimeout(()=>si.style.display='none',4000);
  } else { draw(); }
}

//  FREE-DRAW PTV PROJECTOR 
// Draws the user-defined PTV vector + all ratio projection lines
function drawProjMoveOverlay(ctx,pad,tx,ty,ph,pw,vs,gap,lo,hi,lastCandle){
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;

  // Convert stored bar/price to pixels
  const x1=pad.l+(projPt1Bar-vs)*gap+gap/2;
  const y1=pad.t+(1-(projPt1Price-lo)/(hi-lo))*ph;

  if(projStep===0) return;

  // pt2: if step=1, use live mouse; if step=2, use locked pt2
  const p2Bar=projStep>=2?projPt2Bar:projLiveBar;
  const p2Price=projStep>=2?projPt2Price:projLivePrice;
  const x2=pad.l+(p2Bar-vs)*gap+gap/2;
  const y2=pad.t+(1-(p2Price-lo)/(hi-lo))*ph;

  //  DRAW THE USER PTV VECTOR 
  // pt1 dot
  ctx.beginPath();ctx.arc(x1,y1,7,0,2*Math.PI);
  ctx.fillStyle='#50c8f5';ctx.fill();
  ctx.strokeStyle='#555';ctx.lineWidth=1.5;ctx.stroke();
  ctx.fillStyle='#000';ctx.font='bold 9px sans-serif';ctx.textAlign='center';
  ctx.fillText('1',x1,y1+3);

  // pt2 dot
  ctx.beginPath();ctx.arc(x2,y2,7,0,2*Math.PI);
  ctx.fillStyle=projStep>=2?'#50c8f5':'rgba(80,200,245,.5)';ctx.fill();
  ctx.strokeStyle='#555';ctx.lineWidth=1.5;ctx.stroke();
  ctx.fillStyle='#000';ctx.font='bold 9px sans-serif';ctx.textAlign='center';
  ctx.fillText('2',x2,y2+3);

  // Vector line pt1->pt2
  ctx.strokeStyle='rgba(80,200,245,.9)';ctx.lineWidth=2.5;
  ctx.beginPath();ctx.moveTo(x1,y1);ctx.lineTo(x2,y2);ctx.stroke();

  // Right-angle dashes
  ctx.strokeStyle='rgba(80,200,245,.3)';ctx.lineWidth=1;ctx.setLineDash([3,4]);
  ctx.beginPath();ctx.moveTo(x1,y1);ctx.lineTo(x2,y1);ctx.lineTo(x2,y2);ctx.stroke();
  ctx.setLineDash([]);

  // Calculate PTV from user-drawn vector
  const dayDiff=Math.abs(p2Bar-p1Bar()||Math.abs(p2Bar-projPt1Bar));
  const rawPriceDiff=Math.abs(p2Price-projPt1Price);
  const hrs=dayDiff*hpd;
  const pp=showBal?rawPriceDiff/balDecimalShift:rawPriceDiff;
  const ptv=Math.sqrt(pp*pp+hrs*hrs);
  const ang=Math.atan2(pp,hrs)*180/Math.PI;
  const isUp=p2Price>=projPt1Price;

  // Live PTV label on the vector
  const midX=(x1+x2)/2,midY=(y1+y2)/2-14;
  const pvlbl=`PTV ${ptv.toFixed(2)}  ${ang.toFixed(1)}deg  ${dayDiff.toFixed(0)}d`;
  ctx.font='bold 10px sans-serif';ctx.textAlign='center';
  ctx.fillStyle='rgba(255,255,255,.9)';ctx.fillRect(midX-pvlbl.length*3,midY-10,pvlbl.length*6,13);
  ctx.fillStyle='#50c8f5';ctx.fillText(pvlbl,midX,midY);

  // Angle arc at pt1
  const arcA=Math.atan2(y1-y2,x2-x1);
  ctx.strokeStyle='rgba(80,200,245,.5)';ctx.lineWidth=1.5;
  ctx.beginPath();ctx.arc(x1,y1,18,Math.min(0,arcA),Math.max(0,arcA));ctx.stroke();

  if(projStep<2||ptv<=0) return;

  //  PROJECT ALL RATIOS FORWARD FROM pt2 
  const KEY_PROJ=[
    [0.382,'0.382'],[0.5,'0.5'],[0.618,'0.618'],[1,'1x'],
    [1.414,'sqrt2'],[1.618,'PHI'],[1.732,'sqrt3'],
    [2,'2x'],[2.236,'sqrt5'],[2.618,'PHI'],[3,'3x'],[3.14159,'\u03c0']
  ];

  const isKeyR=r=>r===1||r===1.618||r===0.618||r===2||r===1.414||r===1.732;

  KEY_PROJ.forEach(([rv,rn])=>{
    const np=ptv*rv;
    const nH=Math.cos(ang*Math.PI/180)*np;
    const nP=Math.sin(ang*Math.PI/180)*np;
    const daysFwd=nH/hpd;
    const targetBar=p2Bar+daysFwd;
    const xe=pad.l+(targetBar-vs)*gap+gap/2;
    const isKey=isKeyR(rv);
    const alpha=rv===1?0.9:isKey?0.7:0.4;
    const lw=isKey?2:0.9;

    // Price targets from pt2
    const upP=p2Price+(showBal?nP*balDecimalShift:nP);
    const dnP=p2Price-(showBal?nP*balDecimalShift:nP);

    [[upP,'rgba(26,200,96,'],[dnP,'rgba(200,64,64,']].forEach(([price,rgb],di)=>{
      const yp=pad.t+(1-(price-lo)/(hi-lo))*ph;
      if(yp<pad.t-30||yp>pad.t+ph+30) return;
      const col=rgb+alpha+')';
      const xe2=Math.min(xe,pad.l+pw+60);

      // Projection line from pt2
      ctx.strokeStyle=col;ctx.lineWidth=lw;
      ctx.setLineDash(isKey?[7,3]:[3,6]);
      ctx.beginPath();ctx.moveTo(x2,y2);ctx.lineTo(xe2,yp);ctx.stroke();
      ctx.setLineDash([]);

      // Horizontal level
      ctx.strokeStyle=col.replace(alpha,alpha*0.25);ctx.lineWidth=0.7;ctx.setLineDash([2,7]);
      ctx.beginPath();ctx.moveTo(x2,yp);ctx.lineTo(xe2,yp);ctx.stroke();
      ctx.setLineDash([]);

      // Vertical date line for key
      if(isKey&&xe>=pad.l&&xe<=pad.l+pw+40){
        ctx.strokeStyle=col.replace(alpha,0.2);ctx.lineWidth=1;ctx.setLineDash([3,6]);
        ctx.beginPath();ctx.moveTo(xe,pad.t);ctx.lineTo(xe,pad.t+ph);ctx.stroke();
        ctx.setLineDash([]);
      }

      // Dot + label
      if(yp>=pad.t-5&&yp<=pad.t+ph+5){
        ctx.beginPath();ctx.arc(xe2,yp,isKey?5:3,0,2*Math.PI);
        ctx.fillStyle=col;ctx.fill();

        // Label box
        const lbl=rn+(di===0?' UP':' DN')+Math.round(price).toLocaleString('en-IN');
        const lx=xe2<pad.l+pw-5?xe2+7:pad.l+pw+2;
        const ly=yp+(di===0?-3:10);
        ctx.fillStyle='rgba(255,255,255,.88)';
        const lw2=lbl.length*5.5;
        ctx.fillRect(lx-1,ly-10,lw2+4,13);
        ctx.fillStyle=col;
        ctx.font=(isKey?'bold ':'')+' 9px sans-serif';
        ctx.textAlign='left';ctx.fillText(lbl,lx,ly);
      }
    });

    // Days label at target bar on x axis
    if(isKey&&xe>=pad.l&&xe<=pad.l+pw+40){
      const dOut=targetBar-lastCandle;
      ctx.fillStyle='rgba(80,200,245,.6)';ctx.font='bold 8px sans-serif';ctx.textAlign='center';
      ctx.fillText(dOut>0?'+'+dOut.toFixed(0)+'d':'d'+targetBar.toFixed(0),
        Math.min(xe,pad.l+pw+40),pad.t+ph+14);
    }
  });

  // Instruction hint if step=2
  ctx.fillStyle='rgba(80,200,245,.5)';ctx.font='11px sans-serif';ctx.textAlign='left';
  ctx.fillText('Right-click = clear  Click chart = move pt2',pad.l+5,pad.t+ph-5);
}

function p1Bar(){ return projPt1Bar; }


// ===========================================================
// PTV FAN — radiate ratio lines from anchor point A
// A = anchor, B = defines base PTV
// Lines from A through all ratio targets in SPACE (price levels)
// These act as support/resistance, not time projections
// ===========================================================
// ============================================================
// COWAN ELLIPSE ENGINE
// PTV = major axis. Minor axis perpendicular at centre.
// Mated ellipse rotated 60deg. Terminus = predicted turning point.
// ============================================================

// ============================================================
// COWAN ELLIPSE — PROPER PRICE-TIME SPACE IMPLEMENTATION
//
// KEY INSIGHT: Ellipse geometry must be computed in balanced
// price-time space where 1 price unit = 1 time unit.
// ONLY THEN does 60 degree rotation and the equilateral
// triangle rule work correctly.
//
// Steps:
// 1. Convert A and B from bar/price to price-time coords
//    ptCoord = (time_hrs, price_scaled)
//    where time_hrs = bar * hpd
//    and price_scaled = price / scaleFactor (balanced mode)
// 2. Compute ellipse in that space
// 3. Rotate mated ellipse 60deg in that space
// 4. Convert terminus back to bar/price for display
// 5. Convert all ellipse points to pixels for drawing
// ============================================================

function toggleEllipse(){
  ellMode=!ellMode; ellStep=0;
  const b=document.getElementById('tog-ellipse');
  b.classList.toggle('on',ellMode);
  b.textContent=ellMode?'🥚 Ellipse ON':'🥚 Ellipse OFF';
  b.style.color=ellMode?'#fff':'#a066e0';
  document.getElementById('ellipse-controls').style.display=ellMode?'block':'none';
  document.getElementById('c').style.cursor=ellMode?'crosshair':'default';
  if(!ellMode){draw();return;}
  // Warn if not in balanced mode
  if(!showBal){
    document.getElementById('ell-status').textContent=
      'WARNING: Enable Balanced 45 mode for accurate ellipse geometry. Click A to start anyway.';
  } else {
    document.getElementById('ell-status').textContent='Click A (swing start)';
  }
}

function clearEllipse(){
  ellStep=0;
  ellABar=ellAPrice=ellBBar=ellBPrice=0;
  ellADate=ellBDate=null;
  document.getElementById('ell-status').textContent='Click A (swing start)';
  document.getElementById('ell-lock-sf-btn').style.display='none';
  document.getElementById('ell-continue-btn').style.display='none';
  document.getElementById('ell-copy-btn').style.display='none';
  document.getElementById('ell-paste-status').style.display='none';
  ellPasteMode=false;
  document.getElementById('ell-lock-sf-status').style.display='none';
  draw();
}

// Lock the auto-SF from the current ellipse A→B swing into the Square Chart panel
// This squares the ENTIRE chart to this swing — correct Cowan geometry
function lockSFFromEllipse(){
  if(ellStep<2){alert('Place both A and B points first');return;}
  const days=Math.abs(ellBBar-ellABar)||1;
  const pp=Math.abs(ellBPrice-ellAPrice)||1;
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  // SF = price_diff / (bars * hpd) makes price and time axes equal
  const sf=pp/(days*hpd);
  const sfRounded=parseFloat(sf.toFixed(4));

  // Set Square Chart panel
  document.getElementById('sq-time').value=sfRounded;
  document.getElementById('sq-price').value=1;
  updateSquare();

  // Also sync to Balanced 45° engine
  balScaleFactor=sfRounded;
  if(showBal) draw();

  // Update status
  const btn=document.getElementById('ell-lock-sf-btn');
  const stat=document.getElementById('ell-lock-sf-status');
  btn.style.background='#1a8f60';btn.style.borderColor='#1a8f60';
  btn.textContent='✅ SF Locked';
  stat.style.display='inline';
  stat.textContent='SF='+sfRounded+' locked → chart squared to this swing ('+days+' bars × '+Math.round(pp)+' pts)';

  // Flash confirmation
  setTimeout(()=>{
    btn.style.background='#c07800';btn.style.borderColor='#c07800';
    btn.textContent='🔒 Lock SF → Square Chart';
  },2500);
}

// Continue chain: new A = current B, new B = terminus T of mated ellipse
// This lets you chain ellipses like the CycleTrader mated pairs wave
function continueEllipseChain(){
  if(ellStep<2){alert('Place both A and B points first');return;}

  // Compute terminus exactly as drawEllipseOverlay does
  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;
  const dirSel=document.getElementById('ell-mated-rot').value;

  // Get SF same way as overlay
  let SF=1;
  const geomSF=getGeomSF();
  if(geomSF!==1) SF=geomSF;
  else if(showBal&&balScaleFactor>0) SF=balScaleFactor;
  else {
    const dayD=Math.abs(ellBBar-ellABar)||1;
    const priceD=Math.abs(ellBPrice-ellAPrice)||1;
    SF=priceD/(dayD*hpd);
  }

  // Balanced space coords
  const bsA={t:ellABar*hpd, p:SF>0?ellAPrice/SF:ellAPrice};
  const bsB={t:ellBBar*hpd, p:SF>0?ellBPrice/SF:ellBPrice};
  const majT=bsB.t-bsA.t, majP=bsB.p-bsA.p;
  const majLen=Math.sqrt(majT*majT+majP*majP);
  const axAng=Math.atan2(majP,majT);

  // Angle from selected system
  const ang = ellAngle==='all'?60:Number(ellAngle);
  const dir = dirSel==='1'?1:-1;
  const matedAng = axAng + dir*ang*Math.PI/180;

  // Terminus in balanced space
  const termT = bsB.t + majLen*Math.cos(matedAng);
  const termP = bsB.p + majLen*Math.sin(matedAng);

  // Convert back to bar + price
  const termBar   = Math.round(termT/hpd);
  const termPrice = termP*SF;

  // Clamp termBar to valid range (can be in future)
  const safeTermBar = Math.max(0, Math.min(candles.length+200, termBar));

  // Set new A = current B, new B = terminus
  ellABar   = ellBBar;
  ellAPrice = ellBPrice;
  ellADate  = ellBDate;
  ellBBar   = safeTermBar;
  ellBPrice = termPrice;
  ellBDate  = safeTermBar<candles.length ? candles[safeTermBar]?.t||null : null;
  ellStep   = 2;

  // Update status
  const dFromNow=safeTermBar-(candles.length-1);
  const dStr=(dFromNow>0?'+':'')+Math.round(dFromNow)+'d';
  document.getElementById('ell-status').textContent=
    'Chained → new A=old B, new B=T '+Math.round(termPrice).toLocaleString('en-IN')+' ('+dStr+')';

  document.getElementById('ell-lock-sf-btn').style.display='inline';
  document.getElementById('ell-continue-btn').style.display='inline';
  document.getElementById('ell-lock-sf-status').style.display='none';
  draw();
}

// ── COPY ELLIPSE ─────────────────────────────────────────────
function copyEllipse(){
  if(ellStep<2){alert('Place both A and B first');return;}
  // Store EXACTLY what A and B are — bar index difference and price difference
  // Nothing fancy. Paste will reproduce these exact differences from a new A point.
  ellCopied = {
    dBar:  ellBBar   - ellABar,    // how many bars from A to B
    dPrice: ellBPrice - ellAPrice, // how many price points from A to B
    minorR: parseFloat(document.getElementById('ell-minor-ratio').value)||0.618
  };
  const btn=document.getElementById('ell-copy-btn');
  btn.style.background='#1a7a50';
  btn.textContent='✅ Copied! ('+ellCopied.dBar+'bars, '+Math.round(ellCopied.dPrice)+'pts)';
  document.getElementById('ell-paste-btn').style.display='inline';
  setTimeout(()=>{btn.style.background='#1a8f60';btn.textContent='📋 Copy Ellipse';},2500);
}

// ── PASTE ELLIPSE ────────────────────────────────────────────
// Step 1: click to place new A point
// Step 2: move mouse to rotate (length stays the same)
// Step 3: click to confirm
function pasteEllipse(){
  if(!ellCopied){alert('Copy an ellipse first');return;}
  ellPasteMode=true;
  ellPasteBar=0; ellPastePrice=0;
  document.getElementById('ell-paste-status').style.display='inline';
  document.getElementById('ell-paste-status').textContent='Click chart for new A point';
  document.getElementById('c').style.cursor='crosshair';
}

// Convert bar index + price to balanced price-time space
// In balanced space: unit = 1 price point = 1 trading hour
// So both axes are in the same "unit" and geometry is correct
function toBalancedSpace(bar, price, hpd, scaleFactor){
  // Time axis: bar * hpd gives trading hours
  const t = bar * hpd;
  // Price axis: divide by scaleFactor to bring to same scale as time
  // scaleFactor = price_range / time_range (from balanced 45 calculation)
  const p = scaleFactor > 0 ? price / scaleFactor : price;
  return {t, p};
}

// Convert balanced space coords back to bar + price
function fromBalancedSpace(t, p, hpd, scaleFactor){
  const bar = t / hpd;
  const price = p * scaleFactor;
  return {bar, price};
}

// drawEllipseBalanced is now inlined inside drawEllipseOverlay using balToPx closure
// This stub kept for reference only — NOT called anymore
function drawEllipseBalanced_UNUSED(){}

function drawEllipseOverlay(ctx,pad,ph,pw,vs,gap,lo,hi){
  if(ellStep===0 && !(ellPasteMode && ellCopied && ellPasteBar!==0)) return;

  // ── PASTE PREVIEW ───────────────────────────────────────
  if(ellPasteMode && ellCopied && ellPasteBar!==0){
    const {dBar, dPrice, minorR:mR} = ellCopied;
    const origLen = Math.sqrt(dBar**2 + dPrice**2);
    // Rotated B position
    const newDBar   = origLen * Math.cos(ellPasteAngle);
    const newDPrice = origLen * Math.sin(ellPasteAngle);
    const bBar2   = ellPasteBar  + newDBar;
    const bPrice2 = ellPastePrice + newDPrice;

    // Inline toPx
    const px2=(bar,price)=>({
      x:pad.l+(bar-vs)*gap+gap/2,
      y:pad.t+(1-(price-lo)/(hi-lo))*ph
    });
    const pxA2=px2(ellPasteBar, ellPastePrice);
    const pxB2=px2(bBar2, bPrice2);

    // Ellipse geometry — same as main ellipse drawing
    // Use the same SF/hpd that drawEllipseOverlay would use
    const hpd2=parseFloat(document.getElementById('hpd').value)||6.5;
    let SF2=getGeomSF();
    if(SF2===1&&showBal&&balScaleFactor>0) SF2=balScaleFactor;
    else if(SF2===1){
      const dd=Math.abs(newDBar)||1, pp=Math.abs(newDPrice)||1;
      SF2=pp/(dd*hpd2);
    }
    const bsA2={t:ellPasteBar*hpd2, p:ellPastePrice/SF2};
    const bsB2={t:bBar2*hpd2,       p:bPrice2/SF2};
    const mT=bsB2.t-bsA2.t, mP=bsB2.p-bsA2.p;
    const mLen=Math.sqrt(mT*mT+mP*mP);
    if(mLen>0.001){
      const sMaj2=mLen/2, sMin2=sMaj2*mR;
      const cT2=(bsA2.t+bsB2.t)/2, cP2=(bsA2.p+bsB2.p)/2;
      const ang2=Math.atan2(mP,mT);
      const bp2=(t,p)=>px2(t/hpd2, p*SF2);
      ctx.save();
      ctx.beginPath();ctx.rect(pad.l,pad.t,pw,ph);ctx.clip();
      ctx.beginPath();
      for(let i=0;i<=240;i++){
        const th=2*Math.PI*i/240;
        const ex=sMaj2*Math.cos(th),ey=sMin2*Math.sin(th);
        const rx=ex*Math.cos(ang2)-ey*Math.sin(ang2);
        const ry=ex*Math.sin(ang2)+ey*Math.cos(ang2);
        const pt=bp2(cT2+rx,cP2+ry);
        i===0?ctx.moveTo(pt.x,pt.y):ctx.lineTo(pt.x,pt.y);
      }
      ctx.closePath();
      ctx.fillStyle='rgba(24,95,165,.06)';ctx.fill();
      ctx.strokeStyle='rgba(24,95,165,.75)';ctx.lineWidth=2;ctx.setLineDash([6,3]);ctx.stroke();
      ctx.setLineDash([]);ctx.restore();
    }

    // A dot
    ctx.beginPath();ctx.arc(pxA2.x,pxA2.y,9,0,2*Math.PI);
    ctx.fillStyle='rgba(24,95,165,.85)';ctx.fill();
    ctx.strokeStyle='#fff';ctx.lineWidth=2;ctx.stroke();
    ctx.fillStyle='#fff';ctx.font='bold 10px sans-serif';ctx.textAlign='center';
    ctx.fillText('A',pxA2.x,pxA2.y+4);
    // B dot
    ctx.beginPath();ctx.arc(pxB2.x,pxB2.y,7,0,2*Math.PI);
    ctx.fillStyle='rgba(24,95,165,.7)';ctx.fill();
    ctx.strokeStyle='#fff';ctx.lineWidth=1.5;ctx.stroke();
    ctx.fillStyle='#fff';ctx.font='bold 9px sans-serif';ctx.textAlign='center';
    ctx.fillText('B',pxB2.x,pxB2.y+3);
    // Axis line
    ctx.strokeStyle='rgba(24,95,165,.4)';ctx.lineWidth=1.5;ctx.setLineDash([4,3]);
    ctx.beginPath();ctx.moveTo(pxA2.x,pxA2.y);ctx.lineTo(pxB2.x,pxB2.y);ctx.stroke();
    ctx.setLineDash([]);
    // Label
    ctx.fillStyle='rgba(24,95,165,.95)';ctx.font='bold 10px sans-serif';ctx.textAlign='left';
    ctx.fillText('↻ '+(ellPasteAngle*180/Math.PI).toFixed(0)+'°  click to confirm',pxA2.x+12,pxA2.y-10);
    return;
  }

  const hpd     = parseFloat(document.getElementById('hpd').value)||6.5;
  const minorR  = parseFloat(document.getElementById('ell-minor-ratio').value)||0.618;
  const showMated = document.getElementById('ell-show-mated').checked;
  const showTri   = document.getElementById('ell-show-triangle').checked;
  const showTerm  = document.getElementById('ell-show-terminus').checked;

  // Helper: find bar from stored date — resolve stored bar index to actual candle
  function bfd(date,fb){
    if(!date||!candles.length) return fb;
    let best=fb,bd=Infinity;
    for(let i=Math.max(0,fb-30);i<Math.min(candles.length,fb+30);i++){
      const d=Math.abs(candles[i].t.getTime()-date.getTime());
      if(d<bd){bd=d;best=i;}
    }
    return best;
  }

  // Resolve A and B bar indices FIRST — used consistently everywhere below
  const aBar   = bfd(ellADate, ellABar);
  const bPrice = ellStep>=2 ? ellBPrice : ellLivePrice;
  const bBar   = ellStep>=2 ? bfd(ellBDate, ellBBar) : ellLiveBar;

  // Get scale factor — priority: Square panel > Balanced mode > Auto from swing
  // IMPORTANT: use resolved aBar/bBar (not raw ellABar/ellBBar) for consistency
  let SF = 1;
  const geomSF = getGeomSF();
  if(geomSF !== 1){
    SF = geomSF;
  } else if(showBal && balScaleFactor>0){
    SF = balScaleFactor;
  } else {
    // Auto-compute from A→B swing using RESOLVED bar indices
    const dayD = Math.abs(bBar - aBar)||1;
    const priceD = Math.abs(bPrice - ellAPrice)||1;
    SF = priceD / (dayD * hpd);
  }

  // Helper: bar/price → pixel
  function toPx(bar,price){
    return {
      x: pad.l+(bar-vs)*gap+gap/2,
      y: pad.t+(1-(price-lo)/(hi-lo))*ph
    };
  }

  // Helper: balanced space point → pixel
  function balToPx(t,p){
    const bar   = t/hpd;
    const price = p*SF;
    return toPx(bar,price);
  }

  // Convert A and B to balanced price-time space
  const bsA = toBalancedSpace(aBar,  ellAPrice, hpd, SF);
  const bsB = toBalancedSpace(bBar,  bPrice,    hpd, SF);

  // A and B in pixels (for dots and labels)
  const pxA = toPx(aBar, ellAPrice);
  const pxB = toPx(bBar, bPrice);

  // ── A DOT ───────────────────────────────────────────────
  ctx.beginPath();ctx.arc(pxA.x,pxA.y,8,0,2*Math.PI);
  ctx.fillStyle='#a066e0';ctx.fill();
  ctx.strokeStyle='#555';ctx.lineWidth=1.5;ctx.stroke();
  ctx.fillStyle='#fff';ctx.font='bold 10px sans-serif';ctx.textAlign='center';
  ctx.fillText('A',pxA.x,pxA.y+4);

  if(ellStep<1) return;

  // ── B DOT ───────────────────────────────────────────────
  ctx.beginPath();ctx.arc(pxB.x,pxB.y,8,0,2*Math.PI);
  ctx.fillStyle=ellStep>=2?'#a066e0':'rgba(160,102,224,.4)';ctx.fill();
  ctx.strokeStyle='#555';ctx.lineWidth=1.5;ctx.stroke();
  ctx.fillStyle='#fff';ctx.font='bold 10px sans-serif';ctx.textAlign='center';
  ctx.fillText('B',pxB.x,pxB.y+4);

  // A→B axis line (pixels)
  ctx.strokeStyle='rgba(160,102,224,.7)';ctx.lineWidth=2;ctx.setLineDash([]);
  ctx.beginPath();ctx.moveTo(pxA.x,pxA.y);ctx.lineTo(pxB.x,pxB.y);ctx.stroke();

  if(ellStep<2) return;

  // ── ELLIPSE GEOMETRY — ZOOM-INVARIANT ───────────────────
  // KEY INSIGHT: We compute geometry in balanced data space,
  // then convert each point: balanced → (bar,price) → pixel
  // This means zoom/pan only repositions the ellipse — never distorts it.
  // The shape is determined 100% by the data (A bar, A price, B bar, B price, SF).

  const majT = bsB.t - bsA.t;  // time component in balanced space
  const majP = bsB.p - bsA.p;  // price component in balanced space
  const majLen = Math.sqrt(majT*majT + majP*majP);
  if(majLen<0.001) return;

  const sMaj   = majLen/2;
  const sMin   = sMaj * minorR;
  const cT     = (bsA.t + bsB.t)/2;
  const cP     = (bsA.p + bsB.p)/2;
  const axAng  = Math.atan2(majP, majT);
  const perpAng= axAng + Math.PI/2;

  const dayDiff = Math.abs(bBar-aBar);
  const ppRaw   = Math.abs(bPrice-ellAPrice);
  const ptv     = Math.sqrt(ppRaw*ppRaw + (dayDiff*hpd)**2);

  // Balanced → data → pixel (the ONLY correct conversion path)
  function balPt(t, p){ return toPx(t/hpd, p*SF); }

  // Generic ellipse draw — all params in balanced space
  function drawEll(ecT,ecP,eMajT,eMajP,eSMin,col,lw,dash){
    const eAng=Math.atan2(eMajP,eMajT);
    const eMaj=Math.sqrt(eMajT*eMajT+eMajP*eMajP);
    ctx.save();
    ctx.beginPath();ctx.rect(pad.l,pad.t,pw,ph);ctx.clip();
    ctx.beginPath();
    for(let i=0;i<=240;i++){
      const th=2*Math.PI*i/240;
      const ex=eMaj*Math.cos(th), ey=eSMin*Math.sin(th);
      const rx=ex*Math.cos(eAng)-ey*Math.sin(eAng);
      const ry=ex*Math.sin(eAng)+ey*Math.cos(eAng);
      const pt=balPt(ecT+rx,ecP+ry);
      i===0?ctx.moveTo(pt.x,pt.y):ctx.lineTo(pt.x,pt.y);
    }
    ctx.closePath();
    if(dash) ctx.setLineDash(dash);
    ctx.strokeStyle=col;ctx.lineWidth=lw;ctx.stroke();
    ctx.setLineDash([]);ctx.restore();
  }

  // ── PRIMARY ELLIPSE — fill then stroke ──────────────────
  (function(){
    const eAng=axAng, eMaj=sMaj;
    ctx.save();
    ctx.beginPath();ctx.rect(pad.l,pad.t,pw,ph);ctx.clip();
    ctx.beginPath();
    for(let i=0;i<=240;i++){
      const th=2*Math.PI*i/240;
      const ex=eMaj*Math.cos(th), ey=sMin*Math.sin(th);
      const rx=ex*Math.cos(eAng)-ey*Math.sin(eAng);
      const ry=ex*Math.sin(eAng)+ey*Math.cos(eAng);
      const pt=balPt(cT+rx,cP+ry);
      i===0?ctx.moveTo(pt.x,pt.y):ctx.lineTo(pt.x,pt.y);
    }
    ctx.closePath();
    ctx.fillStyle='rgba(140,80,220,.06)';ctx.fill();
    ctx.strokeStyle='rgba(140,80,220,.95)';ctx.lineWidth=2.5;ctx.stroke();
    ctx.restore();
  })();

  // Minor axis, centre dot, endpoints
  const pxM1=balPt(cT+sMin*Math.cos(perpAng), cP+sMin*Math.sin(perpAng));
  const pxM2=balPt(cT-sMin*Math.cos(perpAng), cP-sMin*Math.sin(perpAng));
  const pxC =balPt(cT,cP);
  ctx.strokeStyle='rgba(140,80,220,.3)';ctx.lineWidth=1;ctx.setLineDash([4,4]);
  ctx.beginPath();ctx.moveTo(pxM1.x,pxM1.y);ctx.lineTo(pxM2.x,pxM2.y);ctx.stroke();
  ctx.setLineDash([]);
  ctx.beginPath();ctx.arc(pxC.x,pxC.y,3,0,2*Math.PI);
  ctx.fillStyle='rgba(140,80,220,.6)';ctx.fill();
  [pxM1,pxM2].forEach(p=>{
    ctx.beginPath();ctx.arc(p.x,p.y,4,0,2*Math.PI);
    ctx.fillStyle='rgba(140,80,220,.4)';ctx.fill();
  });

  // PTV label
  ctx.fillStyle='rgba(255,255,255,.94)';ctx.fillRect(pxC.x-64,pxC.y-22,128,14);
  ctx.fillStyle='#7040b0';ctx.font='bold 10px sans-serif';ctx.textAlign='center';
  ctx.fillText('PTV='+ptv.toFixed(2)+(showBal?' (bal)':''),pxC.x,pxC.y-11);

  // ── S/R CROSS LINES through ellipse centre ───────────────
  // From Image 2: the dashed cross (major axis + minor axis) = support & resistance
  // These are the KEY trading levels from Cowan's ellipse
  if(document.getElementById('ell-show-cross').checked){
    ctx.save();
    ctx.beginPath();ctx.rect(pad.l-60,pad.t,pw+120,ph);ctx.clip();
    ctx.strokeStyle='rgba(100,60,200,.35)';ctx.lineWidth=1;ctx.setLineDash([4,5]);

    // Major axis line — extended through full ellipse (A→B direction, through centre)
    const majEx = sMaj*1.05; // slightly beyond ellipse edge
    const majA1 = balPt(cT + majEx*Math.cos(axAng), cP + majEx*Math.sin(axAng));
    const majA2 = balPt(cT - majEx*Math.cos(axAng), cP - majEx*Math.sin(axAng));
    ctx.beginPath();ctx.moveTo(majA1.x,majA1.y);ctx.lineTo(majA2.x,majA2.y);ctx.stroke();

    // Minor axis line — perpendicular through centre
    const minEx = sMin*1.05;
    const minA1 = balPt(cT + minEx*Math.cos(perpAng), cP + minEx*Math.sin(perpAng));
    const minA2 = balPt(cT - minEx*Math.cos(perpAng), cP - minEx*Math.sin(perpAng));
    ctx.beginPath();ctx.moveTo(minA1.x,minA1.y);ctx.lineTo(minA2.x,minA2.y);ctx.stroke();

    ctx.setLineDash([]);ctx.restore();

    // Price level tags on Y axis for the 4 cardinal points
    const cardinals = [
      {t:cT+sMaj*Math.cos(axAng),  p:cP+sMaj*Math.sin(axAng),  lbl:'B'},
      {t:cT-sMaj*Math.cos(axAng),  p:cP-sMaj*Math.sin(axAng),  lbl:'A'},
      {t:cT+sMin*Math.cos(perpAng),p:cP+sMin*Math.sin(perpAng),lbl:'R1'},
      {t:cT-sMin*Math.cos(perpAng),p:cP-sMin*Math.sin(perpAng),lbl:'S1'},
    ];
    cardinals.forEach(c=>{
      const pr=(c.p)*SF;
      if(pr<lo||pr>hi) return;
      const yy=ty(pr);
      ctx.fillStyle='rgba(100,60,200,.55)';
      ctx.fillRect(pad.l-38,yy-7,36,13);
      ctx.fillStyle='#fff';ctx.font='bold 8px sans-serif';ctx.textAlign='right';
      ctx.fillText(c.lbl+' '+Math.round(pr).toLocaleString('en-IN'),pad.l-3,yy+4);
    });
  }

  if(!showMated) return;

  // ── MATED ELLIPSE — angle system + direction ─────────────
  // ellAngle = 45/60/72/90/'all'  — sets the rotation angle
  // ell-mated-rot = -1(CW) / 1(CCW) / both
  const dirSel = document.getElementById('ell-mated-rot').value;
  const showMinorPts = document.getElementById('ell-show-minor-pts').checked;

  // Colors per angle system
  const angColors={
    60: {col:'rgba(50,190,240,.8)', fill:'rgba(50,190,240,.05)', tcol:'#0088bb'},
    72: {col:'rgba(240,100,60,.8)', fill:'rgba(240,100,60,.05)', tcol:'#c04010'},
    45: {col:'rgba(160,80,220,.8)', fill:'rgba(160,80,220,.05)', tcol:'#7030b0'},
    90: {col:'rgba(40,180,120,.8)', fill:'rgba(40,180,120,.05)', tcol:'#1a7a50'},
  };
  // CCW gets a lighter shade
  const angColorsCCW={
    60: {col:'rgba(80,220,120,.8)', fill:'rgba(80,220,120,.05)', tcol:'#22aa60'},
    72: {col:'rgba(220,160,40,.8)', fill:'rgba(220,160,40,.05)', tcol:'#aa7700'},
    45: {col:'rgba(220,80,160,.8)', fill:'rgba(220,80,160,.05)', tcol:'#b02070'},
    90: {col:'rgba(180,120,40,.8)', fill:'rgba(180,120,40,.05)', tcol:'#8a5500'},
  };

  // Build list of {deg, col, fill, tcol, label} to draw
  const angList = ellAngle==='all' ? [60,72,45,90] : [Number(ellAngle)];
  let toDraw=[];
  angList.forEach(ang=>{
    const cw  ={deg:-ang, ...angColors[ang],    label:'🔺'+ang+'°↻'};
    const ccw ={deg:+ang, ...angColorsCCW[ang], label:'🔺'+ang+'°↺'};
    if(dirSel==='-1'||dirSel==='both') toDraw.push(cw);
    if(dirSel==='1' ||dirSel==='both') toDraw.push(ccw);
  });
  if(!toDraw.length) toDraw=[{deg:-60,...angColors[60],label:'60°↻'}];

  // Helper: draw one mated ellipse + terminus
  function drawMated(cfg){
    const matedAng = axAng + cfg.deg*Math.PI/180;
    const mMajT = majLen*Math.cos(matedAng);
    const mMajP = majLen*Math.sin(matedAng);
    const termT = bsB.t + mMajT;
    const termP = bsB.p + mMajP;
    const mcT   = (bsB.t + termT)/2;
    const mcP   = (bsB.p + termP)/2;

    // Mated ellipse — same sMaj/sMin as primary, but rotated to matedAng
    // Semi-minor is perpendicular to matedAng (not axAng)
    const mSMaj = sMaj;
    const mSMin = sMin; // sMin = sMaj * minorR, same ratio
    (function(){
      ctx.save();
      ctx.beginPath();ctx.rect(pad.l,pad.t,pw,ph);ctx.clip();
      ctx.beginPath();
      for(let i=0;i<=240;i++){
        const th=2*Math.PI*i/240;
        // Local frame: major along matedAng, minor perpendicular to matedAng
        const ex=mSMaj*Math.cos(th);
        const ey=mSMin*Math.sin(th);
        const rx=ex*Math.cos(matedAng)-ey*Math.sin(matedAng);
        const ry=ex*Math.sin(matedAng)+ey*Math.cos(matedAng);
        const pt=balPt(mcT+rx, mcP+ry);
        i===0?ctx.moveTo(pt.x,pt.y):ctx.lineTo(pt.x,pt.y);
      }
      ctx.closePath();
      ctx.fillStyle=cfg.fill;ctx.fill();
      ctx.strokeStyle=cfg.col;ctx.lineWidth=2;
      ctx.setLineDash([7,4]);ctx.stroke();ctx.setLineDash([]);ctx.restore();
    })();

    // B→Terminus axis
    const pxTerm=balPt(termT,termP);
    ctx.strokeStyle=cfg.col.replace('.8','.45');ctx.lineWidth=1.5;ctx.setLineDash([6,3]);
    ctx.beginPath();ctx.moveTo(pxB.x,pxB.y);ctx.lineTo(pxTerm.x,pxTerm.y);ctx.stroke();
    ctx.setLineDash([]);

    // Mated minor axis reaction points
    if(showMinorPts){
      const mPerpAng=matedAng+Math.PI/2;
      const mm1=balPt(mcT+sMin*Math.cos(mPerpAng),mcP+sMin*Math.sin(mPerpAng));
      const mm2=balPt(mcT-sMin*Math.cos(mPerpAng),mcP-sMin*Math.sin(mPerpAng));
      [mm1,mm2].forEach(p=>{
        ctx.beginPath();ctx.arc(p.x,p.y,5,0,2*Math.PI);
        ctx.fillStyle=cfg.col.replace('.8','.5');ctx.fill();
        ctx.strokeStyle='#fff';ctx.lineWidth=1;ctx.stroke();
      });
    }

    // Triangle A→B→T
    if(showTri){
      ctx.save();
      ctx.beginPath();ctx.rect(pad.l-50,pad.t,pw+140,ph);ctx.clip();
      ctx.strokeStyle=cfg.col.replace('.8','.4');ctx.lineWidth=1.5;ctx.setLineDash([5,4]);
      ctx.beginPath();ctx.moveTo(pxA.x,pxA.y);ctx.lineTo(pxB.x,pxB.y);ctx.lineTo(pxTerm.x,pxTerm.y);ctx.closePath();
      ctx.stroke();ctx.setLineDash([]);
      ctx.fillStyle=cfg.fill;ctx.fill();
      ctx.restore();
      ctx.fillStyle=cfg.tcol;ctx.font='bold 9px sans-serif';ctx.textAlign='left';
      ctx.fillText(cfg.label,pxB.x+8,pxB.y-4+(toDraw.indexOf(cfg)*11));
    }

    // Terminus point
    if(showTerm){
      const termBar  =termT/hpd;
      const termPrice=termP*SF;
      const dFromNow =termBar-(candles.length-1);
      const tPStr=termPrice>=100?Math.round(termPrice).toLocaleString('en-IN'):termPrice.toFixed(2);
      const dStr=(dFromNow>0?'+':'')+Math.round(dFromNow)+'d';

      ctx.shadowColor=cfg.tcol;ctx.shadowBlur=16;
      ctx.beginPath();ctx.arc(pxTerm.x,pxTerm.y,9,0,2*Math.PI);
      ctx.fillStyle=cfg.tcol;ctx.fill();
      ctx.shadowBlur=0;
      ctx.strokeStyle='#666';ctx.lineWidth=1.5;ctx.stroke();
      ctx.fillStyle='#fff';ctx.font='bold 9px sans-serif';ctx.textAlign='center';
      ctx.fillText('T',pxTerm.x,pxTerm.y+3);

      // Crosshairs
      ctx.strokeStyle=cfg.col.replace('.8','.15');ctx.lineWidth=0.8;ctx.setLineDash([2,5]);
      ctx.beginPath();ctx.moveTo(pad.l,pxTerm.y);ctx.lineTo(pxTerm.x,pxTerm.y);ctx.stroke();
      ctx.beginPath();ctx.moveTo(pxTerm.x,pad.t);ctx.lineTo(pxTerm.x,pad.t+ph);ctx.stroke();
      ctx.setLineDash([]);

      // Y-axis tag
      ctx.fillStyle=cfg.tcol;ctx.fillRect(pad.l-52,pxTerm.y-8,49,16);
      ctx.fillStyle='#fff';ctx.font='bold 9px sans-serif';ctx.textAlign='right';
      ctx.fillText(tPStr,pad.l-4,pxTerm.y+4);

      // Info box (offset per rotation so they don't overlap)
      const idx=toDraw.indexOf(cfg);
      const tlbl=cfg.label+' T: '+tPStr+'  ('+dStr+')';
      const lx=Math.min(pxTerm.x+14,pad.l+pw-200);
      const ly=pxTerm.y-14+(idx*36);
      ctx.fillStyle='rgba(255,255,255,.96)';ctx.fillRect(lx-4,ly-13,tlbl.length*6.4+10,30);
      ctx.strokeStyle=cfg.tcol;ctx.lineWidth=1.5;
      ctx.strokeRect(lx-4,ly-13,tlbl.length*6.4+10,30);
      ctx.fillStyle=cfg.tcol;ctx.font='bold 11px sans-serif';ctx.textAlign='left';
      ctx.fillText(tlbl,lx,ly);
      ctx.fillStyle='#888';ctx.font='9px sans-serif';
      ctx.fillText('Terminus — predicted turning point',lx,ly+12);
    }
  }

  toDraw.forEach(drawMated);

  // ── CROSS LINES for MATED ELLIPSE (S/R from mated centre) ──
  if(document.getElementById('ell-show-cross').checked){
    toDraw.forEach(cfg=>{
      const matedAng2 = axAng + cfg.deg*Math.PI/180;
      const mMajT2 = majLen*Math.cos(matedAng2);
      const mMajP2 = majLen*Math.sin(matedAng2);
      const termT2 = bsB.t + mMajT2;
      const termP2 = bsB.p + mMajP2;
      const mcT2 = (bsB.t + termT2)/2;
      const mcP2 = (bsB.p + termP2)/2;
      const mPerpAng2 = matedAng2 + Math.PI/2;

      ctx.save();
      ctx.beginPath();ctx.rect(pad.l-60,pad.t,pw+120,ph);ctx.clip();
      ctx.strokeStyle=cfg.col.replace('.8','.3');ctx.lineWidth=1;ctx.setLineDash([4,5]);

      // Major axis of mated ellipse
      const mMajEnd1=balPt(mcT2+sMaj*Math.cos(matedAng2),mcP2+sMaj*Math.sin(matedAng2));
      const mMajEnd2=balPt(mcT2-sMaj*Math.cos(matedAng2),mcP2-sMaj*Math.sin(matedAng2));
      ctx.beginPath();ctx.moveTo(mMajEnd1.x,mMajEnd1.y);ctx.lineTo(mMajEnd2.x,mMajEnd2.y);ctx.stroke();

      // Minor axis of mated ellipse
      const mMinEnd1=balPt(mcT2+sMin*Math.cos(mPerpAng2),mcP2+sMin*Math.sin(mPerpAng2));
      const mMinEnd2=balPt(mcT2-sMin*Math.cos(mPerpAng2),mcP2-sMin*Math.sin(mPerpAng2));
      ctx.beginPath();ctx.moveTo(mMinEnd1.x,mMinEnd1.y);ctx.lineTo(mMinEnd2.x,mMinEnd2.y);ctx.stroke();

      ctx.setLineDash([]);ctx.restore();
    });
  }

  // ── CHAIN ELLIPSE — terminus becomes next A ──────────────
  // From Image 1: mated pairs chain — each ellipse's end = next start
  // Draw one more generation: terminus T → T2
  if(document.getElementById('ell-show-chain').checked && toDraw.length>0){
    const cfg=toDraw[0]; // use first direction for chain
    const matedAng3 = axAng + cfg.deg*Math.PI/180;
    const termT3 = bsB.t + majLen*Math.cos(matedAng3);
    const termP3 = bsB.p + majLen*Math.sin(matedAng3);

    // Chain ellipse: starts at T (terminus), same angle, same size
    const chainAng = matedAng3; // continues in same direction
    const chain2T = termT3 + majLen*Math.cos(chainAng);
    const chain2P = termP3 + majLen*Math.sin(chainAng);
    const chainCT = (termT3 + chain2T)/2;
    const chainCP = (termP3 + chain2P)/2;

    ctx.save();
    ctx.beginPath();ctx.rect(pad.l,pad.t,pw,ph);ctx.clip();
    ctx.beginPath();
    for(let i=0;i<=240;i++){
      const th=2*Math.PI*i/240;
      const ex=sMaj*Math.cos(th), ey=sMin*Math.sin(th);
      const rx=ex*Math.cos(chainAng)-ey*Math.sin(chainAng);
      const ry=ex*Math.sin(chainAng)+ey*Math.cos(chainAng);
      const pt=balPt(chainCT+rx,chainCP+ry);
      i===0?ctx.moveTo(pt.x,pt.y):ctx.lineTo(pt.x,pt.y);
    }
    ctx.closePath();
    ctx.fillStyle='rgba(100,150,255,.04)';ctx.fill();
    ctx.strokeStyle='rgba(100,150,255,.55)';ctx.lineWidth=1.5;
    ctx.setLineDash([8,4]);ctx.stroke();ctx.setLineDash([]);

    // Chain cross lines
    if(document.getElementById('ell-show-cross').checked){
      const cPerpAng=chainAng+Math.PI/2;
      ctx.strokeStyle='rgba(100,150,255,.25)';ctx.lineWidth=1;ctx.setLineDash([3,5]);
      const ce1=balPt(chainCT+sMaj*Math.cos(chainAng),chainCP+sMaj*Math.sin(chainAng));
      const ce2=balPt(chainCT-sMaj*Math.cos(chainAng),chainCP-sMaj*Math.sin(chainAng));
      const cm1=balPt(chainCT+sMin*Math.cos(cPerpAng),chainCP+sMin*Math.sin(cPerpAng));
      const cm2=balPt(chainCT-sMin*Math.cos(cPerpAng),chainCP-sMin*Math.sin(cPerpAng));
      ctx.beginPath();ctx.moveTo(ce1.x,ce1.y);ctx.lineTo(ce2.x,ce2.y);ctx.stroke();
      ctx.beginPath();ctx.moveTo(cm1.x,cm1.y);ctx.lineTo(cm2.x,cm2.y);ctx.stroke();
      ctx.setLineDash([]);
    }

    // T2 terminus of chain
    const pxT2=balPt(chain2T,chain2P);
    const t2Price=chain2P*SF;
    const t2Bar=chain2T/hpd;
    const t2Days=Math.round(t2Bar-(candles.length-1));
    ctx.shadowColor='rgba(100,150,255,.8)';ctx.shadowBlur=14;
    ctx.beginPath();ctx.arc(pxT2.x,pxT2.y,8,0,2*Math.PI);
    ctx.fillStyle='rgba(100,150,255,.9)';ctx.fill();
    ctx.shadowBlur=0;ctx.strokeStyle='#555';ctx.lineWidth=1;ctx.stroke();
    ctx.fillStyle='#fff';ctx.font='bold 9px sans-serif';ctx.textAlign='center';
    ctx.fillText('T2',pxT2.x,pxT2.y+3);
    const t2lbl='T2: '+Math.round(t2Price).toLocaleString('en-IN')+'  ('+(t2Days>0?'+':'')+t2Days+'d)';
    const t2lx=Math.min(pxT2.x+12,pad.l+pw-180);
    ctx.fillStyle='rgba(255,255,255,.95)';ctx.fillRect(t2lx-3,pxT2.y-25,t2lbl.length*6.2+8,16);
    ctx.strokeStyle='rgba(100,150,255,.5)';ctx.lineWidth=1;ctx.strokeRect(t2lx-3,pxT2.y-25,t2lbl.length*6.2+8,16);
    ctx.fillStyle='rgba(60,100,220,.9)';ctx.font='bold 10px sans-serif';ctx.textAlign='left';
    ctx.fillText(t2lbl,t2lx,pxT2.y-13);
    ctx.restore();
  }

  // SF source label bottom-left
  const sfSrc=(sqTimeScale!==1||sqPriceScale!==1)?'📐 Square panel':showBal?'⚖️ Balanced':'🔄 Auto';
  ctx.fillStyle='rgba(100,70,160,.65)';ctx.font='bold 9px sans-serif';ctx.textAlign='left';
  ctx.fillText('SF='+SF.toFixed(4)+' ('+sfSrc+')',pad.l+4,pad.t+ph-5);
}

function toggleFan(){
  fanMode=!fanMode;
  fanStep=0;
  const b=document.getElementById('tog-fan');
  b.classList.toggle('on',fanMode);
  b.textContent=fanMode?'🌟 PTV Fan ON':'🌟 PTV Fan OFF';
  b.style.color=fanMode?'#fff':'#e08040';
  document.getElementById('fan-controls').style.display=fanMode?'block':'none';
  document.getElementById('c').style.cursor=fanMode?'crosshair':'default';
  document.getElementById('fan-status').textContent='Click point A (anchor)';
  if(!fanMode) draw();
}

function clearFan(){
  fanStep=0;
  fanABar=fanAPrice=fanBBar=fanBPrice=0;
  fanADate=fanBDate=fanLiveDate=null;
  document.getElementById('fan-status').textContent='Click point A (anchor)';
  draw();
}

function drawFanOverlay(ctx,pad,ph,pw,vs,gap,lo,hi,lastCandle){
  if(fanStep===0) return;

  // ============================================================
  // KEY FIX: Fan lines are PURE HORIZONTAL PRICE LEVELS
  // Y coordinate = only from price, never from bar/X position
  // This means pan/zoom NEVER moves the lines
  // Only the A and B dot markers move with their candles
  // ============================================================

  const hpd=parseFloat(document.getElementById('hpd').value)||6.5;

  // Helper: find bar index from stored date
  function barFromDate(date,fallback){
    if(!date||!candles.length) return fallback;
    let best=fallback,bd=Infinity;
    for(let i=Math.max(0,fallback-30);i<Math.min(candles.length,fallback+30);i++){
      const d=Math.abs(candles[i].t.getTime()-date.getTime());
      if(d<bd){bd=d;best=i;}
    }
    return best;
  }

  // A dot position — X follows the candle, Y from stored price
  const aBarIdx=barFromDate(fanADate,fanABar);
  const ax=pad.l+(aBarIdx-vs)*gap+gap/2;
  const ay=pad.t+(1-(fanAPrice-lo)/(hi-lo))*ph;

  // B: use live mouse while step=1, locked price when step=2
  const bPrice=fanStep>=2?fanBPrice:fanLivePrice;
  const bBarIdx=fanStep>=2?barFromDate(fanBDate,fanBBar):Math.round(fanLiveBar||fanBBar);
  const bx=pad.l+(bBarIdx-vs)*gap+gap/2;
  const by=pad.t+(1-(bPrice-lo)/(hi-lo))*ph;

  // Base price difference (A to B) — this is fixed
  const rawPriceDiff=Math.abs(bPrice-fanAPrice);
  const isUp=bPrice<fanAPrice;

  // Draw A dot
  ctx.beginPath();ctx.arc(ax,ay,8,0,2*Math.PI);
  ctx.fillStyle='#e08040';ctx.fill();
  ctx.strokeStyle='#555';ctx.lineWidth=1.5;ctx.stroke();
  ctx.fillStyle='#000';ctx.font='bold 10px sans-serif';ctx.textAlign='center';
  ctx.fillText('A',ax,ay+3);

  if(fanStep<1) return;

  // Draw A->B connector line (just visual, not the fan)
  ctx.strokeStyle='rgba(224,128,64,.6)';ctx.lineWidth=1.5;ctx.setLineDash([4,4]);
  ctx.beginPath();ctx.moveTo(ax,ay);ctx.lineTo(bx,by);ctx.stroke();
  ctx.setLineDash([]);

  // B dot
  ctx.beginPath();ctx.arc(bx,by,7,0,2*Math.PI);
  ctx.fillStyle=fanStep>=2?'#e08040':'rgba(224,128,64,.45)';ctx.fill();
  ctx.strokeStyle='#555';ctx.lineWidth=1.5;ctx.stroke();
  ctx.fillStyle='#000';ctx.font='bold 10px sans-serif';ctx.textAlign='center';
  ctx.fillText('B',bx,by+3);

  // PTV info
  const dayDiff=Math.abs(bBarIdx-aBarIdx);
  const pp=showBal?rawPriceDiff/balDecimalShift:rawPriceDiff;
  const basePTV=Math.sqrt(pp*pp+(dayDiff*hpd)**2);
  ctx.font='bold 10px sans-serif';ctx.textAlign='left';
  ctx.fillStyle='rgba(255,255,255,.9)';ctx.fillRect(ax+12,ay-22,110,13);
  ctx.fillStyle='#e08040';ctx.fillText('PTV '+basePTV.toFixed(2),ax+13,ay-12);

  if(fanStep<2||rawPriceDiff<=0) return;

  // ============================================================
  // HORIZONTAL PRICE LEVEL LINES
  // Each line = A_price ± (rawPriceDiff × ratio)
  // Y = computed from price only — NEVER changes with pan/zoom
  // Extends full width of chart (pad.l to pad.l+pw)
  // ============================================================
  const FAN_RATS=[
    {id:'fan-r-0382',rv:0.382,label:'0.382',col:'#6a9ddb'},
    {id:'fan-r-05',  rv:0.5,  label:'0.5',  col:'#6a9ddb'},
    {id:'fan-r-0618',rv:0.618,label:'0.618',col:'#a066e0'},
    {id:'fan-r-1',   rv:1,    label:'1x',   col:'#e08040'},
    {id:'fan-r-1272',rv:1.272,label:'1.272',col:'#f5a623'},
    {id:'fan-r-1414',rv:1.414,label:'sqrt2',col:'#f5a623'},
    {id:'fan-r-1618',rv:1.618,label:'PHI',  col:'#f5e050'},
    {id:'fan-r-1732',rv:1.732,label:'sqrt3',col:'#f5e050'},
    {id:'fan-r-2',   rv:2,    label:'2x',   col:'#1a8f60'},
    {id:'fan-r-2236',rv:2.236,label:'sqrt5',col:'#1a8f60'},
    {id:'fan-r-2618',rv:2.618,label:'PHI2', col:'#50c8f5'},
    {id:'fan-r-3',   rv:3,    label:'3x',   col:'#50c8f5'},
    {id:'fan-r-pi',  rv:3.14159,label:'pi', col:'#c04040'},
  ];

  FAN_RATS.forEach(rat=>{
    const el=document.getElementById(rat.id);
    if(!el||!el.checked) return;

    const isKey=rat.rv===1||rat.rv===1.618||rat.rv===1.414||rat.rv===2||rat.rv===1.732;
    const lw=isKey?2:1;
    const col=rat.col;

    // Target price = FIXED in space, only depends on fanAPrice and rawPriceDiff
    const targetPriceDiff=rawPriceDiff*rat.rv;
    const targetPrice=isUp?fanAPrice-targetPriceDiff:fanAPrice+targetPriceDiff;

    // Y from price ONLY — this is what stays fixed
    const targetY=pad.t+(1-(targetPrice-lo)/(hi-lo))*ph;

    // Skip if off screen
    if(targetY<pad.t-5||targetY>pad.t+ph+5) return;

    const alpha=isKey?0.85:0.55;

    // HORIZONTAL LINE — full width, purely from price
    ctx.strokeStyle=col+(Math.round(alpha*255).toString(16).padStart(2,'0'));
    ctx.lineWidth=lw;
    ctx.setLineDash(isKey?[]:[5,4]);
    ctx.beginPath();
    ctx.moveTo(pad.l,targetY);       // left edge
    ctx.lineTo(pad.l+pw+50,targetY); // right edge + margin
    ctx.stroke();
    ctx.setLineDash([]);

    // Also draw diagonal FROM A toward the right — this creates the fan look
    // But it's secondary — the horizontal is the true level
    ctx.strokeStyle=col+(Math.round(alpha*0.4*255).toString(16).padStart(2,'0'));
    ctx.lineWidth=0.7;
    ctx.beginPath();
    ctx.moveTo(ax,ay);
    ctx.lineTo(pad.l+pw+50,targetY);
    ctx.stroke();

    // Label on right edge
    const lbl=rat.label+'  '+Math.round(targetPrice).toLocaleString('en-IN');
    const lx=pad.l+pw+2;
    ctx.font=(isKey?'bold ':'')+' 9px sans-serif';ctx.textAlign='left';
    ctx.fillStyle='rgba(255,255,255,.9)';
    ctx.fillRect(lx-1,targetY-10,lbl.length*5.3+4,12);
    ctx.fillStyle=col;
    ctx.fillText(lbl,lx,targetY);

    // Dot on right edge
    if(isKey){
      ctx.beginPath();ctx.arc(pad.l+pw+50,targetY,4,0,2*Math.PI);
      ctx.fillStyle=col;ctx.fill();
    }
  });

  // Status
  ctx.fillStyle='rgba(224,128,64,.5)';ctx.font='10px sans-serif';ctx.textAlign='left';
  ctx.fillText('Fan A='+Math.round(fanAPrice).toLocaleString('en-IN')+
    ' B='+Math.round(bPrice).toLocaleString('en-IN')+
    ' diff='+Math.round(rawPriceDiff).toLocaleString('en-IN')+
    ' right-click=back',pad.l+4,pad.t+ph-5);
}


function toggleRuler(){
  rulerMode=!rulerMode;
  rulerActive=false;
  const b=document.getElementById('tog-ruler');
  b.classList.toggle('on2',rulerMode);
  b.textContent=rulerMode?' Ruler ON':' Ruler OFF';
  b.style.borderColor=rulerMode?'#a066e0':'#a066e0';
  b.style.color=rulerMode?'#fff':'#a066e0';
  document.getElementById('c').style.cursor=rulerMode?'crosshair':'default';
  if(!rulerMode) draw(); // clear overlay
}

// 
// LOAD
async function loadData(){
  let sym=document.getElementById('sym').value;
  
  if(sym==='CUSTOM')
    sym=document.getElementById('csym').value.trim();

  if(!sym){
    alert('Enter symbol');
    return;
  }

  const intv=document.getElementById('intv').value;
  const rng=document.getElementById('rng').value;

  setStat('loading','⏳ Loading '+sym+'...');

  try{

    const url=
      `https://query1.finance.yahoo.com/v8/finance/chart/${encodeURIComponent(sym)}?interval=${intv}&range=${rng}`;

    const proxy=
      `https://corsproxy.io/?${encodeURIComponent(url)}`;

    const r=await fetch(proxy);

    if(!r.ok)
      throw new Error(`HTTP ${r.status}`);

    const data=await r.json();

    if(!data.chart || !data.chart.result)
      throw new Error('No chart data');

    const res=data.chart.result[0];

    const ts=res.timestamp;
    const q=res.indicators.quote[0];

    candles=ts.map((t,i)=>({
      t:new Date(t*1000),
      o:q.open[i],
      h:q.high[i],
      l:q.low[i],
      c:q.close[i]
    }))
    .filter(d=>d.o!=null && d.h!=null && d.l!=null && d.c!=null);

    setStat(
      'success',
      '✅ Loaded '+candles.length+' candles'
    );

    draw();

  }
  catch(e){
    console.error(e);
    setStat('error','❌ '+e.message);
  }
}

// 
// MANUAL POINTS
// 
function addManualPt(price,idx){const i=manualPts.length;manualPts.push({id:'m'+Date.now()+i,price,idx,label:'M'+(i+1),type:'M'});renderManualPts();draw();}
function removeManualPt(id){manualPts=manualPts.filter(p=>p.id!==id);renderManualPts();draw();}
function clearManual(){manualPts=[];renderManualPts();draw();}
function renderManualPts(){
  const c=document.getElementById('pts-list');c.innerHTML='';
  if(!manualPts.length){c.innerHTML='<p style="color:#333;font-size:11px">Click chart to add manual points</p>';return;}
  manualPts.forEach((pt,i)=>{
    const col=COLORS[i%COLORS.length],dt=candles[pt.idx]?fmDate(candles[pt.idx].t):'';
    const r=document.createElement('div');r.className='pt-row';
    r.innerHTML=`<div class="dot" style="background:${col}"></div><b style="color:${col};width:26px;flex-shrink:0">${pt.label}</b>
      <span style="color:#333;font-size:10px;flex-shrink:0;width:70px">${dt}</span>
      <input type="number" value="${pt.price.toFixed(2)}" onchange="pt.price=parseFloat(this.value)||0;draw()" style="width:90px;font-size:12px;margin-left:4px">
      <button onclick="removeManualPt('${pt.id}')" style="margin-left:auto;background:#2a1515;border:none;color:#c04040;border-radius:4px;padding:2px 6px;cursor:pointer;font-size:11px"></button>`;
    c.appendChild(r);
  });
}




// 
// CANVAS EVENTS
// 
// CANVAS EVENTS
// 
const cv=document.getElementById('c');

// Helper: pixel -> bar/price using stored chart params
function pxToChart(mx,my){
  const vs=cv._vs||0,gap=cv._gap||1,pad=cv._pad||{l:78,t:28};
  const ph=cv._ph||420,lo=cv._lo||0,hi=cv._hi||1;
  return{
    bar: vs+(mx-pad.l-gap/2)/gap,
    price: hi-(my-pad.t)/ph*(hi-lo),
    vs,gap,pad,ph,lo,hi
  };
}

cv.addEventListener('mousedown',e2=>{
  if(e2.button!==0) return;
  if(rulerMode){
    const r=cv.getBoundingClientRect();
    rulerStartX=e2.clientX-r.left;
    rulerStartY=e2.clientY-r.top;
    rulerEndX=rulerStartX;rulerEndY=rulerStartY;
    rulerActive=true;
    return;
  }
  isDragging=false;dragStartX=e2.clientX;dragStartVS=viewStart;
});

cv.addEventListener('mousemove',function(e2){
  if(!candles.length) return;
  const r=this.getBoundingClientRect();
  const mx=e2.clientX-r.left,my=e2.clientY-r.top;

  // Paste mode: mouse controls rotation angle
  if(ellPasteMode && ellCopied && ellPasteBar!==0){
    const c3=pxToChart(mx,my);
    // Angle from anchor to mouse in raw bar/price space (same as how dBar/dPrice is stored)
    const dBar   = c3.bar   - ellPasteBar;
    const dPrice = c3.price - ellPastePrice;
    ellPasteAngle = Math.atan2(dPrice, dBar);
    draw();return;
  }

  // Ellipse: live preview
  if(ellMode){
    const c3=pxToChart(mx,my);
    ellLiveBar=Math.round(c3.bar);ellLivePrice=c3.price;
    if(ellStep===2&&e2.buttons===1){
      ellBBar=ellLiveBar;ellBPrice=ellLivePrice;
      ellBDate=candles[Math.max(0,Math.min(candles.length-1,ellLiveBar))]?.t||null;
    }
    if(ellStep>=1) draw();return;
  }

  // Fan: live preview
  if(fanMode){
    const c3=pxToChart(mx,my);
    fanLiveBar=Math.round(c3.bar);fanLivePrice=c3.price;
    fanLiveDate=candles[Math.max(0,Math.min(candles.length-1,Math.round(c3.bar)))]?.t||null;
    if(fanStep===2&&e2.buttons===1){
      fanBBar=fanLiveBar;fanBPrice=fanLivePrice;fanBDate=fanLiveDate;
    }
    if(fanStep>=1) draw();return;
  }

  // Projector: live preview while placing pt2, or drag pt2
  if(projMoveMode){
    const c3=pxToChart(mx,my);
    projLiveBar=c3.bar; projLivePrice=c3.price;
    if(projStep===2&&e2.buttons===1){
      // Dragging pt2
      projPt2Bar=c3.bar; projPt2Price=c3.price;
    }
    if(projStep>=1) draw(); // redraw with live line
    return;
  }

  // Ruler: drag to update end point
  if(rulerMode&&rulerActive&&e2.buttons===1){
    rulerEndX=mx;rulerEndY=my;
    draw();return;
  }

  // Pan: drag chart
  if(!rulerMode&&!projMoveMode&&e2.buttons===1){
    const dx=e2.clientX-dragStartX,{count}=getVR();
    const g2=(this.offsetWidth-168)/Math.max(count,1);
    const shift=dx/g2;
    if(Math.abs(dx)>4) isDragging=true;
    if(isDragging){
      const total=getTotalBars();
      viewStart=Math.max(0,Math.min(total-count,dragStartVS-shift));
      viewEnd=viewStart+count-1;
      if(viewEnd>=total)viewEnd=total-1;
      updateZL();draw();return;
    }
  }

  // OHLC crosshair
  const{vs,gap,pad}=pxToChart(mx,my);
  const idx=Math.max(0,Math.min(candles.length-1,vs+Math.round((mx-pad.l-gap/2)/gap)));
  const c3=candles[idx];if(!c3) return;
  const ci=document.getElementById('cinfo');ci.style.display='block';
  // Show full decimals in crosshair for precise snap reading
  const fmt=v=>v?.toLocaleString('en-IN',{minimumFractionDigits:2,maximumFractionDigits:2});
  ci.innerHTML=`${fmDate(c3.t)} O:<b>${fmt(c3.o)}</b> H:<b style="color:#1a8f60">${fmt(c3.h)}</b> L:<b style="color:#c04040">${fmt(c3.l)}</b> C:<b style="color:#f5a623">${fmt(c3.c)}</b>`;

  // Snap preview — show which exact price will snap on click
  if(!fanMode&&!projMoveMode&&!rulerMode){
    let sp=c3.c,sc='rgba(255,255,255,.6)';
    if(snapMode==='high'){sp=c3.h;sc='rgba(26,143,96,.95)';}
    else if(snapMode==='low'){sp=c3.l;sc='rgba(192,64,64,.95)';}
    else if(snapMode==='exact'){
      const lo2=cv._lo||0,hi2=cv._hi||1,ph2=cv._ph||420,pt2=(cv._pad||{t:28}).t;
      sp=hi2-(my-pt2)/ph2*(hi2-lo2);sc='rgba(160,102,224,.95)';
    }
    cv._snapPrice=sp;
    cv._snapX=pad.l+(idx-vs)*gap+gap/2;
    cv._snapCol=sc;
    // Show snap price in snap info bar
    const si=document.getElementById('snapinfo');
    si.style.display='block';
    si.textContent=`${snapMode.toUpperCase()}: ${sp.toFixed(2)}`;
    si.style.background=sc.replace('.95',',.85').replace('.6',',.6');
    // Redraw chart so crosshair moves with cursor
    if(!cv._rafPending){
      cv._rafPending=true;
      requestAnimationFrame(()=>{cv._rafPending=false;draw();});
    }
  } else {
    cv._snapPrice=null;
    document.getElementById('snapinfo').style.display='none';
  }
});

cv.addEventListener('mouseup',e2=>{
  if(rulerMode&&rulerActive){rulerActive=false;return;}
  setTimeout(()=>isDragging=false,50);
});

cv.addEventListener('click',function(e2){
  if(!candles.length) return;
  const r=this.getBoundingClientRect();
  const mx=e2.clientX-r.left,my=e2.clientY-r.top;
  const c2=pxToChart(mx,my);

  // PASTE MODE — click 1: place A, click 2: confirm rotated B
  if(ellPasteMode && ellCopied){
    const ci2=Math.max(0,Math.min(candles.length-1,Math.round(c2.bar)));
    const snapCandle=candles[ci2];
    const snapPrice=snapMode==='high'?snapCandle.h:snapMode==='low'?snapCandle.l:snapMode==='exact'?c2.price:snapCandle.c;

    if(ellPasteBar===0){
      // First click — set new A point, initialise angle to original
      ellPasteBar=ci2; ellPastePrice=snapPrice;
      // Start angle = original angle (using raw dBar, dPrice as vector)
      ellPasteAngle=Math.atan2(ellCopied.dPrice, ellCopied.dBar);
      document.getElementById('ell-paste-status').textContent=
        'A set @ '+Math.round(snapPrice).toLocaleString('en-IN')+' — move mouse to rotate, click to confirm B';
    } else {
      // Second click — commit. B = A + rotated vector of same length
      const origLen=Math.sqrt(ellCopied.dBar**2 + ellCopied.dPrice**2);
      const newDBar  = origLen * Math.cos(ellPasteAngle);
      const newDPrice= origLen * Math.sin(ellPasteAngle);
      ellABar   = ellPasteBar;
      ellAPrice = ellPastePrice;
      ellADate  = candles[Math.max(0,Math.min(candles.length-1,ellPasteBar))]?.t||null;
      ellBBar   = Math.round(ellPasteBar + newDBar);
      ellBPrice = ellPastePrice + newDPrice;
      ellBDate  = ellBBar>=0&&ellBBar<candles.length ? candles[ellBBar]?.t||null : null;
      ellStep=2; ellPasteMode=false; ellPasteBar=0;
      document.getElementById('ell-paste-status').style.display='none';
      document.getElementById('ell-lock-sf-btn').style.display='inline';
      document.getElementById('ell-continue-btn').style.display='inline';
      document.getElementById('ell-copy-btn').style.display='inline';
      document.getElementById('ell-status').textContent='Pasted ✓ drag B to fine-tune';
    }
    draw();return;
  }

  // ELLIPSE MODE — click A then B
  if(ellMode){
    const ci2=Math.max(0,Math.min(candles.length-1,Math.round(c2.bar)));
    const snapCandle=candles[ci2];
    const snapPrice=snapMode==='high'?snapCandle.h:snapMode==='low'?snapCandle.l:snapMode==='exact'?c2.price:snapCandle.c;
    const snapDate=snapCandle.t;
    if(ellStep===0){
      ellABar=ci2;ellAPrice=snapPrice;ellADate=snapDate;ellStep=1;
      document.getElementById('ell-status').textContent=
        'A @ '+Math.round(snapPrice).toLocaleString('en-IN')+' — now click B (swing end)';
    } else if(ellStep===1){
      ellBBar=ci2;ellBPrice=snapPrice;ellBDate=snapDate;ellStep=2;
      const days=Math.abs(ellBBar-ellABar);
      const pp=Math.abs(ellBPrice-ellAPrice);
      const hpd2=parseFloat(document.getElementById('hpd').value)||6.5;
      const ptv=Math.sqrt(pp*pp+(days*hpd2)**2);
      const autoSF=(pp/(days*hpd2)).toFixed(4);
      document.getElementById('ell-status').textContent=
        'PTV='+ptv.toFixed(2)+' — SF='+autoSF+' — drag B to adjust, right-click=step back';
      document.getElementById('ell-lock-sf-btn').style.display='inline';
      document.getElementById('ell-continue-btn').style.display='inline';
      document.getElementById('ell-copy-btn').style.display='inline';
      document.getElementById('ell-lock-sf-status').style.display='none';
    } else {
      ellBBar=ci2;ellBPrice=snapPrice;ellBDate=snapDate;
    }
    draw();return;
  }

  // FAN MODE -- click A then B
  if(fanMode){
    if(fanStep===0){
      fanABar=Math.round(c2.bar);fanAPrice=c2.price;
      fanADate=candles[Math.max(0,Math.min(candles.length-1,Math.round(c2.bar)))]?.t||null;
      fanStep=1;
      document.getElementById('fan-status').textContent=
        'A @ '+Math.round(c2.price).toLocaleString('en-IN')+' -- now click B';
    } else if(fanStep===1){
      fanBBar=Math.round(c2.bar);fanBPrice=c2.price;
      fanBDate=candles[Math.max(0,Math.min(candles.length-1,Math.round(c2.bar)))]?.t||null;
      fanStep=2;
      const dayDiff=Math.abs(fanBBar-fanABar);
      const pp=Math.abs(fanBPrice-fanAPrice);
      const hpd2=parseFloat(document.getElementById('hpd').value)||6.5;
      const ptv=Math.sqrt(pp*pp+(dayDiff*hpd2)**2);
      document.getElementById('fan-status').textContent=
        'PTV='+ptv.toFixed(2)+' -- drag to adjust, right-click=step back';
    } else {
      fanBBar=Math.round(c2.bar);fanBPrice=c2.price;
      fanBDate=candles[Math.max(0,Math.min(candles.length-1,Math.round(c2.bar)))]?.t||null;
    }
    draw();return;
  }

  // PROJECTOR MODE  two-click: pt1 then pt2
  if(projMoveMode){
    if(projStep===0){
      // First click = point 1 (start of PTV)
      projPt1Bar=c2.bar; projPt1Price=c2.price;
      projStep=1;
      const si=document.getElementById('snapinfo');si.style.display='block';
      si.textContent=` Point 1 set @ ${Math.round(c2.price).toLocaleString('en-IN')} -- now click POINT 2 (end of PTV)`;
      setTimeout(()=>si.style.display='none',4000);
      draw();
    } else if(projStep===1){
      // Second click = point 2 (end of PTV, origin of projections)
      projPt2Bar=c2.bar; projPt2Price=c2.price;
      projStep=2;
      projMoveSelected=true;
      const si=document.getElementById('snapinfo');si.style.display='block';
      const rawD=Math.abs(projPt2Bar-projPt1Bar);
      const rawP=Math.abs(projPt2Price-projPt1Price);
      const hpd2=parseFloat(document.getElementById('hpd').value)||6.5;
      const pp=showBal?rawP/balDecimalShift:rawP;
      const hrs=rawD*hpd2;
      const ptv=Math.sqrt(pp*pp+hrs*hrs);
      si.textContent=` PTV=${ptv.toFixed(2)} -- drag pt2 or click to reposition  right-click to clear`;
      setTimeout(()=>si.style.display='none',4000);
      draw();
    } else {
      // Already drawn  move pt2 to new click
      projPt2Bar=c2.bar; projPt2Price=c2.price;
      draw();
    }
    return;
  }

  // RULER MODE: click to clear
  if(rulerMode){
    rulerActive=false;rulerStartX=rulerStartY=rulerEndX=rulerEndY=0;
    draw();return;
  }

  if(isDragging) return;

  // MANUAL POINT PLACEMENT
  // Find bar by closest pixel X — more accurate than rounding bar float
  const pad2=cv._pad||{l:78};
  const gap2=cv._gap||1;
  const vs2=cv._vs||0;
  // Find which candle center is closest to click X
  let bestIdx=0,bestDist=99999;
  const searchStart=Math.max(0,Math.floor(c2.bar)-2);
  const searchEnd=Math.min(candles.length-1,Math.ceil(c2.bar)+2);
  for(let ci=searchStart;ci<=searchEnd;ci++){
    const cx=pad2.l+(ci-vs2)*gap2+gap2/2;
    const dist=Math.abs(mx-cx);
    if(dist<bestDist){bestDist=dist;bestIdx=ci;}
  }
  const idx=bestIdx;
  const candle=candles[idx];if(!candle) return;
  let price;
  if(snapMode==='high') price=candle.h;
  else if(snapMode==='low') price=candle.l;
  else if(snapMode==='exact') price=c2.price;
  else price=candle.c;
  const si=document.getElementById('snapinfo');si.style.display='block';
  si.textContent=`M${manualPts.length+1} @ ${fmDate(candle.t)} ${snapMode.toUpperCase()}: ${price.toFixed(2)}`;
  setTimeout(()=>si.style.display='none',2500);
  addManualPt(price,idx);
});

cv.addEventListener('contextmenu',e2=>{
  e2.preventDefault();
  if(ellMode){
    if(ellStep>=2){ellStep=1;draw();}
    else if(ellStep===1){ellStep=0;draw();}
    return;
  }
  if(fanMode){
    if(fanStep>=2){fanStep=1;draw();}
    else if(fanStep===1){fanStep=0;draw();}
    return;
  }
  if(projMoveMode){
    // Step back: if pt2 placed go back to pt1, if pt1 go back to idle
    if(projStep>=2){projStep=1;projMoveSelected=false;draw();}
    else if(projStep===1){projStep=0;draw();}
    return;
  }
  if(manualPts.length>0) removeManualPt(manualPts[manualPts.length-1].id);
});

cv.addEventListener('mouseleave',()=>{
  document.getElementById('cinfo').style.display='none';
  document.getElementById('snapinfo').style.display='none';
  isDragging=false;
  cv._snapPrice=null;
  draw();
});
cv.addEventListener('wheel',e2=>{e2.preventDefault();if(e2.deltaY<0)zoomIn();else zoomOut();},{passive:false});
cv.addEventListener('touchstart',e2=>{touchStartX=e2.touches[0].clientX;touchStartVS=viewStart;},{passive:true});
cv.addEventListener('touchmove',e2=>{
  e2.preventDefault();
  const dx=e2.touches[0].clientX-touchStartX,{count}=getVR();
  const g2=(cv.offsetWidth-168)/Math.max(count,1);
  const shift=dx/g2,total=getTotalBars();
  viewStart=Math.max(0,Math.min(total-count,touchStartVS-shift));
  viewEnd=viewStart+count-1;updateZL();draw();
},{passive:false});

function setSnap(m){
  snapMode=m;
  ['c','h','l','e'].forEach(x=>document.getElementById('sb-'+x)?.classList.remove('on'));
  document.getElementById('sb-'+m[0])?.classList.add('on');
}
function toggleAuto(){
  showAuto=!showAuto;
  const b=document.getElementById('tog-auto');
  b.classList.toggle('on',showAuto);
  b.textContent=showAuto?' Auto ON':' Auto OFF';
  draw();
}
function toggleGann(){
  showGann=!showGann;
  const b=document.getElementById('tog-gann');
  b.classList.toggle('on2',showGann);
  b.textContent=showGann?' Gann ON':' Gann OFF';
  draw();
}
function toggleProj(){
  showProj=!showProj;
  const b=document.getElementById('tog-proj');
  b.classList.toggle('on3',showProj);
  b.textContent=showProj?' Proj ON':' Proj OFF';
  draw();
}
function toggleBal(){
  showBal=!showBal;
  const b=document.getElementById('tog-bal');
  b.classList.toggle('on4',showBal);
  b.textContent=showBal?' Balanced 45deg ON':' Balanced 45deg OFF';
  b.style.background=showBal?'#1a5c20':'';
  document.getElementById('bal-panel').style.display=showBal?'block':'none';
  if(showBal){
    const lt=getLastTwo();
    if(lt){
      const rawP=Math.abs(lt.last.price-lt.prev.price);
      const rawD=lt.ptv.dayDiff;
      document.getElementById('bal-p').value=rawP.toFixed(2);
      document.getElementById('bal-t').value=rawD;
      document.getElementById('bal-sp').value=lt.last.price.toFixed(2);
      const r=calcPTV_balanced({price:0,idx:0},{price:rawP,idx:rawD});
      balScaleFactor=r.effSF||r.scaleFactor;
    }
    calcBal();
  } else {
    balScaleFactor=1;
  }
  draw();
}
function toggleConfChart(){
  showConfChart=!showConfChart;
  const b=document.getElementById('tog-conf-chart');
  b.classList.toggle('on3',showConfChart);
  b.textContent=showConfChart?' Ratio Lines ON':' Ratio Lines OFF';
  b.style.borderColor='#f5a623';
  b.style.color=showConfChart?'#fff':'#f5a623';
  draw();
}

function buildSels(){
  ['m1rat','m2rat','m3rat'].forEach(id=>{
    const s=document.getElementById(id);if(!s)return;
    s.innerHTML='';
    RATS.forEach(([v,n])=>{
      const o=document.createElement('option');
      o.value=v;o.textContent=n;
      if(v===1)o.selected=true;
      s.appendChild(o);
    });
  });
}
function sw(n){
  for(let i=1;i<=7;i++){
    document.getElementById('cb'+i).className='cb'+(i===n?' on':'');
    document.getElementById('tb'+i).className='tab'+(i===n?' on':'');
  }
  if(n===4)c4();if(n===6)cr();if(n===7)cpi();
}
function setStat(t,m){
  const s=document.getElementById('stat');s.textContent=m;s.style.display='block';
  s.style.background=t==='loading'?'#0a1525':t==='success'?'#0a251a':'#250a0a';
  s.style.color=t==='loading'?'#5a9fd5':t==='success'?'#5af5a0':'#f57a7a';
  if(t==='success')setTimeout(()=>s.style.display='none',4000);
}
function onSettingChange(){if(candles.length){autoSwings=detectSwings();draw();}}

window.addEventListener('resize',draw);
buildSels();draw();cpi();cr();calcBal();
</script>
</body>
</html>
