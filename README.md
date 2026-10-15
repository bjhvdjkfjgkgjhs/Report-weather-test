<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#0b111d">
<title>Storm Spotter GPS · Reporting Map</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
<style>
:root { color-scheme:dark; --bg:#0b111d; --panel:#121b2c; --panel2:#19253b; --ink:#e9f1ff; --muted:#a1b0c8; --line:#2e3e56; --accent:#5aa7ff; --good:#4bd69c; --orange:#ffc36b; --red:#ff7482; }
* { box-sizing:border-box; } html,body { width:100%;height:100%;margin:0; overflow:hidden;font-family:Inter,ui-sans-serif,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;background:var(--bg);color:var(--ink); }
button,input,select,textarea {font:inherit;} button {cursor:pointer;touch-action:manipulation;} a{color:#95c7ff}
#map{position:absolute;inset:0;z-index:1;background:#152034;}
.leaflet-container{font:inherit}.leaflet-control-zoom{border:1px solid #566b85!important;box-shadow:0 8px 30px #0005!important}.leaflet-control-zoom a{color:#dceaff!important;background:#17283d!important;border-color:#34475e!important}
.top{position:absolute;z-index:600;left:14px;right:434px;top:calc(12px + env(safe-area-inset-top));display:flex;gap:9px;align-items:center;pointer-events:none;}
.brand{pointer-events:auto;display:flex;align-items:center;gap:11px;background:#111c2bf2;border:1px solid #31465f;border-radius:15px;padding:10px 14px;box-shadow:0 10px 30px #0007;white-space:nowrap}
.logo{background:#273d5e;color:#75b7ff;display:grid;place-items:center;width:34px;height:34px;border-radius:10px;font-size:21px}.brand strong{display:block;font-size:14px;letter-spacing:.4px}.brand small{display:block;font-size:11px;color:var(--muted);margin-top:2px}
.pillbtn{pointer-events:auto;display:inline-flex;align-items:center;gap:7px;min-height:43px;padding:0 14px;border:1px solid #375475;border-radius:13px;background:#17253aea;color:#e5f2ff;box-shadow:0 7px 20px #0005;font-size:13px;font-weight:700;white-space:nowrap}.pillbtn.primary{background:#2771cb;border-color:#5794dc}.pillbtn.on{background:#164e43;border-color:#359876}.pillbtn:active,.btn:active{transform:translateY(1px)}
#drawer{position:absolute;right:12px;top:12px;bottom:12px;width:404px;z-index:700;background:#101a2bf7;border:1px solid #344761;border-radius:20px;display:flex;flex-direction:column;box-shadow:0 18px 60px #000a;backdrop-filter:blur(13px);overflow:hidden}
.drawerhead{padding:17px 18px 12px;flex-shrink:0;border-bottom:1px solid var(--line)}.headrow{display:flex;justify-content:space-between;align-items:center;gap:10px}.headrow h1{font-size:17px;margin:0;font-weight:780;letter-spacing:-.2px}.sub{font-size:12px;color:var(--muted);margin:7px 0 0;line-height:1.45}.smallstatus{display:flex;gap:7px;align-items:center;color:var(--muted);font-size:11px}.dot{width:7px;height:7px;border-radius:50%;background:var(--orange)}.dot.ok{background:var(--good)}
.tabs{display:grid;grid-template-columns:1fr 1fr 1fr;gap:6px;margin:14px 0 0}.tab{border:1px solid #35485f;border-radius:10px;background:#1a2940;color:#b8cae1;padding:10px 4px;font-weight:750;font-size:12px}.tab.active{background:#24568d;color:white;border-color:#548dc9}
.scroll{flex:1;overflow-y:auto;overscroll-behavior:contain;padding:16px 18px 21px;scrollbar-color:#3a4c63 transparent}
.sectiontitle{font-size:11px;color:#96b0d0;text-transform:uppercase;letter-spacing:1.1px;font-weight:780;margin:1px 0 10px}
.coordbar{display:grid;grid-template-columns:1fr auto;gap:10px;padding:11px 12px;background:#1a2a40;border:1px solid #32485f;border-radius:12px;align-items:center;margin-bottom:10px}
.coordbar strong{display:block;font-size:14px;font-variant-numeric:tabular-nums}.coordbar span{display:block;font-size:11px;color:#9bb2cb;margin-top:3px}
.btn{border:1px solid #4b6785;background:#203552;color:#e9f3ff;border-radius:10px;padding:10px 12px;font-size:12px;font-weight:700;line-height:1.2;min-height:39px}.btn.accent{background:#2778d2;border-color:#388be8;color:#fff}.btn.mint{background:#1c6859;border-color:#2e9d7d}.btn.ghost{background:#152238;border-color:#32465e}.btn.link{background:#16273e;color:#93c8ff}.btn.danger{background:#42202c;color:#ffb5bd;border-color:#713744}.btn:disabled{cursor:not-allowed;opacity:.5}.row{display:flex;gap:8px;flex-wrap:wrap;align-items:center}.grow{flex:1}.row .grow{min-width:0}
.notice{background:#222d3f;border:1px solid #485568;color:#c3d1e5;font-size:12px;line-height:1.5;padding:11px 12px;border-radius:12px;margin:12px 0}.notice.warn{background:#392c24;border-color:#71523d;color:#f3d5af}.notice strong{color:#fff}.help{font-size:11px;color:#9fb0ca;line-height:1.5;margin:6px 0 12px}
.field{margin:12px 0}.field label{display:block;font-size:11px;font-weight:720;color:#b5c7e1;margin-bottom:6px}.field input,.field select,.field textarea{display:block;width:100%;min-height:39px;padding:10px 11px;border:1px solid #41536d;border-radius:10px;background:#19273c;color:#f0f6ff;outline:none;font-size:13px}.field textarea{min-height:78px;resize:vertical}.field input:focus,.field select:focus,.field textarea:focus{border-color:#61a2ef;box-shadow:0 0 0 2px #326eaa55}.field input::placeholder,.field textarea::placeholder{color:#8194ac}.two{display:grid;grid-template-columns:1fr 1fr;gap:9px}.two .field{margin:5px 0}.check{display:flex;gap:9px;align-items:flex-start;font-size:12px;color:#d1dfed;line-height:1.45;margin:13px 0}.check input{accent-color:#62aafa;width:17px;height:17px;flex-shrink:0;margin-top:0}
.hr{height:1px;background:#30445d;margin:18px 0}.sectionfoot{margin-top:15px}
.drafts{display:grid;gap:8px;margin:10px 0}.draft{display:flex;align-items:center;gap:9px;border:1px solid #344962;border-radius:10px;padding:9px;background:#18283e}.draft .detail{flex:1;min-width:0}.draft strong{display:block;font-size:12px;white-space:nowrap;text-overflow:ellipsis;overflow:hidden}.draft small{display:block;color:#9bafc7;margin-top:3px;font-size:11px}.draft button{padding:7px 9px}
#lsrStatus{font-size:11px;color:var(--muted);margin:7px 0 0}#toast{position:absolute;z-index:1200;bottom:30px;left:50%;transform:translateX(-50%);padding:11px 15px;background:#112a3d;border:1px solid #6291b4;box-shadow:0 10px 35px #000a;border-radius:12px;font-size:12px;max-width:min(92vw,510px);min-width:180px;text-align:center;opacity:0;transition:opacity .16s;pointer-events:none}#toast.show{opacity:1}
.gpsmarker{width:18px;height:18px;border:4px solid #fff;border-radius:100%;background:#2c9cff;box-shadow:0 0 0 3px #2688f766,0 3px 13px #000a}.eventmarker{width:27px;height:33px;background:transparent;position:relative;filter:drop-shadow(0 2px 3px #0009)}.eventmarker::before{content:'!';position:absolute;width:26px;height:26px;background:#ffa847;border:3px solid white;border-radius:50% 50% 50% 8%;transform:rotate(-45deg);color:#102035;font-size:19px;font-weight:900;text-align:center;line-height:23px}.eventmarker::after{content:'!';position:absolute;color:#102035;top:4px;left:10px;font-size:18px;font-weight:900}
.legend{position:absolute;z-index:500;bottom:25px;left:14px;border-radius:11px;padding:9px 12px;background:#111d2de8;border:1px solid #3b516b;max-width:280px;font-size:11px;color:#c4d7eb;box-shadow:0 8px 25px #0006}.legend div{margin:4px 0}.legend b{color:#fff}#gpsDetail{display:inline}.mapnote{color:#aac8eb;font-size:11px}
.leaflet-popup-content-wrapper,.leaflet-popup-tip{background:#18263b;color:#eef7ff}.leaflet-popup-content{font-size:12px;max-width:230px;overflow-wrap:anywhere}
@media(max-width:830px){
.top{left:9px;right:9px;top:calc(7px + env(safe-area-inset-top));justify-content:space-between;flex-wrap:wrap;gap:5px}.brand{padding:7px 9px;border-radius:12px}.logo{width:28px;height:28px;font-size:18px}.brand strong{font-size:11px}.brand small{font-size:10px}.pillbtn{min-height:39px;padding:0 9px;font-size:11px;border-radius:10px}
#drawer{left:0;right:0;bottom:0;top:auto;width:auto;height:min(62dvh,640px);border-radius:19px 19px 0 0;border-left:none;border-right:none;border-bottom:none}.drawerhead{padding:11px 15px 10px}.drawerhead:before{content:'';display:block;width:43px;height:4px;border-radius:3px;background:#64758c;margin:0 auto 10px}.headrow h1{font-size:15px}.sub{font-size:11px}.tabs{margin-top:10px}.scroll{padding:13px 15px calc(16px + env(safe-area-inset-bottom))}.legend{left:9px;bottom:calc(62dvh + 10px);max-width:240px;padding:7px 9px;font-size:10px}.leaflet-control-zoom{display:none}#drawer.collapsed{height:76px}#drawer.collapsed .tabs,#drawer.collapsed .sub,#drawer.collapsed .scroll{display:none}#drawer.collapsed .drawerhead{border:none}#drawer.collapsed ~ .legend{bottom:85px}
}
@media(max-width:385px){.brand small{display:none}.brand{gap:6px}.brand strong{font-size:10px}.pillbtn{font-size:10px}.legend{font-size:9px}.two{grid-template-columns:1fr 1fr}}
</style>
</head>
<body>
<div id="map" aria-label="Interactive map"></div>
<div class="top">
 <div class="brand"><div class="logo">◉</div><div><strong>STORM SPOTTER MAP</strong><small>GPS + official reporting links</small></div></div>
 <button id="gpsButton" class="pillbtn primary" type="button">◎ Start GPS</button>
 <button id="followButton" class="pillbtn" type="button" aria-pressed="true">⌖ Follow: On</button>
</div>
<aside id="drawer" aria-label="Report builder">
 <div class="drawerhead">
  <div class="headrow"><h1>Weather report builder</h1><div class="smallstatus"><i class="dot" id="gpsDot"></i><span id="gpsHeadline">GPS off</span> <button id="collapseButton" class="btn ghost" type="button" style="padding:5px 8px;min-height:25px;display:none">▾</button></div></div>
  <p class="sub">Choose the type, verify the event location, then copy your report to the official reporting service.</p>
  <div class="tabs" role="tablist" aria-label="Report service">
   <button class="tab active" type="button" data-mode="spotter" role="tab" aria-selected="true">Spotter Network</button>
   <button class="tab" type="button" data-mode="nws" role="tab" aria-selected="false">NWS / LSR</button>
   <button class="tab" type="button" data-mode="mping" role="tab" aria-selected="false">mPING</button>
  </div>
 </div>
 <div class="scroll" id="panelScroll">
  <div class="sectiontitle">Observed event location</div>
  <div class="coordbar"><div><strong id="coordText">No event position selected</strong><span id="coordHint">Select GPS or tap the map to set the event location</span></div><button class="btn ghost" type="button" id="copyCoords">Copy</button></div>
  <div class="row"><button class="btn grow" type="button" id="fromGPS">◎ Use my GPS</button><button class="btn grow" type="button" id="pickMap">⌖ Pick on map</button><button class="btn ghost" type="button" id="clearLocation">Clear</button></div>
  <div class="help" id="locationHelp">Your position (blue) is separate from the actual weather event (orange). Only set the marker where the event occurred.</div>
  <div id="guidance" class="notice warn"></div>
  <div class="two"><div class="field"><label for="eventType">Observation type</label><select id="eventType"></select></div><div class="field"><label for="eventTime">Time observed (local)</label><input type="datetime-local" id="eventTime"></div></div>
  <div id="magnitudeWrap" class="two"><div class="field"><label for="magnitude">Amount / measurement</label><input type="number" id="magnitude" step="0.1" min="0" placeholder="e.g. 1.5"></div><div class="field"><label for="units">Units / basis</label><select id="units"><option value="inches">Inches</option><option value="mph">mph</option><option value="feet">Feet</option><option value="n/a">Not applicable</option></select></div></div>
  <div class="two" id="measurementWrap"><div class="field"><label for="verification">Measurement</label><select id="verification"><option value="Measured">Measured</option><option value="Estimated">Estimated</option><option value="Observed, not measured">Observed, not measured</option></select></div><div class="field"><label for="place">Place / cross streets</label><input id="place" autocomplete="off" maxlength="140" placeholder="City / road / landmark"></div></div>
  <div class="field" id="mpPlaceWrap" style="display:none"><label for="mpPlace">Location description (optional)</label><input id="mpPlace" maxlength="140" placeholder="Near ..."></div>
  <div class="field"><label for="details">What did you personally observe?</label><textarea id="details" maxlength="1200" placeholder="Include what happened, observed impacts, and any measurement details."></textarea></div>
  <div id="reporterWrap" class="two"><div class="field"><label for="reporter">Your name (optional)</label><input id="reporter" maxlength="100" autocomplete="name" placeholder="Name"></div><div class="field"><label for="spotterId">Spotter ID (optional)</label><input id="spotterId" maxlength="80" placeholder="NWS or Spotter Network ID"></div></div>
  <label class="check"><input type="checkbox" id="firsthand"> <span>I personally observed this event, and the time and location are accurate. This is not a test report.</span></label>
  <div class="row"><button id="copyReport" class="btn accent grow" type="button">Copy report</button><button id="saveDraft" class="btn" type="button">Save draft</button></div>
  <div class="hr"></div>
  <div class="sectiontitle">Official submission</div>
  <div id="handoffInfo" class="help"></div>
  <div id="officeStatus" class="help"></div>
  <div class="row" id="submissionButtons"></div>
  <div class="hr"></div>
  <div class="row" style="justify-content:space-between"><div class="sectiontitle" style="margin:0">Saved on this device</div><button id="exportBtn" class="btn ghost" type="button">Export GeoJSON</button></div>
  <div class="drafts" id="drafts"><div class="help">No saved drafts yet.</div></div>
  <label class="check"><input id="showLSR" type="checkbox"> <span>Show official NWS Local Storm Reports from the past 24 hours on this map</span></label>
  <p id="lsrStatus">LSR layer off</p>
  <div class="help sectionfoot">This is an independent map, not an official Spotter Network, NWS, or mPING submission client. Saving or copying does <b>not</b> submit a report. Map needs internet; GPS normally needs HTTPS on mobile.</div>
 </div>
</aside>
<div class="legend" id="legend"><div>🔵 <b>Blue</b> = your GPS <span id="gpsDetail"></span></div><div>📍 <b>Orange</b> = event location (draggable)</div><div>🟣 <b>Purple</b> = NWS-issued LSRs (when enabled)</div></div>
<div id="toast" role="status" aria-live="polite"></div>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
(()=>{'use strict';
const $=id=>document.getElementById(id);
const modeNames={spotter:'Spotter Network',nws:'NWS weather report',mping:'mPING observation'};
const types={
 spotter:[['Tornado','Tornado'],['Funnel cloud','Funnel cloud'],['Rotating wall cloud','Rotating wall cloud'],['Hail','Hail'],['Measured wind gust','Measured wind gust'],['Wind damage','Wind damage'],['Flash flood','Flash flood'],['Flooding','Flooding'],['Tropical storm impacts','Tropical storm impacts']],
 nws:[['Tornado','Tornado'],['Funnel cloud','Funnel cloud'],['Wall cloud','Rotating wall cloud'],['Hail','Hail'],['Thunderstorm wind gust','Wind gust'],['Wind damage','Wind damage'],['Flooding','Flooding'],['Flash flooding','Flash flooding'],['Heavy rain','Heavy rain'],['Snow','Snow'],['Sleet','Sleet'],['Freezing rain','Freezing rain'],['Other storm impact','Other']],
 mping:[['None','No precipitation'],['Rain','Rain'],['Drizzle','Drizzle'],['Freezing rain','Freezing rain'],['Freezing drizzle','Freezing drizzle'],['Snow','Snow'],['Snow grains','Snow grains'],['Ice pellets / sleet','Ice pellets / sleet'],['Rain and snow mix','Rain and snow mix'],['Hail','Hail'],['Graupel','Graupel'],['Flooding','Flooding'],['Wind damage','Wind damage'],['Blowing dust','Blowing dust'],['Fog','Fog'],['Other weather','Other weather']]
};
const notes={
 spotter:'<strong>Spotter Network rules:</strong> Firsthand, timely severe-weather reports only. No test, radar-only, relayed, or lightning reports. Their guidance limits delays to 20 minutes and measured gusts to over 50 mph. An approved account and training are required.',
 nws:'<strong>NWS weather report:</strong> You report the observation to your local Weather Forecast Office. The NWS decides whether to issue an official Local Storm Report (LSR). Urgent tornado reports should be phoned in when safe.',
 mping:'<strong>mPING:</strong> Report what is happening at ground level. Official submission is through the current mPING mobile app or its authorized API; this independent HTML file cannot submit mPING reports directly.'
};
let mode='spotter', gps=null, gpsWatch=null, following=true, eventPos=null, eventMarker=null, accuracyRing=null, gpsMarker=null, gpsHeading=null, pickMode=false, wfo=null, wfoPoint=null, wfoRequest=0, wfoLookupState='idle', lsrLayer=null, lsrAbort=null, lsrTimer=null, toastTimer=null, editingId=null;
const map=L.map('map',{zoomControl:false,preferCanvas:true,worldCopyJump:true}).setView([39.073,-84.517],10);
L.control.zoom({position:'bottomleft'}).addTo(map);
const street=L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{maxZoom:19,attribution:'© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap contributors</a>'});
const sat=L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',{maxZoom:19,attribution:'Tiles © Esri, Maxar, Earthstar Geographics, and contributors'});
street.addTo(map);
L.control.layers({'Street / OpenStreetMap':street,'Satellite / Esri':sat},{},{position:'bottomleft'}).addTo(map);
const draftsLayer=L.layerGroup().addTo(map);const lsrGroup=L.layerGroup().addTo(map);
const pinIcon=L.divIcon({html:'<div class="eventmarker"></div>',className:'',iconSize:[29,37],iconAnchor:[13,34]});
const gpsIcon=L.divIcon({html:'<div class="gpsmarker"></div>',className:'',iconSize:[22,22],iconAnchor:[11,11]});
const esc=s=>String(s??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
function toast(message){$('toast').textContent=message;$('toast').classList.add('show');clearTimeout(toastTimer);toastTimer=setTimeout(()=>$('toast').classList.remove('show'),3500)}
function currentLocalInput(){const d=new Date();return new Date(d.getTime()-d.getTimezoneOffset()*60000).toISOString().slice(0,16)}
$('eventTime').value=currentLocalInput();
function setMode(which){mode=which;document.querySelectorAll('.tab').forEach(b=>{let yes=b.dataset.mode===which;b.classList.toggle('active',yes);b.setAttribute('aria-selected',String(yes))});
 const prev=$('eventType').value;$('eventType').replaceChildren();types[mode].forEach(([v,label])=>{const e=document.createElement('option');e.value=v;e.textContent=label;$('eventType').appendChild(e)});if(types[mode].some(a=>a[0]===prev))$('eventType').value=prev;
 $('guidance').innerHTML=notes[mode];$('reporterWrap').style.display=mode==='mping'?'none':'grid';$('measurementWrap').style.display=mode==='mping'?'none':'grid';$('mpPlaceWrap').style.display=mode==='mping'?'block':'none';$('firsthand').parentElement.style.display=mode==='mping'?'none':'flex';$('copyReport').textContent=mode==='mping'?'Copy mPING draft':'Copy report';updateFields();updateHandoff()}
function updateFields(){const type=$('eventType').value;const numeric=type==='Hail'||type.includes('gust')||type==='Flooding'||type==='Flash flood'||type==='Flash flooding'||type==='Snow'||type==='Heavy rain';$('magnitudeWrap').style.display=numeric?'grid':'none';if(type==='Hail'||type==='Snow'||type==='Heavy rain')$('units').value='inches';else if(type.includes('gust'))$('units').value='mph';else if(type==='Flooding'||type==='Flash flood'||type==='Flash flooding')$('units').value='feet';}
function fmtPos(pos){return `${pos.lat.toFixed(5)}, ${pos.lng.toFixed(5)}`}
function setEventPosition(p,pan=false){eventPos=L.latLng(p);if(!eventMarker){eventMarker=L.marker(eventPos,{icon:pinIcon,draggable:true,autoPan:true}).addTo(map);eventMarker.bindPopup('Observed event location. Drag the pin to the exact location.');eventMarker.on('dragend',()=>setEventPosition(eventMarker.getLatLng()));}else eventMarker.setLatLng(eventPos);
 $('coordText').textContent=fmtPos(eventPos);$('coordHint').textContent='Latitude, longitude · orange event pin';if(pan)map.setView(eventPos,Math.max(map.getZoom(),12)); wfo=null;wfoPoint=null;wfoLookupState='idle';wfoRequest++;$('officeStatus').textContent='';if(mode==='nws')resolveWfo();updateHandoff();}
function clearEvent(){eventPos=null;if(eventMarker){map.removeLayer(eventMarker);eventMarker=null;}$('coordText').textContent='No event position selected';$('coordHint').textContent='Choose GPS or tap the map';wfo=null;wfoPoint=null;wfoLookupState='idle';wfoRequest++;$('officeStatus').textContent='';updateHandoff();}
function useGPS(){if(!gps){toast('Start GPS and allow location access first.');return;}setEventPosition([gps.lat,gps.lng],true);toast('Event pin set to your latest GPS fix. Move it if the event occurred elsewhere.');}
function startGPS(){if(!navigator.geolocation){toast('Geolocation is not available in this browser.');return}if(!window.isSecureContext){toast('GPS requires HTTPS (or localhost). Upload this HTML to an HTTPS host for iPhone GPS.');return}
 $('gpsHeadline').textContent='Requesting GPS…';$('gpsButton').textContent='◎ Stop GPS';$('gpsButton').classList.add('on');$('gpsButton').classList.remove('primary');
 gpsWatch=navigator.geolocation.watchPosition(pos=>{const c=pos.coords;gps={lat:c.latitude,lng:c.longitude,accuracy:c.accuracy,speed:c.speed,heading:c.heading,at:pos.timestamp};
 const location=[c.latitude,c.longitude];if(!gpsMarker){gpsMarker=L.marker(location,{icon:gpsIcon,zIndexOffset:700}).addTo(map).bindPopup('Your device GPS location');accuracyRing=L.circle(location,{radius:Math.max(0,c.accuracy||0),color:'#40a3ff',weight:1,fillColor:'#40a3ff',fillOpacity:0.09,interactive:false}).addTo(map)}else{gpsMarker.setLatLng(location);accuracyRing.setLatLng(location);accuracyRing.setRadius(Math.max(0,c.accuracy||0));}
 $('gpsHeadline').textContent='GPS active';$('gpsDot').classList.add('ok');$('gpsDetail').textContent=`(±${Math.round(c.accuracy)} m)`;
 if(following){map.panTo(location,{animate:!map.getBounds().contains(location),duration:0.4})}
 },err=>{let m={1:'Location permission denied',2:'Location not available',3:'GPS timed out'}[err.code]||err.message||'GPS error';$('gpsHeadline').textContent=m;$('gpsDot').classList.remove('ok');toast(m + '. Check location permission and HTTPS.');if(err.code===1)stopGPS();},{enableHighAccuracy:true,maximumAge:3000,timeout:15000});}
function stopGPS(){if(gpsWatch!==null){navigator.geolocation.clearWatch(gpsWatch);gpsWatch=null;}$('gpsHeadline').textContent='GPS stopped';$('gpsDot').classList.remove('ok');$('gpsButton').textContent='◎ Start GPS';$('gpsButton').classList.add('primary');$('gpsButton').classList.remove('on');}
function setFollow(on){following=on;$('followButton').textContent=`⌖ Follow: ${on?'On':'Off'}`;$('followButton').setAttribute('aria-pressed',String(on));$('followButton').classList.toggle('on',on);if(on&&gps)map.panTo([gps.lat,gps.lng]);}
map.on('dragstart',()=>{if(!pickMode)setFollow(false)});
map.on('click',e=>{if(!pickMode)return;setEventPosition(e.latlng);pickMode=false;$('pickMap').textContent='⌖ Pick on map';$('pickMap').classList.remove('mint');toast('Event location pinned. Drag orange marker to refine.');});
$('gpsButton').addEventListener('click',()=>gpsWatch===null?startGPS():stopGPS());$('followButton').addEventListener('click',()=>setFollow(!following));$('fromGPS').addEventListener('click',useGPS);$('clearLocation').addEventListener('click',clearEvent);
$('pickMap').addEventListener('click',()=>{pickMode=!pickMode;$('pickMap').classList.toggle('mint',pickMode);$('pickMap').textContent=pickMode?'Tap map now':'⌖ Pick on map';toast(pickMode?'Tap the map where the weather event occurred.':'Map picking canceled.');if(pickMode&&innerWidth<=830){$('drawer').classList.add('collapsed');$('collapseButton').textContent='▴';setTimeout(()=>map.invalidateSize(),250)}});
$('copyCoords').addEventListener('click',async()=>{if(!eventPos){toast('Choose a location first.');return;}await copyText(fmtPos(eventPos))});
async function copyText(s){try{if(navigator.clipboard?.writeText&&window.isSecureContext)await navigator.clipboard.writeText(s);else{const t=document.createElement('textarea');t.value=s;t.style.position='fixed';t.style.left='-10000px';document.body.appendChild(t);t.select();if(!document.execCommand('copy'))throw new Error('copy failed');t.remove()}toast('Copied to clipboard.')}catch(e){toast('Copy blocked here. Use HTTPS or export the draft.')}}
function getDraft(){return{id:editingId||('draft-'+Date.now()+'-'+Math.random().toString(36).slice(2,6)),service:mode,type:$('eventType').value,observedLocal:$('eventTime').value,lat:eventPos?.lat??null,lon:eventPos?.lng??null,place:(mode==='mping'?$('mpPlace').value:$('place').value).trim(),magnitude:$('magnitude').value,units:$('units').value,basis:$('verification').value,notes:$('details').value.trim(),name:$('reporter').value.trim(),spotterId:$('spotterId').value.trim(),savedAt:new Date().toISOString()}}
function validate(d,forSubmit=false){if(!Number.isFinite(d.lat)||!Number.isFinite(d.lon)){toast('Choose the observed event location with GPS or map.');return false;}if(!d.observedLocal){toast('Enter the observation date and time.');return false;}
const t=new Date(d.observedLocal);if(!Number.isFinite(t.getTime())){toast('Enter a valid observation time.');return false;}if(t.getTime()-Date.now()>5*60000){toast('The event time appears to be in the future.');return false;}
if(forSubmit&&d.service!=='mping'&&!$('firsthand').checked){toast('Confirm this is your own genuine observation before opening submission.');return false;}
if(forSubmit&&d.service==='spotter'&&(Date.now()-t.getTime()>20*60000)){toast('Spotter Network only accepts reports within 20 minutes. Use NWS for older observations.');return false;}
if(forSubmit&&d.service==='spotter'&&d.type.includes('gust')&&(d.units!=='mph'||Number(d.magnitude)<=50||!d.magnitude||d.basis!=='Measured')){toast('Spotter Network requires measured wind gusts greater than 50 mph.');return false;}return true;}
function reportText(d){const humanTime=new Date(d.observedLocal).toLocaleString(undefined,{dateStyle:'medium',timeStyle:'short'});const lines=[`${modeNames[d.service]} REPORT (DRAFT — NOT YET SUBMITTED)`,`Event: ${d.type}`,`Time observed: ${humanTime} (local)`,`Event location: ${d.lat.toFixed(5)}, ${d.lon.toFixed(5)}`];if(d.place)lines.push(`Place: ${d.place}`);if(d.magnitude)lines.push(`Measurement: ${d.magnitude} ${d.units} (${d.basis})`);if(d.notes)lines.push(`Details: ${d.notes}`);if(d.name)lines.push(`Observer: ${d.name}`);if(d.spotterId)lines.push(`Spotter ID: ${d.spotterId}`);lines.push('Source: firsthand observation');return lines.join('\n')}
$('copyReport').addEventListener('click',async()=>{const d=getDraft();if(validate(d))await copyText(reportText(d));});
const storageKey='stormspotter-report-drafts-v1';let saved=[];try{let arr=JSON.parse(localStorage.getItem(storageKey)||'[]');if(Array.isArray(arr))saved=arr.filter(x=>x&&typeof x==='object').slice(0,250)}catch(e){toast('Device storage is unavailable; export reports to keep them.');}
function writeSaved(){try{localStorage.setItem(storageKey,JSON.stringify(saved))}catch(e){toast('Browser storage is unavailable; export your data instead.')}}
$('saveDraft').addEventListener('click',()=>{const d=getDraft();if(!validate(d))return;const idx=saved.findIndex(s=>s.id===d.id);if(idx<0)saved.unshift(d);else saved[idx]=d;editingId=null;writeSaved();renderSaved();toast('Report draft saved on this device. Not submitted.');});
function loadDraft(d){editingId=d.id;setMode(d.service in types?d.service:'nws');$('eventType').value=d.type;updateFields();$('eventTime').value=d.observedLocal||currentLocalInput();$('magnitude').value=d.magnitude||'';$('units').value=d.units||'inches';$('verification').value=d.basis||'Measured';$('place').value=d.place||'';$('mpPlace').value=d.place||'';$('details').value=d.notes||'';$('reporter').value=d.name||'';$('spotterId').value=d.spotterId||'';$('firsthand').checked=false;if(Number.isFinite(d.lat)&&Number.isFinite(d.lon))setEventPosition([d.lat,d.lon],true);toast('Draft loaded for editing.');$('panelScroll').scrollTo({top:0,behavior:'smooth'});}
function renderSaved(){const area=$('drafts');area.replaceChildren();draftsLayer.clearLayers();if(!saved.length){const el=document.createElement('div');el.className='help';el.textContent='No saved drafts yet.';area.appendChild(el);return;}
for(const d of saved){const row=document.createElement('div');row.className='draft';const desc=document.createElement('div');desc.className='detail';const strong=document.createElement('strong');strong.textContent=d.type+' · '+(modeNames[d.service]||d.service);const sm=document.createElement('small');sm.textContent=(d.observedLocal||'')+(d.place?' · '+d.place:'');desc.append(strong,sm);const edit=document.createElement('button');edit.className='btn ghost';edit.textContent='Edit';edit.addEventListener('click',()=>loadDraft(d));const del=document.createElement('button');del.className='btn danger';del.title='Delete this draft';del.textContent='×';del.addEventListener('click',()=>{if(!confirm('Delete this local draft?'))return;saved=saved.filter(a=>a.id!==d.id);writeSaved();renderSaved();toast('Draft deleted.');});row.append(desc,edit,del);area.appendChild(row);
if(Number.isFinite(d.lat)&&Number.isFinite(d.lon)){const color=d.service==='spotter'?'#f59e67':d.service==='nws'?'#c68df7':'#54dcac';L.circleMarker([d.lat,d.lon],{radius:6,color:'#e3eafa',weight:1,fillColor:color,fillOpacity:.82}).bindPopup('<b>Saved draft only</b><br>'+esc(d.type)+'<br>'+esc(d.observedLocal)).addTo(draftsLayer)} }
}
function download(name,content,type){const blob=new Blob([content],{type});const url=URL.createObjectURL(blob),a=document.createElement('a');a.href=url;a.download=name;document.body.appendChild(a);a.click();a.remove();setTimeout(()=>URL.revokeObjectURL(url),15000);}
$('exportBtn').addEventListener('click',()=>{if(!saved.length){toast('Save a report first.');return;}const fc={type:'FeatureCollection',features:saved.filter(s=>Number.isFinite(s.lat)&&Number.isFinite(s.lon)).map(s=>({type:'Feature',geometry:{type:'Point',coordinates:[s.lon,s.lat]},properties:{id:s.id,service:s.service,event:s.type,observedLocal:s.observedLocal,place:s.place,measurement:s.magnitude,units:s.units,basis:s.basis,notes:s.notes,name:s.name,spotterId:s.spotterId,status:'LOCAL_DRAFT_NOT_SUBMITTED'}}))};download('storm-spotter-drafts.geojson',JSON.stringify(fc,null,2),'application/geo+json');toast('Saved drafts exported as GeoJSON.');});
async function resolveWfo(){if(!eventPos||mode!=='nws')return;const pt=L.latLng(eventPos);const seq=++wfoRequest;wfoLookupState='loading';$('officeStatus').textContent='Finding NWS office for the event coordinates…';
try{const res=await fetch(`https://api.weather.gov/points/${pt.lat.toFixed(4)},${pt.lng.toFixed(4)}`,{headers:{Accept:'application/geo+json'}});if(!res.ok)throw new Error('NWS API '+res.status);const json=await res.json();if(seq!==wfoRequest||!eventPos||map.distance(pt,eventPos)>1500)return;const code=String(json.properties?.cwa||'').toUpperCase().replace(/[^A-Z]/g,'');if(!/^[A-Z]{3}$/.test(code))throw new Error('No NWS office');wfo=code;wfoPoint=pt;wfoLookupState='ok';$('officeStatus').textContent='NWS office for event location: '+code;updateHandoff();}catch(e){if(seq!==wfoRequest)return;wfo=null;wfoLookupState='failed';$('officeStatus').textContent='Could not determine NWS office automatically. Find your local office using the NWS link below.';updateHandoff();}}
function mkButton(label,action,kind='link'){const b=document.createElement('button');b.className='btn '+kind;b.type='button';b.textContent=label;b.addEventListener('click',action);return b}
function openLink(url){window.open(url,'_blank','noopener,noreferrer')}
function copyAndOpen(url){const d=getDraft();if(!validate(d,true))return;const txt=reportText(d).replace(' (DRAFT — NOT YET SUBMITTED)','');
// Open from the click handler to preserve popup permissions; clipboard is attempted afterward.
openLink(url);copyText(txt);toast('Official website opened. Paste your report and submit there. Nothing has been sent by this map.');}
function updateHandoff(){const area=$('submissionButtons');area.replaceChildren();if(mode==='spotter'){$('handoffInfo').textContent='Sign in to your trained Spotter Network account and file the report there. This map cannot submit through your account without the official authenticated workflow.';area.append(mkButton('Copy + Spotter Network login ↗',()=>copyAndOpen('https://www.spotternetwork.org/login'),'accent'),mkButton('Official reporting rules ↗',()=>openLink('https://www.spotternetwork.org/pages/reporting-guidelines'),'ghost'));
$('officeStatus').textContent='Spotter Network report submission requires passing its training.';
}else if(mode==='nws'){$('handoffInfo').textContent='A weather report sent to the NWS is not itself an issued LSR. Submit it to the responsible NWS office; NWS decides whether it becomes an LSR.';
const nwsLink='https://inws.ncep.noaa.gov/report/';area.append(mkButton('Copy + official NWS report form ↗',()=>copyAndOpen(nwsLink),'accent'));
if(wfo==='ILN')area.append(mkButton('Compose email · NWS Wilmington',()=>{const d=getDraft();if(!validate(d,true))return;const body=reportText(d).replace(' (DRAFT — NOT YET SUBMITTED)','');window.location.href='mailto:spotreport.iln@noaa.gov?subject='+encodeURIComponent('Firsthand storm report: '+d.type)+'&body='+encodeURIComponent(body);toast('Email draft opened; you must send it.');},'mint'));
area.append(mkButton('NWS Wilmington spotter instructions ↗',()=>openLink('https://www.weather.gov/iln/skywarn_report'),'ghost'));
if(eventPos&&!wfo&&wfoLookupState==='idle')resolveWfo();
}else{$('handoffInfo').textContent='mPING reports are submitted in the official mPING app, which determines its own GPS location. Your saved draft and orange marker are for your reference only; this map cannot transfer the report directly to the mPING app.';$('officeStatus').textContent='Use the current mPING app version.';area.append(mkButton('Open mPING iPhone app ↗',()=>{const d=getDraft();if(!validate(d))return;openLink('https://apps.apple.com/us/app/mping/id584383400')},'accent'),mkButton('mPING official website ↗',()=>openLink('https://mping.ou.edu/'),'ghost'),mkButton('Request API access ↗',()=>openLink('https://mping.ou.edu/static/mping/access.html'),'ghost'));}}
function escapePopup(v){return esc(String(v??'')).slice(0,700)}
async function loadLSR(){if(!$('showLSR').checked)return;lsrAbort?.abort();const ctr=new AbortController();lsrAbort=ctr;
 const bb=map.getBounds();const box=[bb.getWest(),bb.getSouth(),bb.getEast(),bb.getNorth()].map(x=>x.toFixed(5)).join(',');
 const qs=new URLSearchParams({where:'1=1',geometry:box,geometryType:'esriGeometryEnvelope',inSR:'4326',outSR:'4326',spatialRel:'esriSpatialRelIntersects',outFields:'*',returnGeometry:'true',resultRecordCount:'1000',f:'geojson'});
 $('lsrStatus').textContent='Loading published NWS LSRs in this map view…';
 try{const res=await fetch('https://mapservices.weather.noaa.gov/vector/rest/services/obs/nws_local_storm_reports/MapServer/0/query?'+qs,{signal:ctr.signal});if(!res.ok)throw Error('HTTP '+res.status);const data=await res.json();if(data.error)throw Error(data.error.message||'NWS service error');if(!Array.isArray(data.features))throw Error('Unexpected GeoJSON response');lsrGroup.clearLayers();let count=0;
 L.geoJSON(data,{pointToLayer:(f,latlng)=>L.circleMarker(latlng,{radius:6,weight:1,color:'#e9d4ff',fillColor:'#a86af2',fillOpacity:.83}),onEachFeature:(f,layer)=>{count++;const p=f.properties||{};const name=p.Description||p.DESCRIPTION||p.Event||p.EVENT||'Local Storm Report';const where=p.Location||p.LOCATION||'';const remark=p.Remarks||p.REMARKS||'';const when=p['Time Of Report (UTC)']||p.TimeOfReport||'';layer.bindPopup(`<strong>${escapePopup(name)}</strong><br>${escapePopup(where)}<br><small>${escapePopup(when)}</small><br>${escapePopup(remark)}`)}}).eachLayer(l=>lsrGroup.addLayer(l));
 $('lsrStatus').textContent=`${count} official 24-hour LSR markers in map view (NWS feed refreshes about every 30 minutes).`;
 }catch(e){if(e.name==='AbortError')return;$('lsrStatus').textContent='Official LSR feed unavailable: '+e.message+'. Map and reporting remain usable.';}}
$('showLSR').addEventListener('change',e=>{if(e.target.checked)loadLSR();else{lsrAbort?.abort();lsrGroup.clearLayers();$('lsrStatus').textContent='LSR layer off';}});
map.on('moveend',()=>{if($('showLSR').checked){clearTimeout(lsrTimer);lsrTimer=setTimeout(loadLSR,650)}});
$('collapseButton').addEventListener('click',()=>{let c=$('drawer').classList.toggle('collapsed');$('collapseButton').textContent=c?'▴':'▾';setTimeout(()=>map.invalidateSize(),250)});
$('drawer').addEventListener('click',e=>{if($('drawer').classList.contains('collapsed')&&!e.target.closest('button')){$('drawer').classList.remove('collapsed');$('collapseButton').textContent='▾';setTimeout(()=>map.invalidateSize(),250)}});
window.addEventListener('resize',()=>{$('collapseButton').style.display=innerWidth<=830?'inline-flex':'none';if(innerWidth>830)$('drawer').classList.remove('collapsed');map.invalidateSize()});$('collapseButton').style.display=innerWidth<=830?'inline-flex':'none';
$('eventType').addEventListener('change',updateFields);document.querySelectorAll('.tab').forEach(b=>b.addEventListener('click',()=>setMode(b.dataset.mode)));
setMode('spotter');renderSaved();setFollow(true);
})();
</script>
</body>
</html>
