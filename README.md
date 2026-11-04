[index(1).html](https://github.com/user-attachments/files/34372818/index.1.html)
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>ASG ERP</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@500;600;700&family=IBM+Plex+Sans:wght@400;500;600&display=swap">
<style>
/* Layout: dark control-room sidebar + working area. One app, four role dashboards (CEO, Director 1, Director 2, Manager).
   Palette: graphite neutrals, ASG red as the only accent; status colours (amber / blue / green) are separate and semantic. */
:root{
  --font-display:'Barlow Condensed','Arial Narrow',Arial,sans-serif;
  --font-body:'IBM Plex Sans',system-ui,-apple-system,'Segoe UI',sans-serif;
  --bg:#ECEDF0; --surface:#FFFFFF; --surface-2:#F5F5F7; --ink:#16181D; --muted:#5A606B; --line:#D8DADF; --grid:#E4E5E9;
  --accent:#C8102E; --accent-text:#B50E29; --accent-ink:#FFFFFF; --accent-soft:#FBE8EB;
  --side:#16181D; --side-ink:#ECEDF0; --side-muted:#9EA3AE; --side-line:#2B2E36; --side-hover:#22252C;
  --pending:#9A5B00; --pending-bg:#FCF0D9; --progress:#2557A7; --progress-bg:#E3ECFA; --done:#17744A; --done-bg:#DCF1E5;
  --bar:#474C57;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    --bg:#0D0F12; --surface:#161A1F; --surface-2:#1C2027; --ink:#ECEDF0; --muted:#99A1AD; --line:#2B3038; --grid:#252A32;
    --accent:#D92B47; --accent-text:#FF6E83; --accent-ink:#FFFFFF; --accent-soft:#3A1A20;
    --side:#0A0B0E; --side-ink:#ECEDF0; --side-muted:#8D939F; --side-line:#23262D; --side-hover:#16191F;
    --pending:#F0B04A; --pending-bg:#3A2D14; --progress:#85ADF2; --progress-bg:#1B2A47; --done:#62D296; --done-bg:#143426;
    --bar:#6D7482; color-scheme:dark;
  }
}
:root[data-theme="dark"]{
  --bg:#0D0F12; --surface:#161A1F; --surface-2:#1C2027; --ink:#ECEDF0; --muted:#99A1AD; --line:#2B3038; --grid:#252A32;
  --accent:#D92B47; --accent-text:#FF6E83; --accent-ink:#FFFFFF; --accent-soft:#3A1A20;
  --side:#0A0B0E; --side-ink:#ECEDF0; --side-muted:#8D939F; --side-line:#23262D; --side-hover:#16191F;
  --pending:#F0B04A; --pending-bg:#3A2D14; --progress:#85ADF2; --progress-bg:#1B2A47; --done:#62D296; --done-bg:#143426;
  --bar:#6D7482; color-scheme:dark;
}
*{box-sizing:border-box}
body{background:var(--bg);color:var(--ink);font:14px/1.5 var(--font-body);padding-block:16px;
  padding-left:max(16px,env(safe-area-inset-left,0px));padding-right:max(16px,env(safe-area-inset-right,0px))}
h1,h2,h3{font-family:var(--font-display);font-weight:600;margin:0;text-wrap:balance;letter-spacing:.01em}
p{margin:0}
button,input,select,textarea{font:inherit;color:inherit}
:focus-visible{outline:2px solid var(--accent-text);outline-offset:2px}
.muted{color:var(--muted)}
#app{max-width:1320px;margin:0 auto}
.boot{padding:64px 0;text-align:center;color:var(--muted)}

/* buttons + fields */
.btn{display:inline-flex;align-items:center;justify-content:center;gap:6px;border:1px solid transparent;border-radius:6px;padding:8px 14px;font-weight:500;cursor:pointer;background:var(--accent);color:var(--accent-ink);line-height:1.2}
.btn:hover{filter:brightness(1.08)}
.btn.ghost{background:transparent;color:var(--ink);border-color:var(--line)}
.btn.ghost:hover{background:var(--surface-2);filter:none}
.btn.sm{padding:5px 10px;font-size:13px}
.btn.link{background:none;border:0;padding:4px 6px;color:var(--accent-text)}
.btn.link.bad{color:var(--muted)}
.btn.link:hover{text-decoration:underline;filter:none}
.btn.paid{background:var(--done-bg);color:var(--done);border-color:transparent}
.btn.due{background:var(--pending-bg);color:var(--pending)}
.fld{display:flex;flex-direction:column;gap:4px;min-width:0}
.fld>label,.lbl-s{font-size:12px;font-weight:500;color:var(--muted);letter-spacing:.04em;text-transform:uppercase}
.fld input,.fld select,.fld textarea,.sel{background:var(--surface);border:1px solid var(--line);border-radius:6px;padding:8px 10px;width:100%;min-width:0}
.fld textarea{min-height:72px;resize:vertical}
.sel{width:auto;padding:6px 8px;cursor:pointer}
.row2{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:12px}
.err{color:var(--accent-text);font-size:13px;min-height:20px}

/* auth */
.auth{display:grid;grid-template-columns:minmax(0,1.1fr) minmax(0,1fr);gap:16px;align-items:stretch;min-height:560px}
.auth-brand{background:var(--side);color:var(--side-ink);border-radius:10px;padding:32px;display:flex;flex-direction:column;gap:24px;justify-content:space-between}
.brand{display:flex;align-items:center;gap:10px;font-family:var(--font-display);font-weight:700;font-size:22px;letter-spacing:.08em}
.brand .sq{display:grid;place-items:center;width:30px;height:30px;background:var(--accent);color:#fff;border-radius:4px;font-size:20px;letter-spacing:0}
.auth-brand h1{font-size:46px;line-height:1;text-transform:uppercase;margin-bottom:12px}
.auth-brand .lede{color:var(--side-muted);max-width:44ch}
.roles-list{display:flex;flex-direction:column;gap:0;border-top:1px solid var(--side-line)}
.roles-list div{display:grid;grid-template-columns:110px 1fr;gap:12px;padding:10px 0;border-bottom:1px solid var(--side-line);color:var(--side-muted)}
.roles-list b{color:var(--side-ink);font-family:var(--font-display);font-size:17px;font-weight:600;letter-spacing:.03em;text-transform:uppercase}
.offices{color:var(--side-muted);font-size:13px}
.auth-card{background:var(--surface);border:1px solid var(--line);border-radius:10px;padding:32px;display:flex;flex-direction:column;gap:18px;justify-content:center}
.tabs{display:flex;gap:4px;border-bottom:1px solid var(--line)}
.tabs button{background:none;border:0;border-bottom:2px solid transparent;padding:8px 14px;margin-bottom:-1px;cursor:pointer;color:var(--muted);font-weight:500}
.tabs button[aria-selected="true"]{color:var(--ink);border-bottom-color:var(--accent)}
.auth-card form{display:flex;flex-direction:column;gap:14px}
.note{font-size:12.5px;color:var(--muted)}

/* shell */
.shell{display:grid;grid-template-columns:232px minmax(0,1fr);gap:16px;align-items:start}
.side{position:sticky;top:max(16px,env(safe-area-inset-top,0px));background:var(--side);color:var(--side-ink);border-radius:10px;padding:16px;display:flex;flex-direction:column;gap:18px}
.who{padding:12px 0;border-top:1px solid var(--side-line);border-bottom:1px solid var(--side-line)}
.who .r{font-family:var(--font-display);text-transform:uppercase;letter-spacing:.06em;font-size:15px;color:var(--side-muted)}
.who .n{font-weight:500;overflow-wrap:anywhere}
.nav{display:flex;flex-direction:column;gap:2px}
.nav button{background:none;border:0;color:var(--side-muted);text-align:left;padding:9px 10px;border-radius:6px;cursor:pointer;font-weight:500}
.nav button:hover{background:var(--side-hover);color:var(--side-ink)}
.nav button[aria-current="page"]{background:var(--accent);color:#fff}
.side .out{background:none;border:1px solid var(--side-line);color:var(--side-muted);border-radius:6px;padding:8px;cursor:pointer}
.side .out:hover{color:var(--side-ink);background:var(--side-hover)}
.main{display:flex;flex-direction:column;gap:16px;min-width:0}
.top{display:flex;flex-wrap:wrap;gap:12px;align-items:flex-end;justify-content:space-between}
.top h1{font-size:32px;text-transform:uppercase;line-height:1.05}
.top .sub{color:var(--muted)}
.mono-sel{display:flex;align-items:center;gap:8px;color:var(--muted);font-size:13px}

/* content blocks */
.panel{background:var(--surface);border:1px solid var(--line);border-radius:10px;padding:16px;min-width:0}
.panel>h2{font-size:20px;text-transform:uppercase;margin-bottom:12px;display:flex;justify-content:space-between;align-items:baseline;gap:8px;flex-wrap:wrap}
.panel>h2 small{font-family:var(--font-body);font-size:13px;font-weight:400;text-transform:none;color:var(--muted);letter-spacing:0}
.tiles{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px}
.tile{background:var(--surface);border:1px solid var(--line);border-radius:10px;padding:14px;min-width:0}
.tile .lbl{display:flex;align-items:center;gap:6px;font-size:12px;font-weight:500;letter-spacing:.05em;text-transform:uppercase;color:var(--muted)}
.tile .val{font-family:var(--font-display);font-weight:600;font-size:36px;line-height:1.1;margin-top:4px;font-variant-numeric:tabular-nums;overflow-wrap:anywhere}
.tile .val.sm{font-size:26px;padding-top:6px}
.tile .sub{font-size:12.5px;color:var(--muted)}
.dot{width:8px;height:8px;border-radius:50%;display:inline-block;background:var(--muted)}
.dot.pending{background:var(--pending)}.dot.progress{background:var(--progress)}.dot.completed,.dot.paid{background:var(--done)}.dot.red{background:var(--accent)}
.grid2{display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:16px}
.report p{margin-bottom:10px;max-width:78ch}
.report p:last-child{margin-bottom:0}
.report b{font-variant-numeric:tabular-nums}
.banner{display:flex;flex-wrap:wrap;gap:10px;align-items:center;justify-content:space-between;background:var(--accent-soft);border-radius:8px;padding:10px 14px}

/* bar chart (HTML so text stays readable on phones) */
.chart{padding-top:10px}
.plot{position:relative;height:200px;margin-left:44px;border-bottom:1px solid var(--line)}
.gl{position:absolute;left:0;right:0;height:1px;background:var(--grid)}
.yt{position:absolute;left:-44px;width:38px;text-align:right;transform:translateY(50%);font-size:11px;color:var(--muted);font-variant-numeric:tabular-nums}
.cols{position:absolute;inset:0;display:grid;grid-template-columns:repeat(6,1fr);gap:12px;padding:0 6px}
.col{position:relative;height:100%;display:flex;align-items:flex-end;justify-content:center}
.cb{display:block;width:100%;max-width:56px;height:var(--h);background:var(--bar);border-radius:3px 3px 0 0}
.col.on .cb{background:var(--accent)}
.cv{position:absolute;left:-6px;right:-6px;text-align:center;bottom:calc(var(--h) + 4px);font-size:12px;font-weight:500;font-variant-numeric:tabular-nums}
.xl{display:grid;grid-template-columns:repeat(6,1fr);gap:12px;margin-left:44px;padding:6px 6px 0;font-size:12px;color:var(--muted);text-align:center}
.xl .on{color:var(--ink);font-weight:600}
.chart-cap{font-size:12px;color:var(--muted);margin:8px 0 0 44px}

/* stacked bars */
.stk{display:grid;grid-template-columns:minmax(80px,130px) minmax(0,1fr);gap:12px;align-items:center;padding:8px 0}
.stk+.stk{border-top:1px solid var(--grid)}
.stk .nm{font-weight:500;overflow-wrap:anywhere}
.stack{display:flex;height:24px;border-radius:5px;overflow:hidden;background:var(--surface-2);gap:2px}
.seg{display:grid;place-items:center;font-size:12px;font-weight:600;min-width:22px}
.seg.completed,.seg.paid{background:var(--done-bg);color:var(--done)}
.seg.progress{background:var(--progress-bg);color:var(--progress)}
.seg.pending,.seg.unpaid{background:var(--pending-bg);color:var(--pending)}
.legend{display:flex;flex-wrap:wrap;gap:4px 14px;font-size:12.5px;color:var(--muted);margin-top:8px}
.legend span{display:inline-flex;align-items:center;gap:6px}

/* tables */
.tw{overflow-x:auto}
table{border-collapse:collapse;width:100%;min-width:560px}
th{text-align:left;font-size:11.5px;font-weight:600;letter-spacing:.06em;text-transform:uppercase;color:var(--muted);padding:8px 10px;border-bottom:1px solid var(--line);white-space:nowrap}
td{padding:10px;border-bottom:1px solid var(--grid);vertical-align:top}
tr:last-child td{border-bottom:0}
tbody tr:hover{background:var(--surface-2)}
td.n,th.n{text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap}
tfoot td{border-top:1px solid var(--line);font-weight:600;background:var(--surface-2)}
.pn{font-weight:500}
.pn small{display:block;color:var(--muted);font-weight:400}
.pill{display:inline-flex;align-items:center;gap:6px;border-radius:99px;padding:2px 10px;font-size:12.5px;font-weight:500;white-space:nowrap}
.pill.pending,.pill.unpaid{background:var(--pending-bg);color:var(--pending)}
.pill.progress{background:var(--progress-bg);color:var(--progress)}
.pill.completed,.pill.paid{background:var(--done-bg);color:var(--done)}
.chip{display:inline-block;background:var(--surface-2);border:1px solid var(--line);border-radius:4px;padding:0 6px;margin:0 4px 4px 0;font-size:12.5px;white-space:nowrap}
.chip.more{color:var(--muted)}
.tag{display:inline-block;margin-left:6px;padding:0 6px;border-radius:3px;background:var(--surface-2);border:1px solid var(--line);color:var(--muted);font-size:10.5px;letter-spacing:.06em;text-transform:uppercase;vertical-align:middle}
.late{color:var(--pending);font-weight:600;font-size:12px}
.mini{height:5px;border-radius:3px;background:var(--surface-2);margin-top:5px;min-width:70px}
.mini i{display:block;height:100%;border-radius:3px;background:var(--done)}
.acts{white-space:nowrap}
.filters{display:flex;flex-wrap:wrap;gap:10px;align-items:center;margin-bottom:12px}
.empty{padding:28px 12px;text-align:center;color:var(--muted)}
.empty b{display:block;color:var(--ink);font-family:var(--font-display);font-size:20px;text-transform:uppercase;margin-bottom:2px}
.list{display:flex;flex-direction:column}
.list .it{display:flex;justify-content:space-between;gap:12px;padding:9px 0;border-top:1px solid var(--grid)}
.list .it:first-child{border-top:0}
.load{display:flex;align-items:center;gap:8px;min-width:120px}
.load .tr{flex:1;height:8px;background:var(--surface-2);border-radius:4px;overflow:hidden}
.load .tr i{display:block;height:100%;background:var(--progress);border-radius:4px}
.load b{font-variant-numeric:tabular-nums;min-width:1.5ch}


/* ERP roadmap cards */
.module-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(205px,1fr));gap:10px}
.module-card{display:flex;flex-direction:column;gap:6px;padding:12px;border:1px solid var(--line);border-radius:8px;background:var(--surface-2);min-width:0}
.module-card b{font-size:14px}
.module-card span{font-size:12.5px;color:var(--muted)}
.module-card em{font-style:normal;font-size:11px;color:var(--pending);font-weight:600}

