<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FMA-UE Motor Function (A–D) Scoring Tool</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; background: #f1f5f9; color: #1e293b; font-size: 14px; }

    .app-header { background: linear-gradient(135deg,#1e3a8a,#1d4ed8); color:#fff; padding:14px 24px; }
    .app-header h1 { font-size:1.15rem; font-weight:700; }
    .app-header p  { font-size:.7rem; opacity:.6; margin-top:3px; }

    .score-bar { background:#1e3a8a; color:#fff; display:flex; flex-wrap:wrap; gap:5px; padding:7px 16px; justify-content:center; align-items:center; position:sticky; top:0; z-index:900; box-shadow:0 2px 12px rgba(0,0,0,.38); }
    .sc  { background:rgba(255,255,255,.1); border-radius:8px; padding:4px 10px; text-align:center; min-width:56px; }
    .sc .sl { font-size:.58rem; opacity:.7; display:block; }
    .sc .sv { font-size:.95rem; font-weight:700; display:block; }
    .sc .sm { font-size:.58rem; opacity:.44; display:block; }
    .sc.hi  { background:#2563eb; border:1.5px solid rgba(255,255,255,.28); padding:5px 14px; }
    .sc.hi .sv { font-size:1.18rem; }

    .tab-wrap { background:#f8fafc; border-bottom:2px solid #e2e8f0; padding:9px 14px 0; display:flex; gap:4px; overflow-x:auto; }
    .tab-btn { padding:7px 14px; border:none; background:#e2e8f0; border-radius:7px 7px 0 0; cursor:pointer; font-size:.76rem; font-weight:600; color:#475569; white-space:nowrap; transition:background .17s,color .17s; }
    .tab-btn:hover { background:#cbd5e0; }
    .tab-btn.active { background:#1d4ed8; color:#fff; }

    .tab-pane { display:none; }
    .tab-pane.active { display:block; }
    .tab-content { max-width:820px; margin:0 auto; padding:16px 12px 52px; }

    .info-banner { background:#eff6ff; border:1px solid #bfdbfe; border-radius:8px; padding:9px 14px; font-size:.76rem; color:#1e40af; margin-bottom:14px; }

    .s-card { background:#fff; border-radius:10px; box-shadow:0 1px 10px rgba(0,0,0,.07); border:1px solid #e2e8f0; margin-bottom:18px; overflow:hidden; }
    .s-card-hdr { background:linear-gradient(90deg,#1e3a8a,#1d4ed8); color:#fff; padding:9px 15px; font-weight:700; font-size:.85rem; display:flex; justify-content:space-between; align-items:center; }
    .s-card-hdr .mx { opacity:.62; font-size:.74rem; }
    .sub-hdr { background:#eef2ff; padding:6px 14px; font-size:.77rem; font-weight:600; color:#3730a3; border-top:1px solid #e0e7ff; border-bottom:1px solid #e0e7ff; }

    .s-item { border-bottom:1px solid #f1f5f9; }
    .s-item:last-child { border-bottom:none; }
    .s-item-hdr { padding:8px 14px 2px; }
    .s-item-name { font-weight:600; font-size:.83rem; color:#0f172a; }
    .s-item-pos  { font-size:.7rem; color:#64748b; margin-top:2px; font-style:italic; }

    .opt-row { display:flex; align-items:center; gap:10px; padding:5px 14px 5px 20px; cursor:pointer; transition:background .13s; border-left:3px solid transparent; user-select:none; }
    .opt-row:hover { background:#f0f9ff; }
    .opt-row.sel   { background:#dbeafe; border-left-color:#1d4ed8; }

    .sdot { width:24px; height:24px; border-radius:50%; border:2px solid #cbd5e0; display:flex; align-items:center; justify-content:center; font-size:.7rem; font-weight:700; color:#9ca3af; flex-shrink:0; transition:all .13s; }
    .opt-row.sel        .sdot { color:#fff; border-color:transparent; }
    .opt-row.sel.s0 .sdot { background:#ef4444; }
    .opt-row.sel.s1 .sdot { background:#f59e0b; }
    .opt-row.sel.s2 .sdot { background:#10b981; }
    .opt-desc { font-size:.78rem; color:#4b5563; flex:1; line-height:1.4; }
    .opt-row.sel .opt-desc { color:#1e3a8a; font-weight:500; }

    .subtotal { background:#f8faff; padding:7px 14px; display:flex; justify-content:flex-end; align-items:center; gap:8px; font-size:.78rem; color:#64748b; border-top:1px solid #e2e8f0; }
    .st-val { background:#1d4ed8; color:#fff; border-radius:6px; padding:1px 11px; font-weight:700; font-size:.83rem; }

    .cond-notice { background:#fef3c7; border:1px solid #fbbf24; border-radius:6px; padding:7px 13px; font-size:.74rem; color:#92400e; margin:8px 13px; }
    .locked { opacity:.38; pointer-events:none; }

    .sum-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(160px,1fr)); gap:13px; margin-bottom:18px; }
    .sum-card { background:#fff; border-radius:10px; padding:14px; text-align:center; box-shadow:0 1px 8px rgba(0,0,0,.07); border:1px solid #e2e8f0; }
    .sum-card .cl { font-size:.71rem; color:#64748b; font-weight:600; margin-bottom:6px; }
    .sum-card .cv { font-size:1.75rem; font-weight:700; color:#1e3a8a; line-height:1; }
    .sum-card .cm { font-size:.71rem; color:#94a3b8; margin-top:2px; }
    .pbar  { background:#e2e8f0; border-radius:10px; height:7px; margin-top:8px; overflow:hidden; }
    .pfill { background:linear-gradient(90deg,#1d4ed8,#3b82f6); height:100%; border-radius:10px; transition:width .4s; }

    .total-card { background:linear-gradient(135deg,#1e3a8a,#1d4ed8); color:#fff; border-radius:10px; padding:22px 24px; text-align:center; box-shadow:0 4px 18px rgba(29,78,216,.3); margin-bottom:20px; }
    .total-card .tl { font-size:.87rem; opacity:.75; margin-bottom:6px; }
    .total-card .tv { font-size:2.7rem; font-weight:700; }
    .total-card .tp { font-size:.93rem; opacity:.68; margin-top:4px; }

    .btn-row { display:flex; gap:10px; flex-wrap:wrap; }
    .btn { padding:9px 20px; border:none; border-radius:8px; cursor:pointer; font-size:.84rem; font-weight:600; transition:opacity .17s; }
    .btn:hover { opacity:.83; }
    .btn-green { background:#059669; color:#fff; }
    .btn-red   { background:#dc2626; color:#fff; }
    .btn-gray  { background:#e2e8f0; color:#1e293b; }

    .toast { position:fixed; bottom:22px; left:50%; transform:translateX(-50%); background:#1e293b; color:#fff; padding:9px 20px; border-radius:8px; font-size:.8rem; font-weight:500; opacity:0; transition:opacity .3s; pointer-events:none; z-index:9999; }
    .toast.show { opacity:1; }

    .modal-overlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,.45); z-index:9000; justify-content:center; align-items:center; }
    .modal-overlay.open { display:flex; }
    .modal-box { background:#fff; border-radius:12px; padding:24px 28px; max-width:340px; width:90%; text-align:center; box-shadow:0 8px 32px rgba(0,0,0,.28); }
    .modal-box h3 { margin-bottom:10px; color:#1e293b; }
    .modal-box p  { font-size:.83rem; color:#64748b; margin-bottom:20px; }
    .modal-box .btn-row { justify-content:center; }

    @media (max-width:520px) { .app-header h1 { font-size:.95rem; } .sc { min-width:46px; } .sc .sv { font-size:.82rem; } }

    @media print {
      .score-bar,.tab-wrap,.btn-row,.toast,.modal-overlay,.app-header { display:none !important; }
      .tab-pane { display:block !important; }
      .tab-content { padding:0; max-width:100%; }
      .s-card { box-shadow:none; page-break-inside:avoid; }
    }
  </style>
</head>
<body>

<div class="app-header">
  <h1>Fugl-Meyer Assessment — Upper Extremity Motor Function (A–D)</h1>
  <p>Interactive Scoring Tool · Only Motor Sections A–D · Total A–D calculated automatically</p>
</div>

<div class="score-bar">
  <div class="sc"><span class="sl">A. Upper Ext.</span><span class="sv" id="sa">0</span><span class="sm">/36</span></div>
  <div class="sc"><span class="sl">B. Wrist</span><span class="sv" id="sb">0</span><span class="sm">/10</span></div>
  <div class="sc"><span class="sl">C. Hand</span><span class="sv" id="sc">0</span><span class="sm">/14</span></div>
  <div class="sc"><span class="sl">D. Coord.</span><span class="sv" id="sd">0</span><span class="sm">/6</span></div>
  <div class="sc hi"><span class="sl">TOTAL A–D (Motor)</span><span class="sv" id="sabcd">0</span><span class="sm">/66</span></div>
</div>

<div class="tab-wrap">
  <button class="tab-btn active" onclick="switchTab(this,'a')">A. Upper Extremity</button>
  <button class="tab-btn" onclick="switchTab(this,'b')">B. Wrist</button>
  <button class="tab-btn" onclick="switchTab(this,'c')">C. Hand</button>
  <button class="tab-btn" onclick="switchTab(this,'d')">D. Coordination</button>
  <button class="tab-btn" onclick="switchTab(this,'sum')">📊 Summary &amp; Total</button>
</div>

<div class="tab-content">
  <div class="tab-pane active" id="pane-a"></div>
  <div class="tab-pane" id="pane-b"></div>
  <div class="tab-pane" id="pane-c"></div>
  <div class="tab-pane" id="pane-d"></div>
  <div class="tab-pane" id="pane-sum"></div>
</div>

<div class="toast" id="toast"></div>
<div class="modal-overlay" id="resetModal">
  <div class="modal-box">
    <h3>Reset All Scores?</h3>
    <p>This will clear every entered score. This action cannot be undone.</p>
    <div class="btn-row">
      <button class="btn btn-gray" onclick="closeModal()">Cancel</button>
      <button class="btn btn-red"  onclick="confirmReset()">Reset</button>
    </div>
  </div>
</div>

<script>
// ═══════════════════════════ DATA (only A–D) ═══════════════════════════
const DATA = {
  a:{label:'A. Upper Extremity',max:36,parts:[
    {id:'ai',label:'I. Reflex Activity',max:4,note:'Sitting position',items:[
      {id:'a_flex',name:'Flexors',pos:'Biceps and finger flexors (at least one)',opts:[{v:0,d:'None — cannot be elicited'},{v:2,d:'Can be elicited'}]},
      {id:'a_ext', name:'Extensors',pos:'Triceps',opts:[{v:0,d:'None — cannot be elicited'},{v:2,d:'Can be elicited'}]}
    ]},
    {id:'aii',label:'II. Volitional Movement Within Synergies',max:18,note:'Without gravitational help',subs:[
      {label:'Flexor Synergy — Hand from contralateral knee to ipsilateral ear',items:[
        {id:'a_sr',name:'Shoulder Retraction',opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]},
        {id:'a_se',name:'Shoulder Elevation', opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]},
        {id:'a_sa',name:'Shoulder Abduction (90°)',opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]},
        {id:'a_sx',name:'Shoulder External Rotation',opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]},
        {id:'a_ef',name:'Elbow Flexion',  opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]},
        {id:'a_fs',name:'Forearm Supination',opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]}
      ]},
      {label:'Extensor Synergy — Hand from ipsilateral ear to contralateral knee',items:[
        {id:'a_si',name:'Shoulder Adduction / Internal Rotation',opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]},
        {id:'a_ee',name:'Elbow Extension',  opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]},
        {id:'a_fp',name:'Forearm Pronation',opts:[{v:0,d:'None'},{v:1,d:'Partial'},{v:2,d:'Full'}]}
      ]}
    ]},
    {id:'aiii',label:'III. Volitional Movement Mixing Synergies',max:6,note:'Without compensation',items:[
      {id:'a_hl',name:'Hand to Lumbar Spine',pos:'Hand on lap',
       opts:[{v:0,d:'Cannot perform or hand in front of ant-sup iliac spine'},{v:1,d:'Hand behind ant-sup iliac spine (without compensation)'},{v:2,d:'Hand to lumbar spine (without compensation)'}]},
      {id:'a_sf090',name:'Shoulder Flexion 0°–90°',pos:'Elbow at 0°, pronation-supination 0°',
       opts:[{v:0,d:'Immediate abduction or elbow flexion'},{v:1,d:'Abduction or elbow flexion during movement'},{v:2,d:'Flexion 90°, no shoulder abduction or elbow flexion'}]},
      {id:'a_ps90',name:'Pronation-Supination',pos:'Elbow at 90°, shoulder at 0°',
       opts:[{v:0,d:'No pronation/supination, starting position impossible'},{v:1,d:'Limited pronation/supination, maintains starting position'},{v:2,d:'Full pronation/supination, maintains starting position'}]}
    ]},
    {id:'aiv',label:'IV. Volitional Movement with Little or No Synergy',max:6,items:[
      {id:'a_sabd',name:'Shoulder Abduction 0°–90°',pos:'Elbow at 0°, forearm neutral',
       opts:[{v:0,d:'Immediate supination or elbow flexion'},{v:1,d:'Supination or elbow flexion during movement'},{v:2,d:'Abduction 90°, maintains extension and pronation'}]},
      {id:'a_sf180',name:'Shoulder Flexion 90°–180°',pos:'Elbow at 0°, pronation-supination 0°',
       opts:[{v:0,d:'Immediate abduction or elbow flexion'},{v:1,d:'Abduction or elbow flexion during movement'},{v:2,d:'Flexion 180°, no shoulder abduction or elbow flexion'}]},
      {id:'a_ps0',name:'Pronation / Supination',pos:'Elbow at 0°, shoulder at about 30° flexion',
       opts:[{v:0,d:'No pronation/supination, starting position impossible'},{v:1,d:'Limited pronation/supination, maintains start position'},{v:2,d:'Full pronation/supination, maintains starting position'}]}
    ]},
    {id:'av',label:'V. Normal Reflex Activity',max:2,note:'Assessed ONLY if Part IV achieves maximum score (6/6). Compare with unaffected side.',
     conditional:'aiv',condMax:6,items:[
      {id:'a_rn',name:'Biceps, Triceps, Finger Flexors',
       opts:[{v:0,d:'2 of 3 reflexes markedly hyperactive'},{v:1,d:'1 reflex markedly hyperactive or at least 2 reflexes lively'},{v:2,d:'Maximum of 1 reflex lively, none hyperactive (normal)'}]}
    ]}
  ]},
  b:{label:'B. Wrist',max:10,note:'Support may be provided at the elbow. No support at wrist. Check passive ROM prior testing.',parts:[
    {id:'bm',label:'Wrist Assessment',max:10,items:[
      {id:'b_s90',name:'Stability at 15° Dorsiflexion',pos:'Elbow at 90°, forearm pronated, shoulder at 0°',
       opts:[{v:0,d:'Less than 15° active dorsiflexion'},{v:1,d:'Dorsiflexion 15°, no resistance tolerated'},{v:2,d:'Maintains dorsiflexion against resistance'}]},
      {id:'b_r90',name:'Repeated Dorsiflexion / Volar Flexion',pos:'Elbow at 90°, forearm pronated, shoulder at 0°, slight finger flexion',
       opts:[{v:0,d:'Cannot perform volitionally'},{v:1,d:'Limited active range of motion'},{v:2,d:'Full active range of motion, smoothly'}]},
      {id:'b_s0',name:'Stability at 15° Dorsiflexion',pos:'Elbow at 0°, forearm pronated, slight shoulder flexion/abduction',
       opts:[{v:0,d:'Less than 15° active dorsiflexion'},{v:1,d:'Dorsiflexion 15°, no resistance tolerated'},{v:2,d:'Maintains dorsiflexion against resistance'}]},
      {id:'b_r0',name:'Repeated Dorsiflexion / Volar Flexion',pos:'Elbow at 0°, forearm pronated, slight shoulder flexion/abduction',
       opts:[{v:0,d:'Cannot perform volitionally'},{v:1,d:'Limited active range of motion'},{v:2,d:'Full active range of motion, smoothly'}]},
      {id:'b_circ',name:'Circumduction',pos:'Elbow at 90°, forearm pronated, shoulder at 0°',
       opts:[{v:0,d:'Cannot perform volitionally'},{v:1,d:'Jerky movement or incomplete'},{v:2,d:'Complete and smooth circumduction'}]}
    ]}
  ]},
  c:{label:'C. Hand',max:14,note:'Support at elbow to keep 90° flexion. No support at wrist. Compare with unaffected hand. Objects are interposed, active grasp.',parts:[
    {id:'cm',label:'Hand Assessment',max:14,items:[
      {id:'c_mf',name:'Mass Flexion',pos:'From full active or passive extension',
       opts:[{v:0,d:'Cannot perform'},{v:1,d:'Partial flexion'},{v:2,d:'Full active flexion'}]},
      {id:'c_me',name:'Mass Extension',pos:'From full active or passive flexion',
       opts:[{v:0,d:'Cannot perform'},{v:1,d:'Partial extension'},{v:2,d:'Full active extension'}]},
      {id:'c_hook',name:'a. Hook Grasp',pos:'Flexion in PIP and DIP (digits II–V), extension in MCP II–V',
       opts:[{v:0,d:'Cannot be performed'},{v:1,d:'Can hold position but weak'},{v:2,d:'Maintains position against resistance'}]},
      {id:'c_thumb',name:'b. Thumb Adduction',pos:'1st CMC, MCP, IP at 0° — scrap of paper between thumb and 2nd MCP joint',
       opts:[{v:0,d:'Cannot be performed'},{v:1,d:'Can hold paper but not against tug'},{v:2,d:'Can hold paper against a tug'}]},
      {id:'c_pincer',name:'c. Pincer Grasp (Opposition)',pos:'Pulpa of thumb against pulpa of 2nd finger — pencil, tug upward',
       opts:[{v:0,d:'Cannot be performed'},{v:1,d:'Can hold pencil but not against tug'},{v:2,d:'Can hold pencil against a tug'}]},
      {id:'c_cyl',name:'d. Cylinder Grasp',pos:'Cylinder-shaped object (small can) — tug upward, opposition of thumb and fingers',
       opts:[{v:0,d:'Cannot be performed'},{v:1,d:'Can hold cylinder but not against tug'},{v:2,d:'Can hold cylinder against a tug'}]},
      {id:'c_sph',name:'e. Spherical Grasp',pos:'Fingers in abduction/flexion, thumb opposed — tennis ball, tug away',
       opts:[{v:0,d:'Cannot be performed'},{v:1,d:'Can hold ball but not against tug'},{v:2,d:'Can hold ball against a tug'}]}
    ]}
  ]},
  d:{label:'D. Coordination / Speed',max:6,note:'Sitting. After one trial with both arms. Eyes closed. Tip of index finger from knee to nose, 5 times as fast as possible.',parts:[
    {id:'dm',label:'Coordination / Speed',max:6,items:[
      {id:'d_trem',name:'Tremor',
       opts:[{v:0,d:'Marked tremor'},{v:1,d:'Slight tremor'},{v:2,d:'No tremor'}]},
      {id:'d_dys',name:'Dysmetria',
       opts:[{v:0,d:'Pronounced or unsystematic dysmetria'},{v:1,d:'Slight and systematic dysmetria'},{v:2,d:'No dysmetria'}]},
      {id:'d_time',name:'Time',pos:'Start and end with the hand on the knee',
       opts:[{v:0,d:'≥ 6 seconds slower than unaffected side'},{v:1,d:'2–5 seconds slower than unaffected side'},{v:2,d:'Less than 2 seconds difference'}]}
    ]}
  ]}
};

// ═══════════════════════ STATE ═══════════════════════
const S = {};

// ═══════════════════════ HELPERS ═════════════════════
function getAllItems(part){
  let out=[];
  if(part.items) out=out.concat(part.items);
  if(part.subs)  part.subs.forEach(s=>{ out=out.concat(s.items); });
  return out;
}
function partTotal(part){ return getAllItems(part).reduce((s,it)=>s+(S[it.id]??0),0); }
function sectionTotal(k){
  const sec=DATA[k];
  return Math.min(sec.parts.reduce((s,p)=>{
    if(p.conditional){
      const ref=DATA.a.parts.find(x=>x.id===p.conditional);
      if(ref && partTotal(ref)<p.condMax) return s;
    }
    return s+partTotal(p);
  },0), sec.max);
}

// ═══════════════════════ RENDER ══════════════════════
function renderAll(){
  Object.keys(DATA).forEach(k=>{
    document.getElementById(`pane-${k}`).innerHTML = buildSection(DATA[k],k);
  });
  buildSummaryPane();
  checkConditionals();
}

function buildSection(sec){
  let h='';
  if(sec.note) h+=`<div class="info-banner">ℹ️ ${sec.note}</div>`;
  sec.parts.forEach(p=>{ h+=buildPart(p); });
  return h;
}

function buildPart(p){
  let h=`<div class="s-card" id="card-${p.id}">
    <div class="s-card-hdr">
      <span>${p.label}</span>
      <span class="mx">Subtotal: <span id="st-${p.id}">0</span> / ${p.max}</span>
    </div>`;
  if(p.note) h+=`<div class="info-banner" style="margin:8px 12px;font-size:.72rem;">⚠️ ${p.note}</div>`;
  if(p.conditional) h+=`<div class="cond-notice" id="cond-${p.id}" style="display:none;">⚠️ This section is scored <strong>only</strong> when Part IV achieves the full score of <strong>6 / 6</strong>.</div>`;
  h+=`<div id="body-${p.id}" class="${p.conditional?'locked':''}">`;
  if(p.subs) p.subs.forEach(sub=>{ h+=`<div class="sub-hdr">${sub.label}</div>`; sub.items.forEach(it=>{ h+=buildItem(it); }); });
  if(p.items) p.items.forEach(it=>{ h+=buildItem(it); });
  h+=`</div>
    <div class="subtotal">Subtotal &nbsp;<span class="st-val" id="stv-${p.id}">0</span>&nbsp;/ ${p.max}</div>
  </div>`;
  return h;
}

function buildItem(it){
  let h=`<div class="s-item" id="item-${it.id}">
    <div class="s-item-hdr">
      <div class="s-item-name">${it.name}</div>
      ${it.pos?`<div class="s-item-pos">${it.pos}</div>`:''}
    </div>`;
  it.opts.forEach(o=>{
    h+=`<div class="opt-row s${o.v}" id="opt-${it.id}-${o.v}" onclick="pick('${it.id}',${o.v})">
      <div class="sdot">${o.v}</div>
      <div class="opt-desc">${o.d}</div>
    </div>`;
  });
  h+=`</div>`;
  return h;
}

function buildSummaryPane(){
  document.getElementById('pane-sum').innerHTML=`
    <div class="total-card">
      <div class="tl">TOTAL MOTOR FUNCTION (A + B + C + D)</div>
      <div class="tv" id="tv-abcd">0 <span style="font-size:1.3rem;opacity:.55">/ 66</span></div>
      <div class="tp" id="tp-abcd">0% of maximum motor function</div>
    </div>
    <div class="sum-grid" id="sum-grid"></div>
    <div class="btn-row">
      <button class="btn btn-green" onclick="window.print()">🖨️ Print Assessment</button>
      <button class="btn btn-red"   onclick="openModal()">🔄 Reset All Scores</button>
    </div>`;
  refreshSummary();
}

function refreshSummary(){
  const grid=document.getElementById('sum-grid');
  if(!grid) return;
  const rows=[
    {k:'a',label:'A. Upper Extremity',max:36},
    {k:'b',label:'B. Wrist',max:10},
    {k:'c',label:'C. Hand',max:14},
    {k:'d',label:'D. Coordination/Speed',max:6}
  ];
  grid.innerHTML=rows.map(r=>{
    const v=sectionTotal(r.k), pct=Math.round(v/r.max*100);
    return `<div class="sum-card">
      <div class="cl">${r.label}</div>
      <div class="cv">${v}</div><div class="cm">/ ${r.max}</div>
      <div class="pbar"><div class="pfill" style="width:${pct}%"></div></div>
    </div>`;
  }).join('');
  const tot=sectionTotal('a')+sectionTotal('b')+sectionTotal('c')+sectionTotal('d');
  const pct=Math.round(tot/66*100);
  const el=document.getElementById('tv-abcd'), ep=document.getElementById('tp-abcd');
  if(el) el.innerHTML=`${tot} <span style="font-size:1.3rem;opacity:.55">/ 66</span>`;
  if(ep) ep.textContent=`${pct}% of maximum motor function`;
}

// ═══════════════════════ INTERACTION ═════════════════
function pick(id, val){
  S[id]=val;
  const el=document.getElementById(`item-${id}`);
  if(el){
    el.querySelectorAll('.opt-row').forEach(r=>r.classList.remove('sel'));
    const sel=document.getElementById(`opt-${id}-${val}`);
    if(sel) sel.classList.add('sel');
  }
  updateTotals();
  checkConditionals();
}

function updateTotals(){
  Object.values(DATA).forEach(sec=>{
    sec.parts.forEach(p=>{
      const t=partTotal(p);
      const e1=document.getElementById(`st-${p.id}`), e2=document.getElementById(`stv-${p.id}`);
      if(e1) e1.textContent=t;
      if(e2) e2.textContent=t;
    });
  });
  ['a','b','c','d'].forEach(k=>{
    const el=document.getElementById(`s${k}`);
    if(el) el.textContent=sectionTotal(k);
  });
  const tot=sectionTotal('a')+sectionTotal('b')+sectionTotal('c')+sectionTotal('d');
  document.getElementById('sabcd').textContent=tot;
  refreshSummary();
}

function checkConditionals(){
  const aiv=DATA.a.parts.find(p=>p.id==='aiv');
  const av =DATA.a.parts.find(p=>p.id==='av');
  if(!aiv||!av) return;
  const unlocked=partTotal(aiv)>=6;
  const body=document.getElementById('body-av'), notice=document.getElementById('cond-av');
  if(!body) return;
  if(unlocked){
    body.classList.remove('locked');
    if(notice) notice.style.display='none';
  } else {
    body.classList.add('locked');
    if(notice) notice.style.display='block';
    getAllItems(av).forEach(it=>{
      delete S[it.id];
      const el=document.getElementById(`item-${it.id}`);
      if(el) el.querySelectorAll('.opt-row').forEach(r=>r.classList.remove('sel'));
    });
    updateTotals();
  }
}

// ═══════════════════════ TABS ════════════════════════
function switchTab(btn, key){
  document.querySelectorAll('.tab-pane').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  document.getElementById(`pane-${key}`).classList.add('active');
  btn.classList.add('active');
  if(key==='sum') refreshSummary();
}

// ═══════════════════════ MODAL / RESET ═══════════════
function openModal()  { document.getElementById('resetModal').classList.add('open'); }
function closeModal() { document.getElementById('resetModal').classList.remove('open'); }
function confirmReset(){
  Object.keys(S).forEach(k=>delete S[k]);
  document.querySelectorAll('.opt-row').forEach(r=>r.classList.remove('sel'));
  closeModal();
  updateTotals();
  checkConditionals();
  showToast('All scores have been reset.');
}

function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),2500);
}

// ═══════════════════════ INIT ════════════════════════
renderAll();
</script>
</body>
</html>