/* modal */
.scrim{position:fixed;inset:0;background:rgba(8,9,12,.62);display:grid;place-items:center;padding:16px;z-index:50;overflow-y:auto}
.dlg{background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:20px;width:min(560px,100%);max-height:calc(100vh - 32px);overflow-y:auto;display:flex;flex-direction:column;gap:14px}
.dlg h2{font-size:24px;text-transform:uppercase}
.dlg form{display:flex;flex-direction:column;gap:14px}
.dlg .foot{display:flex;justify-content:flex-end;gap:8px;flex-wrap:wrap}
.team{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:6px}
.team label{display:flex;align-items:center;gap:8px;padding:6px 8px;border:1px solid var(--line);border-radius:6px;cursor:pointer;min-width:0}
.team label span{overflow-wrap:anywhere}
.team small{display:block;color:var(--muted);font-size:11.5px}
.gh{grid-column:1/-1;font-size:11.5px;letter-spacing:.06em;text-transform:uppercase;color:var(--muted);margin-top:4px}
.toast{position:fixed;left:50%;transform:translateX(-50%);bottom:calc(20px + env(safe-area-inset-bottom,0px));background:var(--ink);color:var(--bg);padding:9px 16px;border-radius:8px;z-index:60;max-width:calc(100vw - 32px)}

@media (max-width:900px){
  .auth{grid-template-columns:1fr;min-height:0}
  .auth-brand{padding:22px}.auth-brand h1{font-size:38px}
  .auth-card{padding:22px}
  .shell{grid-template-columns:1fr}
  .side{position:static;gap:12px}
  .nav{flex-direction:row;overflow-x:auto}
  .nav button{white-space:nowrap}
  .top h1{font-size:28px}
}
@media (max-width:480px){
  .roles-list div{grid-template-columns:1fr;gap:0}
  .stk{grid-template-columns:1fr;gap:4px}
}
@media (prefers-reduced-motion:no-preference){.btn,.nav button{transition:background .12s,filter .12s}}
</style>
<style>html{color-scheme:light dark}body{margin:0}[hidden]{display:none!important}img{max-width:100%}</style>
</head>
<body>


<div id="app"><div class="boot">Connecting to ASG ERP…</div></div>
<div id="modal"></div>
<div id="toast" class="toast" role="status" aria-live="polite" hidden></div>

<script>
(function(){
'use strict';
var LS='asg-erp-state-v1', SS='asg-erp-session';
var ROLES={ceo:'CEO',dir1:'Director 1',dir2:'Director 2',manager:'Manager'};
var STATUS={pending:'Pending',progress:'In progress',completed:'Completed'};
var CATS=['Electric fence','Boom barrier','CCTV & surveillance','Access control','Fire alarm','Gate automation','Parking management','Smart building','Other'];
var CITIES=['Lahore','Islamabad','Karachi','Rawalpindi','Faisalabad','Multan','Other'];
var NAV={
  ceo:[['overview','Overview'],['projects','Projects'],['salaries','Salaries'],['accounts','Accounts']],
  dir1:[['overview','Dashboard'],['projects','My projects'],['salaries','Salaries']],
  dir2:[['overview','Dashboard'],['projects','My projects'],['salaries','Salaries']],
  manager:[['overview','Overview'],['projects','Projects'],['workload','Workload']]
};
var S={users:[],projects:[],staff:[],erp:{products:[],categories:[],brands:[],units:[],warehouses:[],customers:[],suppliers:[],stockIn:[],stockOut:[],sales:[],purchases:[],expenses:[],income:[],payments:[],employees:[],documents:[],activities:[]}};
var todayStr=new Date().toLocaleDateString('en-CA');
var curMonth=todayStr.slice(0,7);
var ui={user:null,view:'overview',month:curMonth,ready:false,mode:'connecting',note:'',modal:null,fStatus:'all',fDir:'all',authTab:'login',authDrawn:false};
var dbDoc=null,writing=false,again=false;

/* ---------- helpers ---------- */
function $(s){return document.querySelector(s)}
function esc(s){return String(s==null?'':s).replace(/[&<>"']/g,function(c){return{'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]})}
function uid(p){return p+Math.random().toString(36).slice(2,8)+Date.now().toString(36).slice(-3)}
function money(n){return 'Rs '+Math.round(+n||0).toLocaleString('en-IN')}
function lakh(n){n=+n||0;if(n>=1e7)return 'Rs '+(n/1e7).toFixed(2).replace(/\.?0+$/,'')+' Crore';if(n>=1e5)return 'Rs '+(n/1e5).toFixed(1).replace(/\.0$/,'')+' Lakh';return money(n)}
function shortL(v){v=+v||0;if(v>=1e7)return (v/1e7).toFixed(1).replace(/\.0$/,'')+'Cr';if(v>=1e5)return (v/1e5).toFixed(v>=1e6?0:1).replace(/\.0$/,'')+'L';if(v>=1000)return Math.round(v/1000)+'k';return String(Math.round(v))}
function niceMax(v){var e=Math.pow(10,Math.floor(Math.log10(v))),f=v/e;return (f<=1?1:f<=2?2:f<=5?5:10)*e}
function plural(n,w){return n+' '+w+(n===1?'':'s')}
function fmtDate(s){if(!s)return '—';var d=new Date(s+'T00:00:00');return isNaN(d)?'—':d.toLocaleDateString('en-GB',{day:'2-digit',month:'short',year:'numeric'})}
function addM(k,d){var p=k.split('-'),dt=new Date(+p[0],+p[1]-1+d,1);return dt.getFullYear()+'-'+String(dt.getMonth()+1).padStart(2,'0')}
function monthLabel(k){var p=k.split('-');return new Date(+p[0],+p[1]-1,1).toLocaleDateString('en-GB',{month:'long',year:'numeric'})}
function monthShort(k){var p=k.split('-');return new Date(+p[0],+p[1]-1,1).toLocaleDateString('en-GB',{month:'short'})}
function monthOptions(){var o='';for(var i=0;i<12;i++){var k=addM(curMonth,-i);o+='<option value="'+k+'"'+(k===ui.month?' selected':'')+'>'+monthLabel(k)+'</option>'}return o}
function stf(id){return S.staff.find(function(x){return x.id===id})}
function first(n){return String(n).split(' ')[0]}
function dirLabel(d){var u=S.users.find(function(x){return x.role===d});return ROLES[d]+(u?' · '+u.name:'')}
function toast(msg){var t=$('#toast');t.textContent=msg;t.hidden=false;clearTimeout(toast.t);toast.t=setTimeout(function(){t.hidden=true},2600)}
function hashPw(user,pw){
  var txt='asg-erp|'+user+'|'+pw;
  try{
    return crypto.subtle.digest('SHA-256',new TextEncoder().encode(txt)).then(function(buf){
      return Array.prototype.map.call(new Uint8Array(buf),function(b){return b.toString(16).padStart(2,'0')}).join('');
    });
  }catch(e){var h=5381;for(var i=0;i<txt.length;i++){h=((h<<5)+h+txt.charCodeAt(i))>>>0}return Promise.resolve('x'+h.toString(16))}
}

/* ---------- metrics ---------- */
function revOf(P,k){var t=0;P.forEach(function(p){(p.payments||[]).forEach(function(x){if(x.m===k)t+=+x.amt||0})});return t}
function metrics(P,month){
  var m={total:P.length,pending:0,progress:0,completed:0,started:0,doneMonth:0,revMonth:0,revAll:0,valueAll:0,outstanding:0};
  P.forEach(function(p){
    m[p.status]=(m[p.status]||0)+1;
    if((p.start||'').slice(0,7)===month)m.started++;
    if(p.status==='completed'&&(p.done||'').slice(0,7)===month)m.doneMonth++;
    m.valueAll+=+p.value||0;
    (p.payments||[]).forEach(function(x){m.revAll+=+x.amt||0;if(x.m===month)m.revMonth+=+x.amt||0});
  });
  m.outstanding=Math.max(0,m.valueAll-m.revAll);
  return m;
}
function salaryStats(list,month){
  var s={count:list.length,paid:0,unpaid:0,paidAmt:0,unpaidAmt:0,total:0};
  list.forEach(function(x){s.total+=+x.salary||0;if(x.paid&&x.paid[month]){s.paid++;s.paidAmt+=+x.salary||0}else{s.unpaid++;s.unpaidAmt+=+x.salary||0}});
  return s;
}
function activeCount(id){return S.projects.filter(function(p){return p.status!=='completed'&&(p.team||[]).indexOf(id)>-1}).length}
var ORD={progress:0,pending:1,completed:2};
function sortP(l){return l.slice().sort(function(a,b){return (ORD[a.status]-ORD[b.status])||String(a.due||'9').localeCompare(String(b.due||'9'))})}

/* ---------- storage ---------- */
function persist(){
  if(dbDoc){
    if(writing){again=true;return}
    writing=true;
    dbDoc.set(JSON.parse(JSON.stringify(S))).then(function(){},function(){toast('Could not save. Check your connection and try again.')}).then(function(){
      writing=false;if(again){again=false;persist()}
    });
  }else{try{localStorage.setItem(LS,JSON.stringify(S))}catch(e){}}
}
function commit(fn){fn();persist();render()}
function applyRemote(d){
  S={users:d.users||[],projects:d.projects||[],staff:d.staff||[],erp:d.erp||{}};
  if(ui.user){var u=S.users.find(function(x){return x.id===ui.user.id});if(u)ui.user=u;else{logout(true)}}
}
function boot(){
  var p;
  try{p=(window.claude&&window.claude.use)?window.claude.use('db'):Promise.resolve(null)}catch(e){p=Promise.resolve(null)}
  p.catch(function(){return null}).then(function(db){
    if(db){
      ui.mode='shared';
      dbDoc=db.doc('erp/state');
      var timer=setTimeout(function(){if(!ui.ready){ui.ready=true;ui.note='Shared data could not be reached yet. What you see may be out of date.';render()}},10000);
      dbDoc.onSnapshot(function(snap){
        applyRemote(snap.exists?JSON.parse(JSON.stringify(snap.data())):{});
        if(!snap.metadata.fromCache)clearTimeout(timer);
        if(!ui.ready){ui.ready=true;restoreSession()}
        render();
      },function(){ui.ready=true;ui.note='Shared data is not available right now.';render()});
    }else{
      ui.mode='local';
      try{var raw=localStorage.getItem(LS);if(raw)applyRemote(JSON.parse(raw))}catch(e){}
      ui.ready=true;restoreSession();render();
    }
  });
}
function restoreSession(){
  try{var id=sessionStorage.getItem(SS);if(id){var u=S.users.find(function(x){return x.id===id});if(u){ui.user=u;ui.view='overview';ui.month=curMonth}}}catch(e){}
}

/* ---------- auth ---------- */
function startSession(u){
  ui.user=u;ui.view='overview';ui.month=curMonth;ui.fStatus='all';ui.fDir='all';
  try{sessionStorage.setItem(SS,u.id)}catch(e){}
  render();scrollTo(0,0);
}
function logout(silent){
  ui.user=null;ui.authDrawn=false;ui.authTab='login';closeModal();
  try{sessionStorage.removeItem(SS)}catch(e){}
  render();if(!silent)scrollTo(0,0);
}
function setErr(t){var e=$('#auth-err');if(e)e.textContent=t||''}
function doLogin(){
  var u=$('#li-user').value.trim().toLowerCase(),p=$('#li-pass').value;
  hashPw(u,p).then(function(h){
    var user=S.users.find(function(x){return x.username===u&&x.pass===h});
    if(!user){setErr('Username or password is not correct.');return}
    startSession(user);
  });
}
function doSignup(){
  var name=$('#su-name').value.trim(),user=$('#su-user').value.trim().toLowerCase(),pass=$('#su-pass').value,role=$('#su-role').value;
  if(!/^[a-z0-9._-]{3,24}$/.test(user)){setErr('Username needs 3 to 24 letters, numbers, dots, dashes or underscores.');return}
  if(pass.length<6){setErr('Password needs at least 6 characters.');return}
  if(!ROLES[role]){setErr('Choose a role.');return}
  if(S.users.some(function(x){return x.username===user})){setErr('That username is taken. Choose another or log in.');return}
  if(role!=='manager'&&S.users.some(function(x){return x.role===role})){setErr(ROLES[role]+' already has an account. Log in, or ask the CEO to remove it.');return}
  hashPw(user,pass).then(function(h){
    var rec={id:uid('u_'),name:name,username:user,pass:h,role:role,created:todayStr};
    commit(function(){S.users.push(rec)});
    startSession(rec);
  });
}
function roleOptions(){
  return ['ceo','dir1','dir2','manager'].map(function(r){
    var taken=r!=='manager'&&S.users.some(function(x){return x.role===r});
    return '<option value="'+r+'"'+(taken?' disabled':'')+'>'+ROLES[r]+(taken?' (account exists)':'')+'</option>';
  }).join('');
}
function refreshRoles(){var s=$('#su-role');if(s){var v=s.value;s.innerHTML=roleOptions();if(!s.querySelector('option[value="'+v+'"]:not([disabled])'))s.value='manager';else s.value=v}}
function drawAuth(){
  var login=ui.authTab==='login';
  var modeNote=ui.mode==='shared'?'Data is shared: the CEO, Directors and Managers all see the same records.':'Local mode: records are kept in this browser only.';
  $('#app').innerHTML=
  '<div class="auth">'+
   '<section class="auth-brand">'+
    '<div class="brand"><span class="sq">A</span><span>ASG SYSTEMS</span></div>'+
    '<div><h1>Operations ERP</h1><p class="lede">A role-based business operations workspace for ASG Systems, with separate dashboards for the CEO, Directors and Manager.</p></div>'+
    '<div class="roles-list">'+
     '<div><b>CEO</b><span>Company-wide oversight of project delivery, contract portfolio, collections, payroll and ERP expansion.</span></div>'+
     '<div><b>Director 1</b><span>Manages own project portfolio, collections, team and payroll.</span></div>'+
     '<div><b>Director 2</b><span>Manages own project portfolio, collections, team and payroll.</span></div>'+
     '<div><b>Manager</b><span>Monitors delivery status, overdue work, team allocation and workload.</span></div>'+
    '</div>'+
    '<p class="offices">Lahore · Islamabad · Karachi</p>'+
   '</section>'+
   '<section class="auth-card">'+
    '<div class="tabs" role="tablist"><button role="tab" data-act="authtab" data-tab="login" aria-selected="'+login+'">Log in</button><button role="tab" data-act="authtab" data-tab="signup" aria-selected="'+!login+'">Create account</button></div>'+
    (login?
     '<form id="f-login"><div class="fld"><label for="li-user">Username</label><input id="li-user" autocomplete="username" required></div>'+
     '<div class="fld"><label for="li-pass">Password</label><input id="li-pass" type="password" autocomplete="current-password" required></div>'+
     '<div class="err" id="auth-err" role="alert"></div><button class="btn" type="submit">Log in</button>'+
     (S.users.length?'':'<p class="note">No accounts exist yet. Choose “Create account” to register the CEO, the two Directors and the Manager.</p>')+'</form>':
     '<form id="f-signup"><div class="fld"><label for="su-name">Full name</label><input id="su-name" autocomplete="name" required></div>'+
     '<div class="row2"><div class="fld"><label for="su-user">Username</label><input id="su-user" autocomplete="username" required></div>'+
     '<div class="fld"><label for="su-pass">Password</label><input id="su-pass" type="password" autocomplete="new-password" required></div></div>'+
     '<div class="fld"><label for="su-role">Role</label><select id="su-role">'+roleOptions()+'</select></div>'+
     '<p class="note">CEO, Director 1 and Director 2 each have one account. Several Managers can register.</p>'+
     '<div class="err" id="auth-err" role="alert"></div><button class="btn" type="submit">Create account</button></form>')+
    '<p class="note">'+esc(modeNote)+(ui.note?' '+esc(ui.note):'')+'</p>'+
   '</section>'+
  '</div>';
  ui.authDrawn=true;
  if(!login){var s=$('#su-role');if(s){var o=s.querySelector('option:not([disabled])');if(o)s.value=o.value}}
}

/* ---------- small view parts ---------- */
function tile(l,v,sub,tone,sm){return '<div class="tile"><div class="lbl">'+(tone?'<i class="dot '+tone+'"></i>':'')+l+'</div><div class="val'+(sm?' sm':'')+'">'+v+'</div>'+(sub?'<div class="sub">'+sub+'</div>':'')+'</div>'}
function pill(k,label){return '<span class="pill '+k+'">'+(label||STATUS[k])+'</span>'}
function empty(t,s){return '<div class="empty"><b>'+t+'</b>'+(s||'')+'</div>'}
function sampleTag(x){return x.sample?'<span class="tag">Sample</span>':''}
function chips(team){
  var names=(team||[]).map(stf).filter(Boolean).map(function(x){return first(x.name)});
  if(!names.length)return '<span class="muted">No team yet</span>';
  return names.slice(0,3).map(function(n){return '<span class="chip">'+esc(n)+'</span>'}).join('')+(names.length>3?'<span class="chip more">+'+(names.length-3)+'</span>':'');
}
function revChart(P,month){
  var keys=[0,1,2,3,4,5].map(function(i){return addM(month,i-5)});
  var vals=keys.map(function(k){return revOf(P,k)});
  var max=niceMax(Math.max.apply(null,vals.concat([1])));
  var total=vals.reduce(function(a,b){return a+b},0);
  return '<div class="chart" role="img" aria-label="Revenue received in each of the last six months">'+
   '<div class="plot"><i class="gl" style="top:0"></i><i class="gl" style="top:50%"></i>'+
   '<span class="yt" style="bottom:0">0</span><span class="yt" style="bottom:50%">'+shortL(max/2)+'</span><span class="yt" style="bottom:100%">'+shortL(max)+'</span>'+
   '<div class="cols">'+keys.map(function(k,i){var h=(vals[i]/max*100).toFixed(1)+'%';return '<div class="col'+(k===month?' on':'')+'" style="--h:'+h+'"><span class="cv">'+(vals[i]?shortL(vals[i]):'')+'</span><span class="cb"></span></div>'}).join('')+'</div></div>'+
   '<div class="xl">'+keys.map(function(k){return '<span'+(k===month?' class="on"':'')+'>'+monthShort(k)+'</span>'}).join('')+'</div>'+
   '<p class="chart-cap">Amounts in Rs (L = lakh). Six-month total '+lakh(total)+'.</p></div>';
}
function statusStack(m){
  var tot=m.pending+m.progress+m.completed;
  if(!tot)return '<div class="stack"></div>';
  return '<div class="stack">'+['completed','progress','pending'].map(function(k){return m[k]?'<span class="seg '+k+'" style="flex:'+m[k]+'" title="'+STATUS[k]+': '+m[k]+'">'+m[k]+'</span>':''}).join('')+'</div>';
}
function twoStack(a,b,ka,kb){
  if(!a&&!b)return '<div class="stack"></div>';
  return '<div class="stack">'+(a?'<span class="seg '+ka+'" style="flex:'+a+'" title="'+ka+': '+a+'">'+a+'</span>':'')+(b?'<span class="seg '+kb+'" style="flex:'+b+'" title="'+kb+': '+b+'">'+b+'</span>':'')+'</div>';
}
var LEG_STATUS='<div class="legend"><span><i class="dot completed"></i>Completed</span><span><i class="dot progress"></i>In progress</span><span><i class="dot pending"></i>Pending</span></div>';
var LEG_SAL='<div class="legend"><span><i class="dot paid"></i>Paid</span><span><i class="dot pending"></i>Unpaid</span></div>';
function monthSel(){return '<label class="mono-sel">Month <select class="sel" data-change="month" aria-label="Month">'+monthOptions()+'</select></label>'}

/* ---------- tables ---------- */
function filtered(list,withDir){
  return list.filter(function(p){return (ui.fStatus==='all'||p.status===ui.fStatus)&&(!withDir||ui.fDir==='all'||p.dir===ui.fDir)});
}
function filterBar(withDir){
  return '<div class="filters"><label class="mono-sel">Status <select class="sel" data-change="fstatus"><option value="all">All</option>'+Object.keys(STATUS).map(function(k){return '<option value="'+k+'"'+(ui.fStatus===k?' selected':'')+'>'+STATUS[k]+'</option>'}).join('')+'</select></label>'+
  (withDir?'<label class="mono-sel">Director <select class="sel" data-change="fdir"><option value="all">Both</option><option value="dir1"'+(ui.fDir==='dir1'?' selected':'')+'>Director 1</option><option value="dir2"'+(ui.fDir==='dir2'?' selected':'')+'>Director 2</option></select></label>':'')+'</div>';
}
function dueCell(p){
  var late=p.status!=='completed'&&p.due&&p.due<todayStr;
  return fmtDate(p.due)+(late?'<br><span class="late">Overdue</span>':'');
}
function statusSelect(p){
  return '<select class="sel" data-change="status" data-id="'+p.id+'" aria-label="Status of '+esc(p.name)+'">'+Object.keys(STATUS).map(function(k){return '<option value="'+k+'"'+(p.status===k?' selected':'')+'>'+STATUS[k]+'</option>'}).join('')+'</select>';
}
function recvCell(p){
  var rec=(p.payments||[]).reduce(function(a,x){return a+(+x.amt||0)},0),pct=p.value?Math.min(100,Math.round(rec/p.value*100)):0;
  return money(rec)+'<div class="mini" title="'+pct+'% received"><i style="width:'+pct+'%"></i></div>';
}
function projTable(list,mode){
  if(!list.length)return empty('No projects to show',mode==='dir'?'Use “Add project” to create the first one.':'Projects added by the Directors appear here.');
  var head,rows;
  if(mode==='dir'){
    head='<th>Project</th><th>Status</th><th class="n">Contract value</th><th class="n">Received</th><th>Due</th><th>Team</th><th></th>';
    rows=list.map(function(p){return '<tr><td class="pn">'+esc(p.name)+sampleTag(p)+'<small>'+esc(p.client)+' · '+esc(p.category)+' · '+esc(p.city)+'</small></td><td>'+statusSelect(p)+'</td><td class="n">'+money(p.value)+'</td><td class="n">'+recvCell(p)+'</td><td>'+dueCell(p)+'</td><td>'+chips(p.team)+'</td><td class="acts"><button class="btn link" data-act="pay" data-id="'+p.id+'">Payment</button><button class="btn link" data-act="edit-project" data-id="'+p.id+'">Edit</button><button class="btn link bad" data-act="ask-del" data-kind="project" data-id="'+p.id+'">Delete</button></td></tr>'});
  }else if(mode==='mgr'){
    head='<th>Project</th><th>Director</th><th>Status</th><th>Team</th><th>Due</th><th></th>';
    rows=list.map(function(p){return '<tr><td class="pn">'+esc(p.name)+sampleTag(p)+'<small>'+esc(p.client)+' · '+esc(p.city)+'</small></td><td>'+ROLES[p.dir]+'</td><td>'+statusSelect(p)+'</td><td>'+chips(p.team)+(p.team&&p.team.length?'<small class="muted"> '+plural(p.team.length,'person')+'</small>':'')+'</td><td>'+dueCell(p)+'</td><td class="acts"><button class="btn link" data-act="assign" data-id="'+p.id+'">Team &amp; notes</button></td></tr>'});
  }else{
    head='<th>Project</th><th>Director</th><th>Status</th><th class="n">Contract value</th><th class="n">Received</th><th>Due</th><th>Team</th>';
    rows=list.map(function(p){return '<tr><td class="pn">'+esc(p.name)+sampleTag(p)+'<small>'+esc(p.client)+' · '+esc(p.category)+' · '+esc(p.city)+'</small></td><td>'+ROLES[p.dir]+'</td><td>'+pill(p.status)+'</td><td class="n">'+money(p.value)+'</td><td class="n">'+recvCell(p)+'</td><td>'+dueCell(p)+'</td><td>'+chips(p.team)+'</td></tr>'});
  }
  return '<div class="tw"><table><thead><tr>'+head+'</tr></thead><tbody>'+rows.join('')+'</tbody></table></div>';
}
function salTable(list,mode){
  if(!list.length)return empty('No employees yet',mode==='dir'?'Use “Add employee” to start your payroll list.':'Directors add their employees and salaries.');
  var s=salaryStats(list,ui.month);
  var head='<th>Employee</th>'+(mode==='ceo'?'<th>Director</th>':'')+'<th>Role</th><th class="n">Monthly salary</th><th>'+monthShort(ui.month)+' status</th>'+(mode==='dir'?'<th></th>':'');
  var rows=list.map(function(x){
    var paid=x.paid&&x.paid[ui.month];
    return '<tr><td class="pn">'+esc(x.name)+sampleTag(x)+'</td>'+(mode==='ceo'?'<td>'+ROLES[x.dir]+'</td>':'')+'<td>'+esc(x.role)+'</td><td class="n">'+money(x.salary)+'</td><td>'+(paid?pill('paid','Paid '+fmtDate(paid)):pill('unpaid','Not paid yet'))+'</td>'+
    (mode==='dir'?'<td class="acts"><button class="btn sm '+(paid?'ghost':'')+'" data-act="toggle-paid" data-id="'+x.id+'">'+(paid?'Mark unpaid':'Mark paid')+'</button><button class="btn link" data-act="edit-staff" data-id="'+x.id+'">Edit</button><button class="btn link bad" data-act="ask-del" data-kind="staff" data-id="'+x.id+'">Remove</button></td>':'')+'</tr>';
  });
  var cols=(mode==='ceo'?2:1)+1;
  return '<div class="tw"><table><thead><tr>'+head+'</tr></thead><tbody>'+rows.join('')+'</tbody><tfoot><tr><td colspan="'+cols+'">Payroll '+monthLabel(ui.month)+'</td><td class="n">'+money(s.total)+'</td><td colspan="'+(mode==='dir'?2:1)+'">'+s.paid+' paid · '+s.unpaid+' unpaid ('+money(s.unpaidAmt)+')</td></tr></tfoot></table></div>';
}

/* ---------- views ---------- */
function moduleReadiness(){
  var items=[
    ['Procurement & Suppliers','Supplier records, RFQs, purchase orders and approvals'],
    ['Imports & Landed Cost','Shipments, customs, freight, duties and landed-cost allocation'],
    ['Inventory & Warehouses','Multi-warehouse stock, serial numbers, transfers and stock valuation'],
    ['Sales & CRM','Leads, quotations, sales orders, deliveries and customer balances'],
    ['Solutions & Projects','BOQ/BOM, project budgets, installation and completion'],
    ['Government Tenders','Bid pipeline, EMD, guarantees, deadlines and contracts'],
    ['Finance & Accounting','Double-entry ledger, bank reconciliation, tax and financial statements'],
    ['Warranty & Support','Serial-based warranty, RMA, AMC and service tickets']
  ];
  return '<section class="panel"><h2>ERP business modules <small>Expansion roadmap</small></h2><div class="module-grid">'+items.map(function(x){
    return '<div class="module-card"><b>'+esc(x[0])+'</b><span>'+esc(x[1])+'</span><em>Not connected to this version’s data yet</em></div>';
  }).join('')+'</div><p class="note" style="margin-top:12px">These are planned ERP areas, not active transaction modules. No sales, stock, import or accounting figures are fabricated in this dashboard.</p></section>';
}
function viewCeo(){
  var L=monthLabel(ui.month),all=metrics(S.projects,ui.month),sal=salaryStats(S.staff,ui.month);
  var per=['dir1','dir2'].map(function(d){return{d:d,m:metrics(S.projects.filter(function(p){return p.dir===d}),ui.month),s:salaryStats(S.staff.filter(function(x){return x.dir===d}),ui.month)}});
  var hasSample=S.projects.some(function(p){return p.sample})||S.staff.some(function(x){return x.sample});
  var out='';
  if(hasSample)out+='<div class="banner"><span>Sample data is loaded so you can see the dashboards filled in. Real records you add are kept when you clear it.</span><button class="btn sm ghost" data-act="ask-del" data-kind="sample" data-id="x">Clear sample data</button></div>';
  out+='<div class="banner"><span><b>CEO control room</b> · Company-wide project delivery, collections and payroll overview.</span>'+monthSel()+'</div>';
  out+='<div class="tiles">'+
    tile('Total projects',all.total,'across both directors','red')+
    tile('New projects',all.started,'started in '+L,'progress')+
    tile('Completed',all.completed,all.doneMonth+' completed this month','completed')+
    tile('In progress',all.progress,'active delivery','progress')+
    tile('Pending',all.pending,'awaiting commencement','pending')+
    tile('Contract portfolio',lakh(all.valueAll),'total recorded project value','',true)+
    tile('Collections this month',lakh(all.revMonth),'cash received in '+monthShort(ui.month),'completed',true)+
    tile('Outstanding contracts',lakh(all.outstanding),'recorded contract value less receipts','pending',true)+
    tile('Unpaid payroll',lakh(sal.unpaidAmt),sal.unpaid+' employees unpaid in '+monthShort(ui.month),'pending',true)+'</div>';
  var rep='<p>During <b>'+L+'</b>, ASG started <b>'+plural(all.started,'project')+'</b>: '+per[0].m.started+' under '+dirLabel('dir1')+' and '+per[1].m.started+' under '+dirLabel('dir2')+'.</p>'+
    '<p>The company has <b>'+all.total+'</b> recorded projects: <b>'+all.completed+'</b> completed, <b>'+all.progress+'</b> in progress and <b>'+all.pending+'</b> pending.</p>'+
    '<p>Recorded contract value is <b>'+money(all.valueAll)+'</b>; receipts total <b>'+money(all.revAll)+'</b>, leaving <b>'+money(all.outstanding)+'</b> outstanding against these project records.</p>'+
    '<p>For '+L+', <b>'+sal.paid+' of '+sal.count+'</b> employee salaries are marked paid. Unpaid payroll is <b>'+money(sal.unpaidAmt)+'</b>.</p>';
  out+='<div class="grid2"><section class="panel report"><h2>Executive summary <small>'+L+'</small></h2>'+rep+'</section>'+
    '<section class="panel"><h2>Collections trend <small>last 6 months</small></h2>'+revChart(S.projects,ui.month)+'</section></div>';
  out+='<div class="grid2"><section class="panel"><h2>Project delivery by director</h2>'+per.map(function(x){return '<div class="stk"><span class="nm">'+dirLabel(x.d)+'</span>'+statusStack(x.m)+'</div>'}).join('')+LEG_STATUS+'</section>'+
    '<section class="panel"><h2>Payroll by director <small>'+L+'</small></h2>'+per.map(function(x){return '<div class="stk"><span class="nm">'+dirLabel(x.d)+'</span>'+twoStack(x.s.paid,x.s.unpaid,'paid','unpaid')+'</div>'}).join('')+LEG_SAL+'</section></div>';
  out+='<section class="panel"><h2>Director scorecard <small>'+L+'</small></h2><div class="tw"><table><thead><tr><th>Director</th><th class="n">Projects</th><th class="n">New</th><th class="n">In progress</th><th class="n">Pending</th><th class="n">Completed</th><th class="n">Collections</th><th class="n">Payroll paid</th><th class="n">Payroll unpaid</th></tr></thead><tbody>'+
    per.map(function(x){return '<tr><td class="pn">'+dirLabel(x.d)+'</td><td class="n">'+x.m.total+'</td><td class="n">'+x.m.started+'</td><td class="n">'+x.m.progress+'</td><td class="n">'+x.m.pending+'</td><td class="n">'+x.m.completed+'</td><td class="n">'+money(x.m.revMonth)+'</td><td class="n">'+x.s.paid+'</td><td class="n">'+x.s.unpaid+'</td></tr>'}).join('')+
    '</tbody><tfoot><tr><td>Total</td><td class="n">'+all.total+'</td><td class="n">'+all.started+'</td><td class="n">'+all.progress+'</td><td class="n">'+all.pending+'</td><td class="n">'+all.completed+'</td><td class="n">'+money(all.revMonth)+'</td><td class="n">'+sal.paid+'</td><td class="n">'+sal.unpaid+'</td></tr></tfoot></table></div></section>';
  out+=moduleReadiness();
  return out;
}
function viewDirOverview(){
  var d=ui.user.role,P=S.projects.filter(function(p){return p.dir===d}),T=S.staff.filter(function(x){return x.dir===d});
  var m=metrics(P,ui.month),s=salaryStats(T,ui.month),L=monthLabel(ui.month);
  var open=function(st){var l=sortP(P.filter(function(p){return p.status===st}));return l.length?'<div class="list">'+l.map(function(p){return '<div class="it"><span class="pn">'+esc(p.name)+sampleTag(p)+'<small>'+esc(p.client)+' · '+esc(p.city)+'</small></span><span class="muted" style="white-space:nowrap">Due '+fmtDate(p.due)+'</span></div>'}).join('')+'</div>':'<p class="muted">Nothing here.</p>'};
  return '<div class="banner"><span><b>'+esc(ROLES[d])+' workspace</b> · Your projects, customer collections and team payroll.</span>'+monthSel()+'</div>'+
    '<div class="tiles">'+
    tile('My projects',m.total,'assigned to you','red')+
    tile('New this month',m.started,'started in '+L,'progress')+
    tile('In progress',m.progress,'active delivery','progress')+
    tile('Pending',m.pending,'awaiting commencement','pending')+
    tile('Completed',m.completed,m.doneMonth+' finished this month','completed')+
    tile('Contract value',lakh(m.valueAll),'your project portfolio','',true)+
    tile('Collections this month',lakh(m.revMonth),'received in '+monthShort(ui.month),'completed',true)+
    tile('Outstanding',lakh(m.outstanding),'recorded value less receipts','pending',true)+
    tile('Unpaid payroll',lakh(s.unpaidAmt),s.unpaid+' employees in '+monthShort(ui.month),'pending',true)+'</div>'+
    '<div class="grid2"><section class="panel"><h2>Collections trend <small>last 6 months</small></h2>'+revChart(P,ui.month)+'</section>'+
    '<section class="panel"><h2>Team & payroll <small>'+L+'</small></h2>'+(T.length?'<div class="stk"><span class="nm">'+plural(s.count,'employee')+'</span>'+twoStack(s.paid,s.unpaid,'paid','unpaid')+'</div>'+LEG_SAL+'<p style="margin-top:12px">'+s.paid+' of '+s.count+' salaries marked paid. '+money(s.unpaidAmt)+' remains unpaid.</p>':'<p class="muted">No employees added yet. Add your team under Salaries.</p>')+'</section></div>'+
    '<div class="grid2"><section class="panel"><h2>Projects in progress</h2>'+open('progress')+'</section><section class="panel"><h2>Pending projects</h2>'+open('pending')+'</section></div>'+
    '<section class="panel"><h2>Business responsibilities <small>ERP expansion roadmap</small></h2><div class="module-grid">'+
    [['Customer & sales pipeline','Leads, quotations, sales orders and follow-ups'],['Purchasing & imports','Supplier quotations, purchase orders and shipments'],['Inventory oversight','Stock availability, reservations and warehouse movements'],['Solution delivery','BOQ/BOM, project cost, installation and acceptance']].map(function(x){return '<div class="module-card"><b>'+x[0]+'</b><span>'+x[1]+'</span><em>Not connected in this version</em></div>'}).join('')+
    '</div></section>';
}
function viewDirProjects(){
  var d=ui.user.role,P=sortP(filtered(S.projects.filter(function(p){return p.dir===d}),false));
  return '<section class="panel"><h2>My projects <button class="btn" data-act="new-project">Add project</button></h2>'+filterBar(false)+projTable(P,'dir')+'</section>';
}
function viewDirSalaries(){
  var d=ui.user.role,T=S.staff.filter(function(x){return x.dir===d});
  return '<section class="panel"><h2>Employees and salaries <button class="btn" data-act="new-staff">Add employee</button></h2>'+salTable(T,'dir')+'</section>';
}
function viewManager(){
  var P=S.projects,m=metrics(P,ui.month);
  var on=S.staff.filter(function(x){return activeCount(x.id)>0}).length;
  var noTeam=sortP(P.filter(function(p){return p.status!=='completed'&&!(p.team&&p.team.length)}));
  var overdue=sortP(P.filter(function(p){return p.status!=='completed'&&p.due&&p.due<todayStr}));
  var busy=S.staff.map(function(x){return{x:x,n:activeCount(x.id)}}).sort(function(a,b){return b.n-a.n}).slice(0,5).filter(function(r){return r.n>0});
  var per=['dir1','dir2'].map(function(d){return{d:d,m:metrics(P.filter(function(p){return p.dir===d}),ui.month)}});
  return '<div class="banner"><span><b>Operations control</b> · Delivery status, team allocation and overdue work across both directors.</span>'+monthSel()+'</div>'+
    '<div class="tiles">'+
    tile('All projects',m.total,'both directors','red')+
    tile('In progress',m.progress,'currently active','progress')+
    tile('Pending',m.pending,'awaiting start','pending')+
    tile('Completed',m.completed,'completed to date','completed')+
    tile('Employees',S.staff.length,on+' assigned to active projects')+
    tile('Without a team',noTeam.length,'open projects','pending')+
    tile('Overdue projects',overdue.length,'past due date','pending')+
    tile('Started this month',m.started,'new project starts','progress')+'</div>'+
    '<div class="grid2"><section class="panel"><h2>Project status by director</h2>'+per.map(function(x){return '<div class="stk"><span class="nm">'+dirLabel(x.d)+'</span>'+statusStack(x.m)+'</div>'}).join('')+LEG_STATUS+'</section>'+
    '<section class="panel"><h2>Most-loaded employees</h2>'+(busy.length?'<div class="list">'+busy.map(function(r){return '<div class="it"><span class="pn">'+esc(r.x.name)+'<small>'+esc(r.x.role)+' · '+ROLES[r.x.dir]+'</small></span><span><b>'+r.n+'</b> active '+(r.n===1?'project':'projects')+'</span></div>'}).join('')+'</div>':'<p class="muted">No employees are assigned yet.</p>')+'</section></div>'+
    '<div class="grid2"><section class="panel"><h2>Projects without a team</h2>'+(noTeam.length?'<div class="list">'+noTeam.map(function(p){return '<div class="it"><span class="pn">'+esc(p.name)+sampleTag(p)+'<small>'+ROLES[p.dir]+' · '+esc(p.city)+'</small></span><button class="btn sm" data-act="assign" data-id="'+p.id+'">Assign team</button></div>'}).join('')+'</div>':'<p class="muted">Every open project has a team.</p>')+'</section>'+
    '<section class="panel"><h2>Overdue projects</h2>'+(overdue.length?'<div class="list">'+overdue.map(function(p){return '<div class="it"><span class="pn">'+esc(p.name)+'<small>'+ROLES[p.dir]+' · Due '+fmtDate(p.due)+'</small></span><span class="late">Overdue</span></div>'}).join('')+'</div>':'<p class="muted">No overdue projects.</p>')+'</section></div>'+
    '<section class="panel"><h2>Operational workstreams <small>ERP expansion roadmap</small></h2><div class="module-grid">'+
    [['Procurement & receiving','Track purchase requests, POs, GRNs and supplier delivery'],['Warehouse operations','Stock counts, transfers, serial numbers and reservations'],['Dispatch & delivery','Delivery planning, challans and proof of delivery'],['Service coordination','Warranty claims, repairs, RMA and maintenance']].map(function(x){return '<div class="module-card"><b>'+x[0]+'</b><span>'+x[1]+'</span><em>Not connected in this version</em></div>'}).join('')+
    '</div></section>';
}
function viewWorkload(){
  if(!S.staff.length)return '<section class="panel">'+empty('No employees yet','Directors add employees under Salaries. They then appear here.')+'</section>';
  var rows=S.staff.map(function(x){return{x:x,n:activeCount(x.id),all:S.projects.filter(function(p){return (p.team||[]).indexOf(x.id)>-1})}}).sort(function(a,b){return b.n-a.n||a.x.name.localeCompare(b.x.name)});
  var mx=Math.max.apply(null,rows.map(function(r){return r.n}).concat([1]));
  return '<section class="panel"><h2>Who works on what <small>'+plural(S.staff.length,'employee')+'</small></h2><div class="tw"><table><thead><tr><th>Employee</th><th>Reports to</th><th>Active projects</th><th>Projects</th></tr></thead><tbody>'+
   rows.map(function(r){
     var act=r.all.filter(function(p){return p.status!=='completed'});
     return '<tr><td class="pn">'+esc(r.x.name)+sampleTag(r.x)+'<small>'+esc(r.x.role)+'</small></td><td>'+ROLES[r.x.dir]+'</td><td><div class="load"><b>'+r.n+'</b><span class="tr"><i style="width:'+(r.n/mx*100)+'%"></i></span></div></td><td>'+(act.length?act.map(function(p){return '<span class="chip" title="'+STATUS[p.status]+'">'+esc(p.name)+'</span>'}).join(''):'<span class="muted">Not assigned</span>')+'</td></tr>';
   }).join('')+'</tbody></table></div></section>';
}
function viewAccounts(){
  return '<section class="panel"><h2>Accounts <small>'+plural(S.users.length,'account')+'</small></h2><div class="tw"><table><thead><tr><th>Name</th><th>Username</th><th>Role</th><th>Created</th><th></th></tr></thead><tbody>'+
   S.users.map(function(u){return '<tr><td class="pn">'+esc(u.name)+'</td><td>'+esc(u.username)+'</td><td>'+ROLES[u.role]+'</td><td>'+fmtDate(u.created)+'</td><td class="acts"><button class="btn link" data-act="setpw" data-id="'+u.id+'">Set new password</button>'+(u.id===ui.user.id?'':'<button class="btn link bad" data-act="ask-del" data-kind="user" data-id="'+u.id+'">Remove</button>')+'</td></tr>'}).join('')+
   '</tbody></table></div><p class="note" style="margin-top:12px">Removing a Director or the CEO account frees that role so the person can register again.</p></section>';
}

function viewFor(){
  var r=ui.user.role,v=ui.view;
  if(r==='ceo'){
    if(v==='projects')return '<section class="panel"><h2>All projects</h2>'+filterBar(true)+projTable(sortP(filtered(S.projects,true)),'ceo')+'</section>';
    if(v==='salaries'){var T=S.staff.filter(function(x){return ui.fDir==='all'||x.dir===ui.fDir});return '<section class="panel"><h2>Salaries <small>'+monthLabel(ui.month)+'</small></h2><div class="filters"><label class="mono-sel">Director <select class="sel" data-change="fdir"><option value="all">Both</option><option value="dir1"'+(ui.fDir==='dir1'?' selected':'')+'>Director 1</option><option value="dir2"'+(ui.fDir==='dir2'?' selected':'')+'>Director 2</option></select></label></div>'+salTable(T,'ceo')+'</section>'}
    if(v==='accounts')return viewAccounts();
    return viewCeo();
  }
  if(r==='manager'){
    if(v==='projects')return '<section class="panel"><h2>All projects</h2>'+filterBar(true)+projTable(sortP(filtered(S.projects,true)),'mgr')+'</section>';
    if(v==='workload')return viewWorkload();
    return viewManager();
  }
  if(v==='projects')return viewDirProjects();
  if(v==='salaries')return viewDirSalaries();
  return viewDirOverview();
}
function titleFor(){
  var r=ui.user.role,v=ui.view;
  var names={overview:r==='ceo'?'Company overview':r==='manager'?'Project overview':'Dashboard',projects:r==='ceo'||r==='manager'?'All projects':'My projects',salaries:'Salaries',accounts:'Accounts',workload:'Workload'};
  return names[v]||'';
}
function render(){
  if(!ui.ready)return;
  var y=window.scrollY;
  if(!ui.user){
    if(!ui.authDrawn)drawAuth();else refreshRoles();
    return;
  }
  var u=ui.user,nav=NAV[u.role];
  var showMonth=(ui.view==='overview'||ui.view==='salaries')&&u.role!=='manager'||(u.role==='manager'&&ui.view==='overview'&&false);
  $('#app').innerHTML=
  '<div class="shell">'+
   '<aside class="side"><div class="brand"><span class="sq">A</span><span>ASG ERP</span></div>'+
    '<div class="who"><div class="r">'+ROLES[u.role]+'</div><div class="n">'+esc(u.name)+'</div></div>'+
    '<nav class="nav" aria-label="Sections">'+nav.map(function(n){return '<button data-act="tab" data-tab="'+n[0]+'"'+(ui.view===n[0]?' aria-current="page"':'')+'>'+n[1]+'</button>'}).join('')+'</nav>'+
    '<button class="out" data-act="logout">Log out</button></aside>'+
   '<main class="main"><header class="top"><div><h1>'+titleFor()+'</h1><p class="sub">'+(ui.mode==='shared'?'Live shared data':'Local mode, saved in this browser')+(ui.note?' · '+esc(ui.note):'')+'</p></div>'+(showMonth?monthSel():'')+'</header>'+viewFor()+'</main>'+
  '</div>';
  window.scrollTo(0,y);
}

/* ---------- modals ---------- */
function fld(id,label,inner){return '<div class="fld"><label for="'+id+'">'+label+'</label>'+inner+'</div>'}
function opts(list,sel){return list.map(function(x){return '<option'+(x===sel?' selected':'')+'>'+esc(x)+'</option>'}).join('')}
function teamBoxes(staff,checked,grouped){
  if(!staff.length)return '<p class="muted">No employees to assign yet. Add employees under Salaries first.</p>';
  var html='<div class="team">',last='';
  staff.forEach(function(x){
    if(grouped&&x.dir!==last){last=x.dir;html+='<div class="gh">'+ROLES[x.dir]+'</div>'}
    html+='<label><input type="checkbox" name="team" value="'+x.id+'"'+(checked.indexOf(x.id)>-1?' checked':'')+'><span>'+esc(x.name)+'<small>'+esc(x.role)+'</small></span></label>';
  });
  return html+'</div>';
}
function modalHTML(){
  var m=ui.modal;if(!m)return '';
  var t='',body='';
  if(m.type==='project'){
    var d=ui.user.role,p=m.id?S.projects.find(function(x){return x.id===m.id}):null;
    if(m.id&&!p)return '';
    var own=S.staff.filter(function(x){return x.dir===d});
    t=p?'Edit project':'Add project';
    body='<form id="f-project">'+
     fld('pf-name','Project name','<input id="pf-name" required value="'+esc(p?p.name:'')+'">')+
     '<div class="row2">'+fld('pf-client','Client','<input id="pf-client" required value="'+esc(p?p.client:'')+'">')+fld('pf-value','Contract value (Rs)','<input id="pf-value" type="number" min="0" step="1000" required value="'+(p?p.value:'')+'">')+'</div>'+
     '<div class="row2">'+fld('pf-cat','Category','<select id="pf-cat">'+opts(CATS,p?p.category:CATS[0])+'</select>')+fld('pf-city','City','<select id="pf-city">'+opts(CITIES,p?p.city:'Lahore')+'</select>')+fld('pf-status','Status','<select id="pf-status">'+Object.keys(STATUS).map(function(k){return '<option value="'+k+'"'+((p?p.status:'pending')===k?' selected':'')+'>'+STATUS[k]+'</option>'}).join('')+'</select>')+'</div>'+
     '<div class="row2">'+fld('pf-start','Start date','<input id="pf-start" type="date" required value="'+(p?p.start:todayStr)+'">')+fld('pf-due','Due date','<input id="pf-due" type="date" value="'+(p?p.due||'':'')+'">')+'</div>'+
     '<div class="fld"><span class="lbl-s">Team (your employees)</span>'+teamBoxes(own,p?p.team||[]:[],false)+'</div>'+
     fld('pf-notes','Notes','<textarea id="pf-notes">'+esc(p?p.notes||'':'')+'</textarea>')+
     '<div class="foot"><button type="button" class="btn ghost" data-act="close">Cancel</button><button class="btn" type="submit">'+(p?'Save changes':'Add project')+'</button></div></form>';
  }else if(m.type==='assign'){
    var q=S.projects.find(function(x){return x.id===m.id});if(!q)return '';
    t='Team &amp; status';
    var all=S.staff.slice().sort(function(a,b){return a.dir.localeCompare(b.dir)||a.name.localeCompare(b.name)});
    body='<p><b>'+esc(q.name)+'</b><br><span class="muted">'+ROLES[q.dir]+' · '+esc(q.client)+'</span></p><form id="f-assign">'+
     fld('af-status','Status','<select id="af-status">'+Object.keys(STATUS).map(function(k){return '<option value="'+k+'"'+(q.status===k?' selected':'')+'>'+STATUS[k]+'</option>'}).join('')+'</select>')+
     '<div class="fld"><span class="lbl-s">Employees on this project</span>'+teamBoxes(all,q.team||[],true)+'</div>'+
     fld('af-notes','Notes','<textarea id="af-notes">'+esc(q.notes||'')+'</textarea>')+
     '<div class="foot"><button type="button" class="btn ghost" data-act="close">Cancel</button><button class="btn" type="submit">Save</button></div></form>';
  }else if(m.type==='pay'){
    var pr=S.projects.find(function(x){return x.id===m.id});if(!pr)return '';
    var rec=(pr.payments||[]).reduce(function(a,x){return a+(+x.amt||0)},0);
    t='Record payment';
    body='<p><b>'+esc(pr.name)+'</b><br><span class="muted">Received '+money(rec)+' of '+money(pr.value)+'</span></p><form id="f-pay">'+
     '<div class="row2">'+fld('yf-amt','Amount received (Rs)','<input id="yf-amt" type="number" min="1" step="1000" required>')+fld('yf-month','For month','<select id="yf-month">'+monthOptions()+'</select>')+'</div>'+
     '<div class="foot"><button type="button" class="btn ghost" data-act="close">Cancel</button><button class="btn" type="submit">Record payment</button></div></form>';
  }else if(m.type==='staff'){
    var e=m.id?S.staff.find(function(x){return x.id===m.id}):null;
    if(m.id&&!e)return '';
    t=e?'Edit employee':'Add employee';
    body='<form id="f-staff">'+fld('sf-name','Employee name','<input id="sf-name" required value="'+esc(e?e.name:'')+'">')+
     '<div class="row2">'+fld('sf-role','Role','<input id="sf-role" required placeholder="Site engineer" value="'+esc(e?e.role:'')+'">')+fld('sf-sal','Monthly salary (Rs)','<input id="sf-sal" type="number" min="0" step="1000" required value="'+(e?e.salary:'')+'">')+'</div>'+
     '<div class="foot"><button type="button" class="btn ghost" data-act="close">Cancel</button><button class="btn" type="submit">'+(e?'Save changes':'Add employee')+'</button></div></form>';
  }else if(m.type==='pw'){
    var us=S.users.find(function(x){return x.id===m.id});if(!us)return '';
    t='Set new password';
    body='<p>New password for <b>'+esc(us.name)+'</b> ('+esc(us.username)+').</p><form id="f-pw">'+fld('pw-new','New password','<input id="pw-new" type="password" minlength="6" required autocomplete="new-password">')+
     '<div class="foot"><button type="button" class="btn ghost" data-act="close">Cancel</button><button class="btn" type="submit">Save password</button></div></form>';
  }else if(m.type==='confirm'){
    var txt={project:'Delete this project and its payment records?',staff:'Remove this employee from payroll and from every project team?',user:'Remove this account? The person will need to register again.',sample:'Remove all sample projects and employees? Records you added yourself stay.'}[m.kind];
    t='Please confirm';
    body='<p>'+txt+'</p><div class="foot"><button class="btn ghost" data-act="close">Cancel</button><button class="btn" data-act="confirm-yes">Yes, continue</button></div>';
  }
  return '<div class="scrim"><div class="dlg" role="dialog" aria-modal="true" aria-labelledby="dlg-t"><h2 id="dlg-t">'+t+'</h2>'+body+'</div></div>';
}
function openModal(m){
  ui.modal=m;$('#modal').innerHTML=modalHTML();
  var f=$('#modal input:not([type=checkbox]),#modal select,#modal button');if(f)f.focus();
}
function closeModal(){ui.modal=null;$('#modal').innerHTML=''}

/* ---------- actions ---------- */
function canTouch(p){return ui.user&&(ui.user.role==='manager'||ui.user.role===p.dir)}
function fixDone(p){if(p.status==='completed'){if(!p.done)p.done=todayStr}else p.done=''}
function saveProject(f){
  var v=function(id){return f.querySelector('#'+id).value.trim()};
  var rec={name:v('pf-name'),client:v('pf-client'),category:v('pf-cat'),city:v('pf-city'),status:v('pf-status'),value:Math.max(0,+v('pf-value')||0),start:v('pf-start'),due:v('pf-due'),notes:v('pf-notes')};
  var picked=[].map.call(f.querySelectorAll('input[name=team]:checked'),function(x){return x.value});
  var id=ui.modal.id,d=ui.user.role;
  closeModal();
  commit(function(){
    var ex=id&&S.projects.find(function(p){return p.id===id});
    if(ex){
      var own={};S.staff.forEach(function(s){if(s.dir===ex.dir)own[s.id]=1});
      var keep=(ex.team||[]).filter(function(i){return !own[i]});
      Object.assign(ex,rec,{team:keep.concat(picked)});fixDone(ex);
    }else{
      var np=Object.assign({id:uid('p_'),dir:d,payments:[],team:picked,done:''},rec);fixDone(np);S.projects.push(np);
    }
  });
  toast(id?'Project updated':'Project added');
}
function saveAssign(f){
  var id=ui.modal.id,st=f.querySelector('#af-status').value,notes=f.querySelector('#af-notes').value.trim();
  var picked=[].map.call(f.querySelectorAll('input[name=team]:checked'),function(x){return x.value});
  closeModal();
  commit(function(){var p=S.projects.find(function(x){return x.id===id});if(!p||!canTouch(p))return;p.status=st;fixDone(p);p.team=picked;p.notes=notes});
  toast('Project updated');
}
function savePay(f){
  var id=ui.modal.id,amt=+f.querySelector('#yf-amt').value,mo=f.querySelector('#yf-month').value;
  if(!(amt>0))return;
  closeModal();
  commit(function(){var p=S.projects.find(function(x){return x.id===id});if(!p||ui.user.role!==p.dir)return;p.payments=p.payments||[];p.payments.push({m:mo,amt:amt,on:todayStr})});
  toast('Payment recorded');
}
function saveStaff(f){
  var id=ui.modal.id,name=f.querySelector('#sf-name').value.trim(),role=f.querySelector('#sf-role').value.trim(),sal=Math.max(0,+f.querySelector('#sf-sal').value||0),d=ui.user.role;
  closeModal();
  commit(function(){
    var ex=id&&S.staff.find(function(x){return x.id===id});
    if(ex){if(ex.dir===d){ex.name=name;ex.role=role;ex.salary=sal}}
    else S.staff.push({id:uid('s_'),name:name,role:role,dir:d,salary:sal,paid:{}});
  });
  toast(id?'Employee updated':'Employee added');
}
function savePw(f){
  var id=ui.modal.id,pw=f.querySelector('#pw-new').value;
  var u=S.users.find(function(x){return x.id===id});if(!u||pw.length<6)return;
  hashPw(u.username,pw).then(function(h){closeModal();commit(function(){var x=S.users.find(function(y){return y.id===id});if(x)x.pass=h});toast('Password updated')});
}
function doConfirm(){
  var m=ui.modal;closeModal();
  commit(function(){
    if(m.kind==='project'){var p=S.projects.find(function(x){return x.id===m.id});if(p&&ui.user.role===p.dir)S.projects=S.projects.filter(function(x){return x.id!==m.id})}
    else if(m.kind==='staff'){var s=S.staff.find(function(x){return x.id===m.id});if(s&&ui.user.role===s.dir){S.staff=S.staff.filter(function(x){return x.id!==m.id});S.projects.forEach(function(p){p.team=(p.team||[]).filter(function(i){return i!==m.id})})}}
    else if(m.kind==='user'){if(ui.user.role==='ceo'&&m.id!==ui.user.id)S.users=S.users.filter(function(x){return x.id!==m.id})}
    else if(m.kind==='sample'){if(ui.user.role==='ceo'){var gone={};S.staff.forEach(function(s){if(s.sample)gone[s.id]=1});S.staff=S.staff.filter(function(s){return !s.sample});S.projects=S.projects.filter(function(p){return !p.sample});S.projects.forEach(function(p){p.team=(p.team||[]).filter(function(i){return !gone[i]})})}}
  });
  toast('Done');
}

document.addEventListener('click',function(e){
  if(e.target.classList&&e.target.classList.contains('scrim')){closeModal();return}
  var b=e.target.closest('[data-act]');if(!b)return;
  var a=b.dataset.act,id=b.dataset.id;
  if(a==='authtab'){ui.authTab=b.dataset.tab;ui.authDrawn=false;render()}
  else if(a==='tab'){ui.view=b.dataset.tab;ui.fStatus='all';ui.fDir='all';render();scrollTo(0,0)}
  else if(a==='logout')logout();
  else if(a==='new-project'&&/^dir/.test(ui.user.role))openModal({type:'project'});
  else if(a==='edit-project')openModal({type:'project',id:id});
  else if(a==='assign')openModal({type:'assign',id:id});
  else if(a==='pay')openModal({type:'pay',id:id});
  else if(a==='new-staff')openModal({type:'staff'});
  else if(a==='edit-staff')openModal({type:'staff',id:id});
  else if(a==='setpw')openModal({type:'pw',id:id});
  else if(a==='ask-del')openModal({type:'confirm',kind:b.dataset.kind,id:id});
  else if(a==='confirm-yes')doConfirm();
  else if(a==='close')closeModal();
  else if(a==='toggle-paid'){
    commit(function(){var s=S.staff.find(function(x){return x.id===id});if(!s||s.dir!==ui.user.role)return;s.paid=s.paid||{};if(s.paid[ui.month])delete s.paid[ui.month];else s.paid[ui.month]=todayStr});
  }
});
document.addEventListener('change',function(e){
  var t=e.target,k=t.dataset&&t.dataset.change;if(!k)return;
  if(k==='month'){ui.month=t.value;render()}
  else if(k==='fstatus'){ui.fStatus=t.value;render()}
  else if(k==='fdir'){ui.fDir=t.value;render()}
  else if(k==='status'){
    var p=S.projects.find(function(x){return x.id===t.dataset.id});
    if(!p||!canTouch(p)||!STATUS[t.value]){render();return}
    commit(function(){p.status=t.value;fixDone(p)});
  }
});
document.addEventListener('submit',function(e){
  e.preventDefault();var f=e.target;
  switch(f.id){
    case 'f-login':doLogin();break;
    case 'f-signup':doSignup();break;
    case 'f-project':saveProject(f);break;
    case 'f-assign':saveAssign(f);break;
    case 'f-pay':savePay(f);break;
    case 'f-staff':saveStaff(f);break;
    case 'f-pw':savePw(f);break;
  }
});
document.addEventListener('keydown',function(e){if(e.key==='Escape'&&ui.modal)closeModal()});

  /* ---------- ASG ERP expansion (authentication UI above intentionally unchanged) ---------- */
  var legacyRender=render;
  var ERP_GROUPS=[
    ['Dashboard',[['erp-dashboard','Main Dashboard'],['erp-business','Business Overview'],['erp-sales-overview','Sales Overview'],['erp-purchase-overview','Purchase Overview'],['erp-inventory-overview','Inventory Overview'],['erp-finance-overview','Financial Overview'],['erp-activity','Recent Activities'],['erp-notifications','Notifications'],['erp-actions','Quick Actions']]],
    ['Inventory Management',[['products','All Products'],['categories','Product Categories'],['brands','Product Brands'],['units','Product Units'],['warehouses','Warehouse Management'],['stock-adjustments','Stock Adjustment'],['low-stock','Low Stock Alerts'],['out-of-stock','Out of Stock'],['stock-history','Stock History'],['barcode','Barcode Management'],['product-gallery','Product Pictures & Gallery']]],
    ['GET IN – Stock Receiving',[['stockIn','New Stock Entry'],['stockIn-history','Received Stock History']]],
    ['GET OUT – Stock Issuing',[['stockOut','New Stock Issue'],['stockOut-history','Delivery Records']]],
    ['Sales Management',[['sales','All Sales'],['quotations','Quotations'],['invoices','Invoices'],['customers','All Customers'],['customer-payments','Customer Payments']]],
    ['Purchase Management',[['purchases','All Purchases'],['purchase-orders','Purchase Orders'],['suppliers','All Suppliers'],['supplier-payments','Supplier Payments']]],
    ['Finance & Accounting',[['income','Income'],['expenses','Expenses'],['cashbook','Cash Book'],['accounts-receivable','Accounts Receivable'],['accounts-payable','Accounts Payable'],['profit-loss','Profit & Loss']]],
    ['Pictures & Documents',[['documents','Document Gallery']]],
    ['Reports & Analytics',[['reports','Reports Dashboard'],['inventory-reports','Inventory Reports'],['sales-reports','Sales Reports'],['purchase-reports','Purchase Reports']]],
    ['Employees & HR',[['employees','All Employees'],['departments','Departments'],['attendance','Attendance'],['salary','Salary Management']]],
    ['Settings',[['company','Company Profile'],['tax','Tax Settings'],['invoice-settings','Invoice Settings'],['backup','Backup & Restore']]]
  ];
  var ERP_META={
    products:{label:'Product',fields:[['name','Product name','text'],['sku','SKU / Barcode','text'],['category','Category','text'],['brand','Brand','text'],['unit','Unit','text'],['cost','Cost price','number'],['price','Selling price','number'],['minStock','Minimum stock','number'],['warehouse','Warehouse','text'],['picture','Picture URL (optional)','text']]},
    categories:{label:'Category',fields:[['name','Category name','text'],['description','Description','text']]},
    brands:{label:'Brand',fields:[['name','Brand name','text']]},units:{label:'Unit',fields:[['name','Unit name','text'],['symbol','Symbol','text']]},
    warehouses:{label:'Warehouse',fields:[['name','Warehouse name','text'],['location','Location','text']]},
    customers:{label:'Customer',fields:[['name','Customer name','text'],['phone','Phone','text'],['email','Email','email'],['address','Address','text'],['openingBalance','Opening balance (Rs)','number']]},
    suppliers:{label:'Supplier',fields:[['name','Supplier name','text'],['phone','Phone','text'],['email','Email','email'],['address','Address','text'],['openingBalance','Opening balance (Rs)','number']]},
    stockIn:{label:'Stock receiving',fields:[['productId','Product','product'],['qty','Quantity','number'],['unitCost','Unit cost (Rs)','number'],['supplier','Supplier','text'],['reference','Invoice / reference','text'],['notes','Notes','text']]},
    stockOut:{label:'Stock issue',fields:[['productId','Product','product'],['qty','Quantity','number'],['issueType','Issue type (Customer / Location transfer)','text'],['issuedTo','Customer / issued to','text'],['destination','Destination location / warehouse','text'],['reference','Dispatch reference','text'],['notes','Notes (optional)','text']]},
    sales:{label:'Sale',fields:[['productId','Product','product'],['qty','Quantity','number'],['unitPrice','Unit price (Rs)','number'],['customer','Customer','text'],['reference','Invoice number','text']]},
    purchases:{label:'Purchase',fields:[['productId','Product','product'],['qty','Quantity','number'],['unitCost','Unit cost (Rs)','number'],['supplier','Supplier','text'],['reference','PO / invoice number','text']]},
    expenses:{label:'Expense',fields:[['title','Expense title','text'],['amount','Amount (Rs)','number'],['category','Category','text'],['date','Date','date'],['notes','Notes','text']]},
    income:{label:'Income',fields:[['title','Income title','text'],['amount','Amount (Rs)','number'],['source','Source','text'],['date','Date','date']]},
    employees:{label:'Employee',fields:[['name','Employee name','text'],['department','Department','text'],['phone','Phone','text'],['salary','Monthly salary (Rs)','number'],['joiningDate','Joining date','date']]}
  };
  var erpView='erp-dashboard',erpSearch='';
  function erpData(k){S.erp=S.erp||{};S.erp[k]=S.erp[k]||[];return S.erp[k]}
  function erpLog(action,detail){erpData('activities').unshift({id:uid('a_'),action:action,detail:detail,date:new Date().toISOString()});S.erp.activities=S.erp.activities.slice(0,100)}
  function stockOf(id){return erpData('stockIn').filter(x=>x.productId===id).reduce((a,x)=>a+(+x.qty||0),0)+erpData('purchases').filter(x=>x.productId===id).reduce((a,x)=>a+(+x.qty||0),0)-erpData('stockOut').filter(x=>x.productId===id).reduce((a,x)=>a+(+x.qty||0),0)-erpData('sales').filter(x=>x.productId===id).reduce((a,x)=>a+(+x.qty||0),0)}
  function erpLabel(k){for(var g of ERP_GROUPS)for(var x of g[1])if(x[0]===k)return x[1];return k}
  function erpRows(k){var map={'stockIn-history':'stockIn','stockOut-history':'stockOut','low-stock':'products','out-of-stock':'products','stock-history':'stockIn','sales-overview':'sales','purchase-overview':'purchases','inventory-overview':'products','business':'products','finance-overview':'expenses','customer-payments':'payments','supplier-payments':'payments','quotations':'sales','invoices':'sales','purchase-orders':'purchases','cashbook':'income','accounts-receivable':'sales','accounts-payable':'purchases','attendance':'employees','salary':'employees','departments':'employees','barcode':'products','product-gallery':'products','inventory-reports':'products','sales-reports':'sales','purchase-reports':'purchases'};return erpData(map[k]||k)}
  function erpMoney(n){return 'Rs '+(Number(n)||0).toLocaleString('en-PK',{maximumFractionDigits:2})}
  function erpMetric(label,value,sub){return '<div class="tile"><div class="lbl">'+label+'</div><div class="val sm">'+value+'</div><div class="sub">'+(sub||'')+'</div></div>'}
  function erpDashboard(){
    var ps=erpData('products'),sales=erpData('sales'),purch=erpData('purchases'),ex=erpData('expenses'),inc=erpData('income');
    var qty=ps.reduce((a,p)=>a+Math.max(0,stockOf(p.id)),0),value=ps.reduce((a,p)=>a+Math.max(0,stockOf(p.id))*(+p.cost||0),0);
    var today=new Date().toISOString().slice(0,10),sumToday=k=>erpData(k).filter(x=>(x.date||x.created||'').slice(0,10)===today).reduce((a,x)=>a+(+x.qty||0)*(+(x.unitPrice||x.unitCost||x.amount)||0),0);
    var low=ps.filter(p=>stockOf(p.id)<=Number(p.minStock||0)&&stockOf(p.id)>0).length,zero=ps.filter(p=>stockOf(p.id)<=0).length;
    var saleTotal=sales.reduce((a,x)=>a+(+x.qty||0)*(+x.unitPrice||0),0),purchaseTotal=purch.reduce((a,x)=>a+(+x.qty||0)*(+x.unitCost||0),0),incomeTotal=saleTotal,expenseTotal=ex.reduce((a,x)=>a+(+x.amount||0),0);
    return '<div class="banner"><b>ASG SYSTEMS · Enterprise Resource Planning</b><span>Live totals from saved transactions</span></div><div class="tiles">'+
      erpMetric('Total products',ps.length,'Product records')+erpMetric('Total stock quantity',qty,'Available units')+erpMetric('Stock value',erpMoney(value),'At recorded cost')+erpMetric("Today's sales",erpMoney(sumToday('sales')),'Sales transactions')+
      erpMetric("Today's purchases",erpMoney(sumToday('purchases')),'Purchase transactions')+erpMetric('Total sales',erpMoney(saleTotal),'Recorded sales')+erpMetric('Total purchases',erpMoney(purchaseTotal),'Recorded purchases')+erpMetric('Net income',erpMoney(incomeTotal-expenseTotal),'Income less expenses')+
      erpMetric('Low stock',low,'Products at/below minimum')+erpMetric('Out of stock',zero,'Zero or negative stock')+erpMetric('Customers',erpData('customers').length,'Customer records')+erpMetric('Suppliers',erpData('suppliers').length,'Supplier records')+'</div>'+
      '<div class="grid2"><section class="panel"><h2>Recent stock receiving</h2>'+erpTable('stockIn',erpData('stockIn').slice(-5).reverse())+'</section><section class="panel"><h2>Recent stock issuing</h2>'+erpTable('stockOut',erpData('stockOut').slice(-5).reverse())+'</section></div>'+
      '<section class="panel"><h2>Recent activity</h2>'+erpActivity()+'</section>';
  }
  function erpActivity(){var a=erpData('activities').slice(0,12);return a.length?'<div class="list">'+a.map(x=>'<div class="it"><span>'+esc(x.action)+' · '+esc(x.detail)+'</span><small class="muted">'+new Date(x.date).toLocaleString()+'</small></div>').join('')+'</div>':empty('No activity yet','Your ERP transactions will appear here.')}
  function erpTable(k,rows){
    if(!rows||!rows.length)return empty('No records yet','Use Add New to create your first record.');
    var keys=Object.keys(rows[0]).filter(x=>!['id','picture'].includes(x)).slice(0,6);
    return '<div class="tw"><table><thead><tr>'+keys.map(x=>'<th>'+esc(x)+'</th>').join('')+'<th>Available stock</th><th>Actions</th></tr></thead><tbody>'+rows.map(r=>'<tr>'+keys.map(x=>'<td>'+esc(r[x])+'</td>').join('')+'<td>'+(k==='products'?stockOf(r.id):'—')+'</td><td class="acts"><button class="btn link" data-erp-edit="'+k+'" data-id="'+r.id+'">Edit</button><button class="btn link bad" data-erp-delete="'+k+'" data-id="'+r.id+'">Delete</button></td></tr>').join('')+'</tbody></table></div>';
  }
  function erpPage(k){
    if(k==='erp-dashboard'||k==='erp-business')return erpDashboard();
    if(k==='erp-activity'||k==='erp-notifications')return '<section class="panel"><h2>'+erpLabel(k)+'</h2>'+erpActivity()+'</section>';
    if(k==='income'){var auto=erpData('sales').map(x=>({id:x.id,title:'Sale '+(x.reference||''),amount:(+x.qty||0)*(+x.unitPrice||0),source:'Sales',date:x.date||x.created||''}));return '<section class="panel"><h2>Income <small>Automatically generated from sales transactions</small></h2>'+erpTable('income',auto)+'</section>'}
    if(k==='cashbook'){var cash=[];erpData('sales').forEach(x=>cash.push({id:x.id,date:x.date||x.created||'',type:'Cash In',description:'Sale '+(x.reference||''),amount:(+x.qty||0)*(+x.unitPrice||0)}));erpData('purchases').forEach(x=>cash.push({id:x.id,date:x.date||x.created||'',type:'Cash Out',description:'Purchase '+(x.reference||''),amount:(+x.qty||0)*(+x.unitCost||0)}));erpData('expenses').forEach(x=>cash.push({id:x.id,date:x.date||'',type:'Cash Out',description:x.title||'Expense',amount:+x.amount||0}));cash.sort((a,b)=>String(b.date).localeCompare(String(a.date)));return '<section class="panel"><h2>Cash Book <small>Cash in and out from sales, purchases and expenses</small></h2>'+erpTable('cashbook',cash)+'</section>'}
    if(k==='erp-actions')return '<section class="panel"><h2>Quick Actions</h2><div class="module-grid">'+[['products','Add product'],['stockIn','Receive stock'],['stockOut','Issue stock'],['sales','New sale'],['purchases','New purchase'],['expenses','Add expense']].map(x=>'<button class="btn ghost" data-erp-new="'+x[0]+'">'+x[1]+'</button>').join('')+'</div></section>';
    if(k==='low-stock'||k==='out-of-stock')return '<section class="panel"><h2>'+erpLabel(k)+'</h2>'+erpTable('products',erpData('products').filter(p=>k==='low-stock'?stockOf(p.id)<=Number(p.minStock||0)&&stockOf(p.id)>0:stockOf(p.id)<=0))+'</section>';
    if(k==='stock-history'){var products=erpData('products');return '<section class="panel"><h2>Stock history · By product</h2>'+products.map(p=>{var rows=erpData('stockIn').filter(x=>x.productId===p.id).map(x=>Object.assign({movement:'Received'},x)).concat(erpData('purchases').filter(x=>x.productId===p.id).map(x=>Object.assign({movement:'Purchase received'},x)),erpData('stockOut').filter(x=>x.productId===p.id).map(x=>Object.assign({movement:x.issueType==='Location transfer'?'Location transfer':'Issued'},x)),erpData('sales').filter(x=>x.productId===p.id).map(x=>Object.assign({movement:'Sold'},x)));return '<section class="panel"><h2>'+esc(p.name)+' <small>Available: '+stockOf(p.id)+'</small></h2>'+erpTable('history',rows)+'</section>'}).join('')+'</section>'}
    if(k==='erp-sales-overview'||k==='erp-purchase-overview'||k==='erp-inventory-overview'||k==='erp-finance-overview')return erpDashboard();
    if(k==='profit-loss'){var rev=erpData('sales').reduce((a,x)=>a+(+x.qty||0)*(+x.unitPrice||0),0),cogs=erpData('sales').reduce((a,x)=>{var p=erpData('products').find(z=>z.id===x.productId);return a+(+x.qty||0)*(+(x.unitCostAtSale!=null?x.unitCostAtSale:(p&&p.cost))||0)},0),gross=rev-cogs,ex=erpData('expenses').reduce((a,x)=>a+(+x.amount||0),0),net=gross-ex;return '<section class="panel report"><h2>Profit & Loss</h2><p>Total Revenues − Total Cost of Sales = Gross Profit</p><p>Total Revenues − Total Cost of Sales − Total Expenses = Net Profit</p><div class="tiles">'+erpMetric('Total Revenues',erpMoney(rev))+erpMetric('Cost of Sales',erpMoney(cogs))+erpMetric('Gross Profit',erpMoney(gross))+erpMetric('Total Expenses',erpMoney(ex))+erpMetric('Net Profit',erpMoney(net))+'</div></section>'}
    if(k==='invoices'){return '<section class="panel"><h2>Sales Invoices <button class="btn" type="button" onclick="window.print()">Print invoices</button></h2>'+erpTable('sales',erpData('sales'))+'</section>'}
    if(k==='reports'||k.endsWith('-reports'))return '<section class="panel"><h2>'+erpLabel(k)+'</h2><p class="note">Export the currently displayed dataset as CSV for Excel.</p><button class="btn" data-erp-export="'+k+'">Export CSV</button>'+erpTable(k,erpRows(k))+'</section>';
    var entity={'stockIn-history':'stockIn','stockOut-history':'stockOut','product-gallery':'products','barcode':'products','quotations':'sales','invoices':'sales','purchase-orders':'purchases','customer-payments':'payments','supplier-payments':'payments','cashbook':'income','accounts-receivable':'sales','accounts-payable':'purchases','attendance':'employees','salary':'employees','departments':'employees'}[k]||k;
    var canAdd=!!ERP_META[entity] && k!=='income' && k!=='cashbook';
    return '<section class="panel"><h2>'+erpLabel(k)+(canAdd?'<button class="btn" data-erp-new="'+entity+'">Add New</button>':'')+'</h2><div class="filters"><input class="sel" style="min-width:220px" placeholder="Search records..." data-erp-search value="'+esc(erpSearch)+'"></div>'+erpTable(entity,erpData(entity))+'</section>';
  }
  function erpModalHTML(kind,id){
    var meta=ERP_META[kind];if(!meta)return '';
    var old=id?erpData(kind).find(x=>x.id===id):null;
    if(!id&&(kind==='purchases'||kind==='stockOut')){
      var isPurchase=kind==='purchases',rows='';
      for(var bi=0;bi<5;bi++) rows+='<div class="row2" style="padding:10px 0;border-bottom:1px solid var(--line)"><div class="fld"><label>Product '+(bi+1)+'</label><select name="productId'+bi+'"><option value="">— Select product —</option>'+erpData('products').map(p=>'<option value="'+p.id+'">'+esc(p.name)+' · Stock '+stockOf(p.id)+'</option>').join('')+'</select></div><div class="fld"><label>Quantity</label><input name="qty'+bi+'" type="number" min="0" step="any"></div>'+(isPurchase?'<div class="fld"><label>Unit cost (Rs)</label><input name="unitCost'+bi+'" type="number" min="0" step="any"></div>':'')+'</div>';
      return '<div class="scrim"><div class="dlg" role="dialog" aria-modal="true"><h2>Add '+(isPurchase?'Purchase Batch':'Stock Issue Batch')+'</h2><form id="f-erp" data-kind="'+kind+'" data-batch="true"><p class="note">Add up to five products in one batch. Leave unused rows blank.</p>'+rows+(isPurchase?'<div class="row2"><div class="fld"><label>Supplier (optional)</label><input name="supplier"></div><div class="fld"><label>PO / Invoice (optional)</label><input name="reference"></div></div>':'<div class="row2"><div class="fld"><label>Issue type</label><select name="issueType"><option>Customer</option><option>Location transfer</option></select></div><div class="fld"><label>Customer / issued to (optional)</label><input name="issuedTo"></div><div class="fld"><label>Destination location (optional)</label><input name="destination"></div></div><div class="fld"><label>Dispatch reference (optional)</label><input name="reference"></div>')+'<div class="foot"><button type="button" class="btn ghost" data-act="close">Cancel</button><button class="btn" type="submit">Save batch</button></div></form></div></div>';
    }
    var html='<form id="f-erp" data-kind="'+kind+'" data-id="'+(id||'')+'"><div class="row2">';
    meta.fields.forEach(f=>{
      var val=old?old[f[0]]||'':(f[0]==='date'?todayStr:'');
      var must=['name','productId','qty','amount','title'].includes(f[0]) && !(f[0]==='title'&&kind==='income'); var input=f[2]==='product'?'<select name="'+f[0]+'" '+(must?'required':'')+'>'+erpData('products').map(p=>'<option value="'+p.id+'" '+(val===p.id?'selected':'')+'>'+esc(p.name)+' · Stock '+stockOf(p.id)+'</option>').join('')+'</select>':'<input name="'+f[0]+'" type="'+f[2]+'" '+(f[2]==='number'?'min="0" step="any"':'')+' value="'+esc(val)+'" '+(must?'required':'')+'>';
      html+=fld('erp-'+f[0],f[1],input);
    });
    html+='</div><div class="foot"><button type="button" class="btn ghost" data-act="close">Cancel</button><button class="btn" type="submit">Save record</button></div></form>';
    return '<div class="scrim"><div class="dlg" role="dialog" aria-modal="true"><h2>'+(old?'Edit ':'Add ')+meta.label+'</h2>'+html+'</div></div>';
  }
  function openErpModal(k,id){ui.modal={type:'erp',kind:k,id:id||''};$('#modal').innerHTML=erpModalHTML(k,id)}
  function saveErpForm(f){
    var k=f.dataset.kind,id=f.dataset.id,meta=ERP_META[k],obj={id:id||uid('e_'),created:todayStr};
    if(f.dataset.batch==='true'){
      var common={date:todayStr,created:todayStr};
      if(k==='purchases'){common.supplier=f.elements.supplier.value.trim();common.reference=f.elements.reference.value.trim()}
      else{common.issueType=f.elements.issueType.value;common.issuedTo=f.elements.issuedTo.value.trim();common.destination=f.elements.destination.value.trim();common.reference=f.elements.reference.value.trim()}
      var added=0;
      for(var j=0;j<5;j++){var pid=f.elements['productId'+j].value,q=Number(f.elements['qty'+j].value||0);if(!pid&&!q)continue;if(!pid||!(q>0)){toast('Each used batch row needs a product and quantity');return}if(k==='stockOut'&&q>stockOf(pid)){var pp=erpData('products').find(x=>x.id===pid);toast('Insufficient stock for '+(pp?pp.name:'product'));return}var rec=Object.assign({id:uid('e_'),productId:pid,qty:q},common);if(k==='purchases')rec.unitCost=Number(f.elements['unitCost'+j].value||0);erpData(k).push(rec);added++}
      if(!added){toast('Enter at least one product and quantity');return}
      erpLog(k+' batch saved',added+' product lines');closeModal();persist();render();toast('Batch saved: '+added+' lines');return;
    }
    meta.fields.forEach(x=>{var el=f.elements[x[0]];obj[x[0]]=x[2]==='number'?Number(el.value||0):el.value.trim()});
    if((k==='stockIn'||k==='stockOut'||k==='sales'||k==='purchases')&&!(obj.qty>0)){toast('Quantity must be greater than zero');return}
    if(['stockOut','sales'].includes(k)){var available=stockOf(obj.productId);if(obj.qty>available){toast('Insufficient stock. Available: '+available);return}}
    var arr=erpData(k);if(id){var ix=arr.findIndex(x=>x.id===id);if(ix>=0)arr[ix]=Object.assign(arr[ix],obj)}else arr.push(obj);
    if(k==='stockIn'||k==='stockOut'||k==='sales'||k==='purchases')obj.date=todayStr;
    if(k==='stockOut'&&!obj.issueType)obj.issueType='Customer';
    if(k==='sales'){var soldProduct=erpData('products').find(x=>x.id===obj.productId);obj.unitCostAtSale=+(soldProduct&&soldProduct.cost)||0;var incomeRows=erpData('income');incomeRows=incomeRows.filter(x=>x.sourceId!==obj.id);incomeRows.push({id:uid('inc_'),sourceId:obj.id,title:'Sale '+(obj.reference||''),amount:obj.qty*obj.unitPrice,source:'Sales',date:todayStr});S.erp.income=incomeRows;}
    erpLog(k+' saved',meta.label+' '+(id?'updated':'created'));closeModal();persist();render();toast(meta.label+' saved');
  }
  render=function(){
    if(!ui.ready||!ui.user){legacyRender();return}
    var groups=ERP_GROUPS.map(g=>'<div class="nav-group"><div class="nav-heading">'+g[0]+'</div>'+g[1].map(n=>'<button data-erp-tab="'+n[0]+'" '+(erpView===n[0]?'aria-current="page"':'')+'>'+n[1]+'</button>').join('')+'</div>').join('');
    var oldNav=NAV[ui.user.role].map(n=>'<button data-act="tab" data-tab="'+n[0]+'">'+n[1]+'</button>').join('');
    $('#app').innerHTML='<div class="shell"><aside class="side"><div class="brand"><span class="sq">A</span><span>ASG SYSTEMS</span></div><div class="who"><div class="r">'+ROLES[ui.user.role]+'</div><div class="n">'+esc(ui.user.name)+'</div></div><nav class="nav" aria-label="ERP modules">'+groups+'<div class="nav-heading">Existing ASG features</div>'+oldNav+'</nav><button class="out" data-act="logout">Log out</button></aside><main class="main"><header class="top"><div><h1>'+esc(erpLabel(erpView))+'</h1><p class="sub">ASG ERP · '+(ui.mode==='shared'?'Live shared data':'Browser-local storage')+'</p></div></header>'+ (['erp-dashboard','erp-business','erp-sales-overview','erp-purchase-overview','erp-inventory-overview','erp-finance-overview','erp-activity','erp-notifications','erp-actions','erp-dashboard'].includes(erpView)?erpPage(erpView):erpPage(erpView))+'</main></div>';
  };
  document.addEventListener('click',function(e){
    var b=e.target.closest('[data-erp-tab],[data-erp-new],[data-erp-edit],[data-erp-delete],[data-erp-export]');if(!b)return;
    if(b.dataset.erpTab){erpView=b.dataset.erpTab;erpSearch='';render()}
    else if(b.dataset.erpNew)openErpModal(b.dataset.erpNew);
    else if(b.dataset.erpEdit)openErpModal(b.dataset.erpEdit,b.dataset.id);
    else if(b.dataset.erpDelete){var k=b.dataset.erpDelete,id=b.dataset.id;if(confirm('Delete this record?')){S.erp[k]=erpData(k).filter(x=>x.id!==id);erpLog('Record deleted',k);persist();render();toast('Record deleted')}}
    else if(b.dataset.erpExport){var k=b.dataset.erpExport,rows=erpRows(k),keys=rows.length?Object.keys(rows[0]):[];var csv=[keys.join(',' )].concat(rows.map(r=>keys.map(x=>'"'+String(r[x]??'').replace(/"/g,'""')+'"').join(','))).join('\n');var a=document.createElement('a');a.href=URL.createObjectURL(new Blob([csv],{type:'text/csv'}));a.download=k+'.csv';a.click();URL.revokeObjectURL(a.href)}
  });
  document.addEventListener('submit',function(e){if(e.target.id==='f-erp'){e.preventDefault();saveErpForm(e.target)}});
  document.addEventListener('input',function(e){if(e.target.matches('[data-erp-search]')){erpSearch=e.target.value;var main=$('#app .main');if(main){var rows=erpData(erpView).filter(x=>JSON.stringify(x).toLowerCase().includes(erpSearch.toLowerCase()));var entity=erpView;var table=main.querySelector('.tw');if(table)table.outerHTML=erpTable(entity,rows)}}});
  var erpStyle=document.createElement('style');erpStyle.textContent='.side{max-height:calc(100vh - 32px);overflow:auto}.nav-group{display:flex;flex-direction:column;gap:2px;margin:5px 0}.nav-heading{font-size:10px;text-transform:uppercase;letter-spacing:.12em;color:#e9a0ad;padding:9px 8px 4px;font-weight:700}.nav button{font-size:12px;padding:7px 9px}.nav button[aria-current=page]{background:var(--accent);color:white}.shell{grid-template-columns:250px minmax(0,1fr)}.side{border-radius:10px}.main{min-width:0}@media(max-width:900px){.side{max-height:none}.nav{display:block;max-height:360px;overflow:auto}.shell{grid-template-columns:1fr}}';document.head.appendChild(erpStyle);

  boot();
})();
</script>

</body>
</html>
