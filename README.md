<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>ELCANO — Esquema Salesforce</title>
<style>
:root{
  --bg:#f0f2f5;--surface:#fff;--surface2:#f7fafc;--surface3:#edf2f7;
  --text:#1a202c;--text2:#718096;--text3:#4a5568;
  --border:#e2e8f0;--accent:#2b6cb0;--canvas-bg:#fff;
  --btn-bg:#edf2f7;--btn-hover:#e2e8f0;
  --input-bg:#f7fafc;--input-border:#e2e8f0;
  --modal-overlay:rgba(0,0,0,.45);--shadow:rgba(0,0,0,.06);
}
body.dark{
  --bg:#0d1117;--surface:#161b22;--surface2:#21262d;--surface3:#30363d;
  --text:#e6edf3;--text2:#8b949e;--text3:#9198a1;
  --border:#30363d;--accent:#58a6ff;--canvas-bg:#0d1117;
  --btn-bg:#21262d;--btn-hover:#30363d;
  --input-bg:#21262d;--input-border:#444c56;
  --modal-overlay:rgba(0,0,0,.7);--shadow:rgba(0,0,0,.3);
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Segoe UI',system-ui,sans-serif;background:var(--bg);color:var(--text);
  height:100vh;overflow:hidden;display:flex;flex-direction:column;font-size:13px;transition:background .2s,color .2s}
#header{background:var(--surface);border-bottom:1px solid var(--border);
  padding:7px 12px;display:flex;align-items:center;gap:8px;
  flex-shrink:0;flex-wrap:wrap;box-shadow:0 1px 3px var(--shadow)}
#header h1{font-size:14px;font-weight:700;color:var(--accent);white-space:nowrap}
#header h1 span{color:var(--text2);font-weight:400;font-size:11px;margin-left:4px}
#search{flex:1;min-width:120px;max-width:180px;background:var(--input-bg);
  border:1px solid var(--input-border);border-radius:6px;padding:4px 9px;
  color:var(--text);font-size:12px;outline:none}
#search:focus{border-color:var(--accent)}
.filter-group{display:flex;gap:4px;flex-wrap:wrap}
.filter-btn{padding:2px 8px;border-radius:12px;border:1px solid;font-size:10px;
  font-weight:600;cursor:pointer;background:transparent;transition:opacity .15s}
.filter-btn.active{opacity:1}.filter-btn.inactive{opacity:.28}
.cat-custom{color:#276749;border-color:#276749}
.cat-standard{color:#2b6cb0;border-color:#2b6cb0}
.cat-metadata{color:#c05621;border-color:#c05621}
.cat-bigobject{color:#6b46c1;border-color:#6b46c1}
.cat-event{color:#c53030;border-color:#c53030}
body.dark .cat-custom{color:#6ee7b7;border-color:#6ee7b7}
body.dark .cat-standard{color:#93c5fd;border-color:#93c5fd}
body.dark .cat-metadata{color:#fdba74;border-color:#fdba74}
body.dark .cat-bigobject{color:#c4b5fd;border-color:#c4b5fd}
body.dark .cat-event{color:#fca5a5;border-color:#fca5a5}
#profile-bar{display:flex;align-items:center;gap:4px}
#profile-bar label{font-size:10px;color:var(--text2);white-space:nowrap}
#profile-select{background:var(--input-bg);border:1px solid var(--input-border);
  border-radius:6px;color:var(--text);font-size:11px;padding:3px 6px;cursor:pointer;outline:none;max-width:180px}
#profile-select:focus{border-color:var(--accent)}
#multisel-badge{font-size:10px;background:#f59e0b;color:#fff;
  padding:1px 8px;border-radius:10px;font-weight:700;white-space:nowrap;display:none;cursor:pointer}
#multisel-badge:hover{background:#d97706}
#controls{margin-left:auto;display:flex;gap:4px;flex-wrap:wrap}
.ctrl-btn{background:var(--btn-bg);border:1px solid var(--border);border-radius:5px;
  color:var(--text3);padding:3px 8px;font-size:11px;cursor:pointer;transition:background .15s;white-space:nowrap}
.ctrl-btn:hover{background:var(--btn-hover)}
.ctrl-btn.active{background:var(--accent);color:#fff;border-color:var(--accent)}
#main{display:flex;flex:1;overflow:hidden}
#canvas-wrap{flex:1;position:relative;background:var(--canvas-bg)}
canvas{display:block}
.legend{position:absolute;bottom:10px;left:10px;background:var(--surface);
  border:1px solid var(--border);border-radius:6px;padding:7px 11px;font-size:10px;
  box-shadow:0 1px 4px var(--shadow)}
.legend-item{display:flex;align-items:center;gap:5px;padding:1px 0;color:var(--text3)}
.legend-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.multisel-hint{position:absolute;bottom:10px;right:10px;background:var(--surface);
  border:1px solid var(--border);border-radius:6px;padding:5px 10px;font-size:10px;
  color:var(--text2);box-shadow:0 1px 4px var(--shadow);pointer-events:none}
#obj-panel{width:300px;background:var(--surface);border-left:1px solid var(--border);
  overflow-y:auto;flex-shrink:0;padding:12px 14px}
#obj-empty{color:var(--text2);font-size:12px;padding:20px 0;text-align:center;line-height:1.8}
#obj-name{font-size:14px;font-weight:700;color:var(--text)}
.obj-badge{font-size:9px;font-weight:700;padding:1px 6px;border-radius:8px;display:inline-block;margin-top:3px}
.obj-meta{font-size:11px;color:var(--text2);margin-top:4px}
.rel-section{margin-top:9px}
.rel-section h4{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.07em;
  color:var(--text2);margin-bottom:4px}
.rel-row{display:flex;align-items:center;gap:4px;padding:3px 0;
  border-bottom:1px solid var(--border);font-size:11px}
.rel-row:last-child{border:none}
.rel-badge{font-size:8px;font-weight:700;padding:1px 4px;border-radius:2px;flex-shrink:0}
.rbadge-md{background:#e9d8fd;color:#6b46c1}
.rbadge-lu{background:#bee3f8;color:#2b6cb0}
body.dark .rbadge-md{background:#44337a;color:#c4b5fd}
body.dark .rbadge-lu{background:#1e3a5f;color:#93c5fd}
.card-tag{font-size:8px;font-weight:700;padding:1px 4px;border-radius:2px;
  background:#f0fff4;color:#276749;border:1px solid #c6f6d5;flex-shrink:0}
body.dark .card-tag{background:#1a4731;color:#6ee7b7;border-color:#276749}
.rel-link{color:var(--accent);cursor:pointer;font-weight:500;overflow:hidden;
  text-overflow:ellipsis;white-space:nowrap}
.rel-link:hover{text-decoration:underline}
.field-row{font-size:10px;padding:2px 0;color:var(--text3);
  border-bottom:1px solid var(--border);display:flex;justify-content:space-between;gap:4px}
.field-row:last-child{border:none}
.field-type-tag{color:var(--text2);font-size:9px;flex-shrink:0}
/* ── path panel ── */
.path-node-chip{display:inline-flex;align-items:center;gap:4px;padding:2px 7px;
  border-radius:10px;font-size:10px;font-weight:600;margin:2px 2px;cursor:pointer;border:none;
  transition:opacity .15s}
.path-node-chip:hover{opacity:.8}
.path-route{padding:6px 0;border-bottom:1px solid var(--border)}
.path-route:last-child{border:none}
.path-route-header{display:flex;align-items:center;gap:5px;font-size:11px;font-weight:600;margin-bottom:3px}
.path-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.path-route-detail{font-size:10px;color:var(--text2);padding-left:14px;line-height:1.6}
.path-route-detail .hop{color:var(--text3);font-weight:500}
.path-none{font-size:10px;color:#e53e3e;padding-left:14px}
/* ── modal ── */
.modal-overlay{position:fixed;inset:0;background:var(--modal-overlay);
  z-index:1000;display:flex;align-items:center;justify-content:center}
.modal-box{background:var(--surface);border:1px solid var(--border);border-radius:10px;
  padding:20px;width:580px;max-width:95vw;max-height:85vh;display:flex;flex-direction:column;
  box-shadow:0 8px 32px rgba(0,0,0,.25)}
.modal-box h2{font-size:15px;font-weight:700;color:var(--text);margin-bottom:14px}
.modal-field{margin-bottom:11px}
.modal-field label{font-size:11px;font-weight:600;color:var(--text2);display:block;margin-bottom:4px}
.modal-field input,.modal-field select{width:100%;background:var(--input-bg);
  border:1px solid var(--input-border);border-radius:6px;padding:5px 9px;
  color:var(--text);font-size:12px;outline:none}
.modal-field input:focus,.modal-field select:focus{border-color:var(--accent)}
#pf-obj-wrap{flex:1;overflow:hidden;display:flex;flex-direction:column;min-height:0}
#pf-obj-search{width:100%;background:var(--input-bg);border:1px solid var(--input-border);
  border-radius:6px;padding:5px 9px;color:var(--text);font-size:12px;outline:none;margin-bottom:6px}
#pf-obj-search:focus{border-color:var(--accent)}
#pf-obj-list{flex:1;overflow-y:auto;border:1px solid var(--border);border-radius:6px;
  padding:4px 0;background:var(--surface2)}
.pf-cat-header{padding:4px 10px;font-size:9px;font-weight:700;text-transform:uppercase;
  letter-spacing:.07em;color:var(--text2);background:var(--surface3);
  display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:1}
.pf-cat-header button{font-size:9px;padding:1px 6px;border-radius:3px;cursor:pointer;
  background:transparent;border:1px solid var(--border);color:var(--text2)}
.pf-cat-header button:hover{background:var(--btn-hover)}
.pf-obj-item{display:flex;align-items:center;gap:7px;padding:4px 10px;cursor:pointer;
  font-size:11px;color:var(--text);border-bottom:1px solid var(--border)}
.pf-obj-item:last-child{border:none}
.pf-obj-item:hover{background:var(--surface3)}
.pf-obj-item input[type=checkbox]{width:13px;height:13px;flex-shrink:0;cursor:pointer;accent-color:var(--accent)}
.pf-obj-label{flex:1;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.pf-obj-id{font-size:9px;color:var(--text2);flex-shrink:0;max-width:160px;
  overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
#pf-count{font-size:10px;color:var(--text2);padding:3px 0 6px;text-align:right}
.modal-actions{display:flex;gap:8px;justify-content:flex-end;margin-top:12px;padding-top:10px;
  border-top:1px solid var(--border)}
.btn-primary{background:var(--accent);color:#fff;border:none;border-radius:6px;
  padding:6px 16px;font-size:12px;font-weight:600;cursor:pointer}
.btn-primary:hover{opacity:.88}
.btn-secondary{background:var(--btn-bg);color:var(--text3);border:1px solid var(--border);
  border-radius:6px;padding:6px 14px;font-size:12px;cursor:pointer}
.btn-secondary:hover{background:var(--btn-hover)}
.confirm-box{background:var(--surface);border:1px solid var(--border);border-radius:8px;
  padding:18px 20px;width:300px;box-shadow:0 4px 16px rgba(0,0,0,.2)}
.confirm-box p{font-size:13px;color:var(--text);margin-bottom:14px}
.btn-danger{background:#c53030;color:#fff;border:none;border-radius:6px;
  padding:6px 14px;font-size:12px;font-weight:600;cursor:pointer}
.btn-danger:hover{background:#9b2c2c}
</style>
</head>
<body>

<div id="header">
  <h1>ELCANO <span>Salesforce Schema</span></h1>

  <div id="profile-bar">
    <label>Perfil:</label>
    <select id="profile-select"><option value="">— Todos —</option></select>
    <button class="ctrl-btn" id="btn-new-profile" title="Crear perfil">+ Perfil</button>
    <button class="ctrl-btn" id="btn-edit-profile" style="display:none" title="Editar">✏️</button>
    <button class="ctrl-btn" id="btn-del-profile"  style="display:none" title="Eliminar">🗑️</button>
  </div>

  <input id="search" placeholder="🔍 Buscar objeto…">

  <div class="filter-group">
    <button class="filter-btn active cat-custom"    data-cat="custom">Custom</button>
    <button class="filter-btn active cat-standard"  data-cat="standard">Standard</button>
    <button class="filter-btn active cat-metadata"  data-cat="metadata">Metadata</button>
    <button class="filter-btn active cat-bigobject" data-cat="bigobject">BigObject</button>
    <button class="filter-btn active cat-event"     data-cat="event">Evento</button>
  </div>

  <span id="multisel-badge" title="Clic para limpiar selección múltiple"></span>

  <div id="controls">
    <button class="ctrl-btn" id="btn-labels">Etiquetas</button>
    <button class="ctrl-btn" id="btn-fit">⊡ Ajustar</button>
    <button class="ctrl-btn" id="btn-theme">🌙 Oscuro</button>
  </div>
</div>

<!-- PROFILE MODAL -->
<div id="profile-modal" class="modal-overlay" style="display:none">
  <div class="modal-box">
    <h2 id="modal-title">Nuevo perfil</h2>
    <div class="modal-field">
      <label>Nombre del perfil</label>
      <input id="pf-name" type="text" placeholder="Ej: Mi área">
    </div>
    <div class="modal-field">
      <label>Área / Departamento (rellena la selección automáticamente)</label>
      <select id="pf-area">
        <option value="">— Seleccionar —</option>
        <optgroup label="PROGRAMA">
          <option value="Programa — Transversal (Business Partner)">Transversal — Business Partner</option>
          <option value="Programa — Ventas">Ventas</option>
          <option value="Programa — Legal Backoffice">Legal Backoffice (Documentación)</option>
          <option value="Programa — Atención al Cliente">Atención al Cliente (Cartera)</option>
          <option value="Programa — Negociación">Negociación (Prejudicial / Amistoso / Judicial)</option>
          <option value="Programa — Dirección Jurídica">Dirección Jurídica</option>
          <option value="Programa — Controller">Controller / Finanzas</option>
          <option value="Programa — Calidad">Calidad</option>
        </optgroup>
        <optgroup label="OTRAS ÁREAS SMD">
          <option value="LSO">LSO (Legal Service Operations)</option>
          <option value="Marketing y Preventa">Marketing y Preventa</option>
          <option value="Servicios Centrales">Servicios Centrales (RRHH / Finanzas)</option>
          <option value="Reclamadora">Reclamadora</option>
          <option value="IT / Tecnología">IT / Tecnología</option>
        </optgroup>
      </select>
    </div>
    <div id="pf-obj-wrap">
      <div class="modal-field" style="margin-bottom:6px">
        <label>Objetos visibles <span id="pf-count"></span></label>
        <input id="pf-obj-search" type="text" placeholder="🔍 Filtrar objetos…">
      </div>
      <div id="pf-obj-list"></div>
    </div>
    <div class="modal-actions">
      <button class="btn-secondary" id="pf-cancel">Cancelar</button>
      <button class="btn-primary"   id="pf-save">Guardar perfil</button>
    </div>
  </div>
</div>

<!-- CONFIRM MODAL -->
<div id="confirm-modal" class="modal-overlay" style="display:none">
  <div class="confirm-box">
    <p id="confirm-msg">¿Eliminar este perfil?</p>
    <div class="modal-actions">
      <button class="btn-secondary" id="confirm-no">Cancelar</button>
      <button class="btn-danger"    id="confirm-yes">Eliminar</button>
    </div>
  </div>
</div>

<div id="main">
  <div id="canvas-wrap">
    <canvas id="c"></canvas>
    <div class="legend">
      <div class="legend-item"><div class="legend-dot" style="background:#c6f6d5;border:1.5px solid #276749"></div>Custom</div>
      <div class="legend-item"><div class="legend-dot" style="background:#bee3f8;border:1.5px solid #2b6cb0"></div>Standard</div>
      <div class="legend-item"><div class="legend-dot" style="background:#feebc8;border:1.5px solid #c05621"></div>Metadata</div>
      <div class="legend-item"><div class="legend-dot" style="background:#e9d8fd;border:1.5px solid #6b46c1"></div>BigObject</div>
      <div class="legend-item"><div class="legend-dot" style="background:#fed7d7;border:1.5px solid #c53030"></div>Evento</div>
      <div style="margin-top:5px;padding-top:4px;border-top:1px solid var(--border)">
        <div class="legend-item"><span style="display:inline-block;width:18px;height:2px;background:#6b46c1;margin-right:5px"></span>MasterDetail</div>
        <div class="legend-item"><span style="display:inline-block;width:18px;height:1.5px;background:#2b6cb0;margin-right:5px"></span>Lookup req.</div>
        <div class="legend-item"><span style="display:inline-block;width:18px;border-top:1px dashed #a0aec0;margin-right:5px"></span>Lookup opc.</div>
      </div>
    </div>
    <div class="multisel-hint">Ctrl+clic para multi-seleccionar (máx 4)</div>
  </div>
  <div id="obj-panel">
    <div id="obj-empty" style="display:block">
      <div style="font-size:22px;margin-bottom:8px">🔍</div>
      Haz clic en un nodo para ver detalles<br>
      <span style="font-size:10px">Ctrl+clic para seleccionar hasta 4<br>y ver rutas entre ellos</span>
    </div>
    <div id="obj-content" style="display:none"></div>
  </div>
</div>

<script>window.__ELCANO_DATA__={"graph":{"nodes":[{"id":"AWS_Connect_Settings__mdt","label":"AWS Connect Settings","cat":"metadata","fc":5,"out":0},{"id":"Account","label":"Account (Cliente)","cat":"standard","fc":45,"out":6},{"id":"AccountInfo__c","label":"AccountInfo","cat":"custom","fc":137,"out":2},{"id":"AccountRelationship__c","label":"AccountRelationship","cat":"custom","fc":7,"out":3},{"id":"Activity","label":"Activity","cat":"standard","fc":5,"out":0},{"id":"BankCIF__mdt","label":"BankCIF","cat":"metadata","fc":3,"out":0},{"id":"BankHolded__c","label":"BankHolded","cat":"custom","fc":2,"out":0},{"id":"Bankruptcy__c","label":"Bankruptcy","cat":"custom","fc":63,"out":4},{"id":"Batch_settings__mdt","label":"Batch settings","cat":"metadata","fc":8,"out":0},{"id":"Billing__c","label":"Billing","cat":"custom","fc":12,"out":3},{"id":"Burofax__c","label":"Burofax","cat":"custom","fc":54,"out":5},{"id":"Campaign","label":"Campaign","cat":"standard","fc":12,"out":0},{"id":"CampaignMember","label":"Campaign Member","cat":"standard","fc":19,"out":2},{"id":"Case","label":"Case","cat":"standard","fc":35,"out":7},{"id":"CatchmentSourceEquivalence__c","label":"CatchmentSourceEquivalence","cat":"custom","fc":6,"out":0},{"id":"CertifiedEmailFiles__c","label":"CertifiedEmailFiles","cat":"custom","fc":4,"out":1},{"id":"CertifiedEmail__c","label":"CertifiedEmail","cat":"custom","fc":6,"out":1},{"id":"Client_Profile_Change__c","label":"Client Profile Change","cat":"custom","fc":8,"out":2},{"id":"CommunityGate__mdt","label":"CommunityGate","cat":"metadata","fc":2,"out":0},{"id":"Configuracion_Slots_Disponibles__mdt","label":"Configuracion Slots Disponibles","cat":"metadata","fc":7,"out":0},{"id":"ContentDocument","label":"ContentDocument","cat":"standard","fc":0,"out":0},{"id":"ContentVersion","label":"ContentVersion","cat":"standard","fc":29,"out":3},{"id":"Contract","label":"Contract","cat":"standard","fc":17,"out":4},{"id":"ContractAccount__c","label":"ContractAccount","cat":"custom","fc":2,"out":2},{"id":"Conversation_History__b","label":"Conversation History","cat":"bigobject","fc":8,"out":0},{"id":"CreditDefaultFileAnalysis__c","label":"CreditDefaultFileAnalysis","cat":"custom","fc":17,"out":5},{"id":"CreditDefaultFileResult__e","label":"CreditDefaultFileResult","cat":"event","fc":3,"out":0},{"id":"CreditDefaultFile__c","label":"CreditDefaultFile","cat":"custom","fc":24,"out":3},{"id":"DebtCaseDepartment__c","label":"DebtCaseDepartment","cat":"custom","fc":1,"out":0},{"id":"DebtCaseReasonCatalog__c","label":"DebtCaseReasonCatalog","cat":"custom","fc":13,"out":3},{"id":"DebtCaseTypeCatalog__c","label":"DebtCaseTypeCatalog","cat":"custom","fc":2,"out":1},{"id":"DebtHolder__c","label":"DebtHolder","cat":"custom","fc":4,"out":2},{"id":"DebtLawsuit__c","label":"DebtLawsuit","cat":"custom","fc":10,"out":2},{"id":"DebtSettlement__c","label":"DebtSettlement","cat":"custom","fc":60,"out":7},{"id":"DebtStatusExperienceSite__mdt","label":"DebtStatusExperienceSite","cat":"metadata","fc":3,"out":0},{"id":"DebtStatusHistory__c","label":"DebtStatusHistory","cat":"custom","fc":6,"out":1},{"id":"Debt__c","label":"Debt","cat":"custom","fc":204,"out":20},{"id":"DebtsettlementLine__c","label":"DebtsettlementLine","cat":"custom","fc":17,"out":2},{"id":"DeletedJournalEntry__c","label":"DeletedJournalEntry","cat":"custom","fc":23,"out":4},{"id":"DepartmentalManagers__mdt","label":"DepartmentalManagers","cat":"metadata","fc":1,"out":0},{"id":"DocumentSignatureConfiguration__mdt","label":"DocumentSignatureConfiguration","cat":"metadata","fc":8,"out":0},{"id":"EmailMessage","label":"Email Message","cat":"standard","fc":1,"out":1},{"id":"Email_To_Case_Settings__mdt","label":"Email To Case Settings","cat":"metadata","fc":4,"out":0},{"id":"EntityProduct__c","label":"EntityProduct","cat":"custom","fc":18,"out":1},{"id":"Entity__c","label":"Entity","cat":"custom","fc":24,"out":2},{"id":"ErrorLog__c","label":"ErrorLog","cat":"custom","fc":12,"out":0},{"id":"Good__c","label":"Good","cat":"custom","fc":108,"out":2},{"id":"History__c","label":"History","cat":"custom","fc":21,"out":12},{"id":"HojaDeEncargoSMD__mdt","label":"HojaDeEncargoSMD","cat":"metadata","fc":9,"out":0},{"id":"IBAN__c","label":"IBAN","cat":"custom","fc":6,"out":1},{"id":"Kmaleon_Auth__mdt","label":"Kmaleon Auth","cat":"metadata","fc":4,"out":0},{"id":"Kmaleon_Setting__mdt","label":"Kmaleon Setting","cat":"metadata","fc":7,"out":0},{"id":"LSODocumentEquivalence__c","label":"LSODocumentEquivalence","cat":"custom","fc":3,"out":0},{"id":"LSODocument__c","label":"LSODocument","cat":"custom","fc":13,"out":3},{"id":"LSOWordDocumentType__mdt","label":"LSOWordDocumentType","cat":"metadata","fc":5,"out":0},{"id":"LSO_cmt_Document__mdt","label":"LSO cmt Document","cat":"metadata","fc":3,"out":0},{"id":"LawsuitLog__c","label":"LawsuitLog","cat":"custom","fc":8,"out":1},{"id":"Lawsuit__c","label":"Lawsuit","cat":"custom","fc":181,"out":10},{"id":"Lead","label":"Lead","cat":"standard","fc":152,"out":7},{"id":"LegalProfessional__mdt","label":"LegalProfessional","cat":"metadata","fc":5,"out":0},{"id":"MASC_Configuration_Email__mdt","label":"MASC Configuration Email","cat":"metadata","fc":2,"out":0},{"id":"ManagerSlotConfiguration__mdt","label":"ManagerSlotConfiguration","cat":"metadata","fc":5,"out":0},{"id":"Notificados__mdt","label":"Notificados","cat":"metadata","fc":15,"out":0},{"id":"NotificationMessageSetting__mdt","label":"NotificationMessageSetting","cat":"metadata","fc":1,"out":0},{"id":"NotificationTypeIds__mdt","label":"NotificationTypeIds","cat":"metadata","fc":1,"out":0},{"id":"Notification__c","label":"Notification","cat":"custom","fc":9,"out":2},{"id":"Opportunity","label":"Opportunity","cat":"standard","fc":20,"out":5},{"id":"OwnerAssignment__mdt","label":"OwnerAssignment","cat":"metadata","fc":6,"out":0},{"id":"PaymentGatewayInstallment__c","label":"PaymentGatewayInstallment","cat":"custom","fc":11,"out":3},{"id":"PaymentGatewayToken__c","label":"PaymentGatewayToken","cat":"custom","fc":24,"out":1},{"id":"PaymentIntegrationConfiguration__mdt","label":"PaymentIntegrationConfiguration","cat":"metadata","fc":8,"out":0},{"id":"PersonAccount","label":"Person Account","cat":"standard","fc":0,"out":1},{"id":"PowerAttorney__c","label":"PowerAttorney","cat":"custom","fc":34,"out":1},{"id":"Pricebook2","label":"Pricebook","cat":"standard","fc":4,"out":0},{"id":"PricebookEntry","label":"Pricebook Entry","cat":"standard","fc":2,"out":2},{"id":"Process__c","label":"Process","cat":"custom","fc":56,"out":2},{"id":"Product2","label":"Product","cat":"standard","fc":1,"out":0},{"id":"Quote","label":"Quote","cat":"standard","fc":36,"out":4},{"id":"RoundRobinLastAssignment__c","label":"RoundRobinLastAssignment","cat":"custom","fc":1,"out":0},{"id":"SalesDebt__c","label":"SalesDebt","cat":"custom","fc":34,"out":5},{"id":"Saving__c","label":"Saving","cat":"custom","fc":39,"out":9},{"id":"Service__c","label":"Service","cat":"custom","fc":92,"out":8},{"id":"SettlementInstallment__c","label":"SettlementInstallment","cat":"custom","fc":10,"out":2},{"id":"SettlementPayment__c","label":"SettlementPayment","cat":"custom","fc":7,"out":3},{"id":"SignaturitContracts__mdt","label":"SignaturitContracts","cat":"metadata","fc":16,"out":0},{"id":"SpanishBank__mdt","label":"SpanishBank","cat":"metadata","fc":7,"out":0},{"id":"StageTransition__mdt","label":"StageTransition","cat":"metadata","fc":7,"out":0},{"id":"Task__mdt","label":"Task","cat":"metadata","fc":6,"out":0},{"id":"TemplateWhatsappMetadata__mdt","label":"TemplateWhatsappMetadata","cat":"metadata","fc":2,"out":0},{"id":"TicketHolded__c","label":"TicketHolded","cat":"custom","fc":6,"out":3},{"id":"TransferType__mdt","label":"TransferType","cat":"metadata","fc":4,"out":0},{"id":"Transfer__c","label":"Transfer","cat":"custom","fc":13,"out":2},{"id":"User","label":"User","cat":"standard","fc":9,"out":0},{"id":"Vendor_Config__mdt","label":"Vendor Config","cat":"metadata","fc":4,"out":0},{"id":"Vendor_Payment__c","label":"Vendor Payment","cat":"custom","fc":11,"out":4},{"id":"VoiceCall","label":"Voice Call","cat":"standard","fc":25,"out":2},{"id":"VoiceCall_Link__c","label":"VoiceCall Link","cat":"custom","fc":9,"out":9},{"id":"WhatsApp_Queue_Mapping__mdt","label":"WhatsApp Queue Mapping","cat":"metadata","fc":3,"out":0},{"id":"signaturit__SignatureRequestFile__c","label":"SignatureRequestFile","cat":"custom","fc":14,"out":1},{"id":"signaturit__SignatureRequestSigner__c","label":"SignatureRequestSigner","cat":"custom","fc":17,"out":5},{"id":"signaturit__SignatureRequest__c","label":"SignatureRequest","cat":"custom","fc":52,"out":2},{"id":"signaturit__SignatureSignerFile__c","label":"SignatureSignerFile","cat":"custom","fc":1,"out":0}],"edges":[{"s":"Account","t":"User","f":"Assigned_document_manager__c","r":"Lookup","req":false},{"s":"Account","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"Account","t":"Entity__c","f":"MortgageBankLook__c","r":"Lookup","req":false},{"s":"Account","t":"Entity__c","f":"PayrollBank__c","r":"Lookup","req":false},{"s":"Account","t":"Entity__c","f":"RecoveryEntity__c","r":"Lookup","req":false},{"s":"AccountInfo__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"AccountInfo__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"AccountRelationship__c","t":"Account","f":"AccountFrom__c","r":"Lookup","req":false},{"s":"AccountRelationship__c","t":"Account","f":"AccountTo__c","r":"Lookup","req":false},{"s":"AccountRelationship__c","t":"AccountRelationship__c","f":"InverseAccountRelationship__c","r":"Lookup","req":false},{"s":"Bankruptcy__c","t":"Account","f":"AccountPartner__c","r":"Lookup","req":false},{"s":"Bankruptcy__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Bankruptcy__c","t":"User","f":"AssignedTo__c","r":"Lookup","req":false},{"s":"Bankruptcy__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"Billing__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Billing__c","t":"Saving__c","f":"Saving__c","r":"Lookup","req":false},{"s":"Billing__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"Burofax__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Burofax__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"Burofax__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"Burofax__c","t":"EntityProduct__c","f":"ProductEntity__c","r":"Lookup","req":false},{"s":"Burofax__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"Case","t":"DebtCaseReasonCatalog__c","f":"DebtCaseReason__c","r":"Lookup","req":false},{"s":"Case","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"Case","t":"DebtCaseDepartment__c","f":"DestinationDepartment__c","r":"Lookup","req":false},{"s":"Case","t":"DebtCaseDepartment__c","f":"OriginDepartment__c","r":"Lookup","req":false},{"s":"Case","t":"User","f":"RequestedBy__c","r":"Lookup","req":false},{"s":"CertifiedEmailFiles__c","t":"CertifiedEmail__c","f":"CertifiedEmail__c","r":"Lookup","req":false},{"s":"CertifiedEmail__c","t":"Burofax__c","f":"Burofax__c","r":"Lookup","req":false},{"s":"Client_Profile_Change__c","t":"AccountInfo__c","f":"AccountInfo__c","r":"Lookup","req":false},{"s":"Client_Profile_Change__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"ContentVersion","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"ContentVersion","t":"User","f":"ValidatedBy__c","r":"Lookup","req":false},{"s":"Contract","t":"Opportunity","f":"Opportunity__c","r":"Lookup","req":false},{"s":"Contract","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"ContractAccount__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"ContractAccount__c","t":"Contract","f":"Contract__c","r":"Lookup","req":false},{"s":"CreditDefaultFileAnalysis__c","t":"User","f":"Analyzer__c","r":"Lookup","req":false},{"s":"CreditDefaultFileAnalysis__c","t":"CreditDefaultFile__c","f":"CreditDefaultFile__c","r":"MasterDetail","req":true},{"s":"CreditDefaultFileAnalysis__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"CreditDefaultFileAnalysis__c","t":"Entity__c","f":"EeportingEntity__c","r":"Lookup","req":false},{"s":"CreditDefaultFileAnalysis__c","t":"Entity__c","f":"SignatoryEntity__c","r":"Lookup","req":false},{"s":"CreditDefaultFile__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"CreditDefaultFile__c","t":"Opportunity","f":"Opportunity__c","r":"Lookup","req":false},{"s":"CreditDefaultFile__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"DebtCaseReasonCatalog__c","t":"DebtCaseTypeCatalog__c","f":"DebtCaseType__c","r":"MasterDetail","req":true},{"s":"DebtCaseReasonCatalog__c","t":"DebtCaseDepartment__c","f":"DestinationDepartment__c","r":"Lookup","req":false},{"s":"DebtCaseReasonCatalog__c","t":"User","f":"ResponsibleInCharge__c","r":"Lookup","req":false},{"s":"DebtCaseTypeCatalog__c","t":"DebtCaseDepartment__c","f":"DebtCaseRequestingDepartment__c","r":"MasterDetail","req":true},{"s":"DebtHolder__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"DebtHolder__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"DebtLawsuit__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"DebtLawsuit__c","t":"Lawsuit__c","f":"Lawsuit__c","r":"Lookup","req":false},{"s":"DebtSettlement__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"DebtSettlement__c","t":"Account","f":"EntityContact__c","r":"Lookup","req":false},{"s":"DebtSettlement__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"DebtSettlement__c","t":"IBAN__c","f":"IBAN__c","r":"Lookup","req":false},{"s":"DebtSettlement__c","t":"Lawsuit__c","f":"Lawsuit__c","r":"Lookup","req":false},{"s":"DebtSettlement__c","t":"User","f":"ReceiptRequestedBy__c","r":"Lookup","req":false},{"s":"DebtSettlement__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"DebtStatusHistory__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"Debt__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Debt__c","t":"Entity__c","f":"DebtRecovery__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"DocManager__c","r":"Lookup","req":false},{"s":"Debt__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"Head_Of_Banking_Analysis__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"Head_of_analysis_Documentary__c","r":"Lookup","req":false},{"s":"Debt__c","t":"IBAN__c","f":"IBAN__c","r":"Lookup","req":false},{"s":"Debt__c","t":"Entity__c","f":"InstallmentPaymentAgreementEntity__c","r":"Lookup","req":false},{"s":"Debt__c","t":"Lead","f":"Lead__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"ManualBurofaxManager__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"MarkedBy__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"Marked_By_Documentary__c","r":"Lookup","req":false},{"s":"Debt__c","t":"Debt__c","f":"ParentDebt__c","r":"Lookup","req":false},{"s":"Debt__c","t":"Entity__c","f":"PreviousEntity__c","r":"Lookup","req":false},{"s":"Debt__c","t":"EntityProduct__c","f":"ProductEntity__c","r":"Lookup","req":false},{"s":"Debt__c","t":"EntityProduct__c","f":"ProductPreviousEntity__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"ResponsibleReviewer__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"Review_Manager_Banking__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"Review_Manager_Documentary__c","r":"Lookup","req":false},{"s":"Debt__c","t":"User","f":"ReviewedBy__c","r":"Lookup","req":false},{"s":"DebtsettlementLine__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"DebtsettlementLine__c","t":"DebtSettlement__c","f":"Debtsettlement__c","r":"Lookup","req":false},{"s":"DeletedJournalEntry__c","t":"DebtSettlement__c","f":"DebtSettlement__c","r":"Lookup","req":false},{"s":"DeletedJournalEntry__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"DeletedJournalEntry__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"DeletedJournalEntry__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"EntityProduct__c","t":"Entity__c","f":"Entity__c","r":"MasterDetail","req":true},{"s":"Entity__c","t":"User","f":"Assigned_document_manager__c","r":"Lookup","req":false},{"s":"Entity__c","t":"User","f":"Negotiator__c","r":"Lookup","req":false},{"s":"Good__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Good__c","t":"Lead","f":"Lead__c","r":"Lookup","req":false},{"s":"History__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"History__c","t":"Bankruptcy__c","f":"Bankruptcy__c","r":"Lookup","req":false},{"s":"History__c","t":"Burofax__c","f":"Burofax__c","r":"Lookup","req":false},{"s":"History__c","t":"User","f":"ChangedBy__c","r":"Lookup","req":false},{"s":"History__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"History__c","t":"DebtSettlement__c","f":"Debtsettlement__c","r":"Lookup","req":false},{"s":"History__c","t":"Lawsuit__c","f":"Lawsuit__c","r":"Lookup","req":false},{"s":"History__c","t":"Lead","f":"Lead__c","r":"Lookup","req":false},{"s":"History__c","t":"Opportunity","f":"Opportunity__c","r":"Lookup","req":false},{"s":"History__c","t":"Process__c","f":"Process__c","r":"Lookup","req":false},{"s":"History__c","t":"Saving__c","f":"Saving__c","r":"Lookup","req":false},{"s":"History__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"IBAN__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"LSODocument__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"LSODocument__c","t":"User","f":"AttachedBy__c","r":"Lookup","req":false},{"s":"LSODocument__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"LawsuitLog__c","t":"Lawsuit__c","f":"Lawsuit__c","r":"MasterDetail","req":true},{"s":"Lawsuit__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"User","f":"AssignedTo__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"Burofax__c","f":"Burofax__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"User","f":"ClaimManager__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"User","f":"MadeBy__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"Entity__c","f":"ReportingEntity__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"User","f":"ReviewedBy__c","r":"Lookup","req":false},{"s":"Lawsuit__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"Lead","t":"User","f":"Collaborator__c","r":"Lookup","req":false},{"s":"Lead","t":"Lead","f":"MasterLead__c","r":"Lookup","req":false},{"s":"Lead","t":"Entity__c","f":"MortgageBankLook__c","r":"Lookup","req":false},{"s":"Lead","t":"User","f":"OwnerOppDuplicateLead__c","r":"Lookup","req":false},{"s":"Lead","t":"Entity__c","f":"PayrollBank__c","r":"Lookup","req":false},{"s":"Notification__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Notification__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"Opportunity","t":"Account","f":"AssociatedAccount__c","r":"Lookup","req":false},{"s":"Opportunity","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"PaymentGatewayInstallment__c","t":"Saving__c","f":"Saving__c","r":"Lookup","req":false},{"s":"PaymentGatewayInstallment__c","t":"Service__c","f":"Service__c","r":"MasterDetail","req":true},{"s":"PaymentGatewayInstallment__c","t":"PaymentGatewayToken__c","f":"UsedToken__c","r":"Lookup","req":false},{"s":"PaymentGatewayToken__c","t":"Account","f":"Account__c","r":"MasterDetail","req":true},{"s":"PowerAttorney__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Process__c","t":"Account","f":"RelatedAccount__c","r":"Lookup","req":false},{"s":"Process__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"Quote","t":"Opportunity","f":"AddendumOpportunity__c","r":"Lookup","req":false},{"s":"Quote","t":"Quote","f":"PreviousLiquidationPlan__c","r":"Lookup","req":false},{"s":"Quote","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"SalesDebt__c","t":"Opportunity","f":"AddendumOpportunity__c","r":"Lookup","req":false},{"s":"SalesDebt__c","t":"Contract","f":"Contract__c","r":"Lookup","req":false},{"s":"SalesDebt__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"SalesDebt__c","t":"Opportunity","f":"Opportunity__c","r":"Lookup","req":false},{"s":"SalesDebt__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"Saving__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Saving__c","t":"BankHolded__c","f":"BankHolded__c","r":"Lookup","req":false},{"s":"Saving__c","t":"DebtSettlement__c","f":"DebtSettlement__c","r":"Lookup","req":false},{"s":"Saving__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"Saving__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"Saving__c","t":"Lawsuit__c","f":"Lawsuit__c","r":"Lookup","req":false},{"s":"Saving__c","t":"Service__c","f":"Reconversion_service__c","r":"Lookup","req":false},{"s":"Saving__c","t":"Service__c","f":"Service__c","r":"MasterDetail","req":true},{"s":"Saving__c","t":"TicketHolded__c","f":"TicketHolded__c","r":"Lookup","req":false},{"s":"Service__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Service__c","t":"User","f":"AssignedSeniorManager__c","r":"Lookup","req":false},{"s":"Service__c","t":"Account","f":"AssociatedAccount__c","r":"Lookup","req":false},{"s":"Service__c","t":"Contract","f":"Contract__c","r":"Lookup","req":false},{"s":"Service__c","t":"Service__c","f":"ConversionService__c","r":"Lookup","req":false},{"s":"Service__c","t":"User","f":"ManagerAssigned__c","r":"Lookup","req":false},{"s":"Service__c","t":"PaymentGatewayToken__c","f":"PaymentGatewayToken__c","r":"Lookup","req":false},{"s":"Service__c","t":"Quote","f":"Quote__c","r":"Lookup","req":false},{"s":"SettlementInstallment__c","t":"DebtSettlement__c","f":"DebtSettlement__c","r":"MasterDetail","req":true},{"s":"SettlementInstallment__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"SettlementPayment__c","t":"DebtSettlement__c","f":"DebtSettlement__c","r":"MasterDetail","req":true},{"s":"SettlementPayment__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"SettlementPayment__c","t":"SettlementInstallment__c","f":"SettlementInstallment__c","r":"MasterDetail","req":true},{"s":"TicketHolded__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"TicketHolded__c","t":"Contract","f":"Contract__c","r":"Lookup","req":false},{"s":"TicketHolded__c","t":"Service__c","f":"Service__c","r":"MasterDetail","req":true},{"s":"Transfer__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Transfer__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"Vendor_Payment__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"Vendor_Payment__c","t":"Bankruptcy__c","f":"Bankruptcy__c","r":"Lookup","req":false},{"s":"Vendor_Payment__c","t":"Saving__c","f":"Saving__c","r":"Lookup","req":false},{"s":"Vendor_Payment__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"VoiceCall","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"Account","f":"Account__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"Bankruptcy__c","f":"Bankruptcy__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"Debt__c","f":"Debt__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"DebtSettlement__c","f":"Debtsettlement__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"Lawsuit__c","f":"Lawsuit__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"Lead","f":"Lead__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"Opportunity","f":"Opportunity__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"Service__c","f":"Service__c","r":"Lookup","req":false},{"s":"VoiceCall_Link__c","t":"VoiceCall","f":"Voice_Call__c","r":"MasterDetail","req":true},{"s":"signaturit__SignatureRequestFile__c","t":"signaturit__SignatureRequest__c","f":"signaturit__SignatureRequest__c","r":"MasterDetail","req":true},{"s":"signaturit__SignatureRequestSigner__c","t":"Account","f":"signaturit__Account__c","r":"Lookup","req":false},{"s":"signaturit__SignatureRequestSigner__c","t":"Lead","f":"signaturit__Lead__c","r":"Lookup","req":false},{"s":"signaturit__SignatureRequestSigner__c","t":"signaturit__SignatureRequest__c","f":"signaturit__SignatureRequest__c","r":"MasterDetail","req":true},{"s":"signaturit__SignatureRequestSigner__c","t":"User","f":"signaturit__User__c","r":"Lookup","req":false},{"s":"signaturit__SignatureRequest__c","t":"Entity__c","f":"Entity__c","r":"Lookup","req":false},{"s":"Opportunity","t":"Account","f":"AccountId","r":"Lookup","req":false},{"s":"Opportunity","t":"Campaign","f":"CampaignId","r":"Lookup","req":false},{"s":"Opportunity","t":"Pricebook2","f":"Pricebook2Id","r":"Lookup","req":false},{"s":"Quote","t":"Opportunity","f":"OpportunityId","r":"Lookup","req":false},{"s":"Contract","t":"Account","f":"AccountId","r":"Lookup","req":true},{"s":"Contract","t":"Pricebook2","f":"Pricebook2Id","r":"Lookup","req":false},{"s":"Case","t":"Account","f":"AccountId","r":"Lookup","req":false},{"s":"Case","t":"Opportunity","f":"OpportunityId","r":"Lookup","req":false},{"s":"CampaignMember","t":"Campaign","f":"CampaignId","r":"MasterDetail","req":true},{"s":"CampaignMember","t":"Lead","f":"LeadId","r":"Lookup","req":false},{"s":"Lead","t":"Account","f":"ConvertedAccountId","r":"Lookup","req":false},{"s":"Lead","t":"Opportunity","f":"ConvertedOpportunityId","r":"Lookup","req":false},{"s":"PricebookEntry","t":"Pricebook2","f":"Pricebook2Id","r":"MasterDetail","req":true},{"s":"PricebookEntry","t":"Product2","f":"Product2Id","r":"MasterDetail","req":true},{"s":"ContentVersion","t":"ContentDocument","f":"ContentDocumentId","r":"MasterDetail","req":true},{"s":"EmailMessage","t":"Opportunity","f":"RelatedToId","r":"Lookup","req":false},{"s":"VoiceCall","t":"User","f":"OwnerId","r":"Lookup","req":false},{"s":"Account","t":"Account","f":"ParentId","r":"Lookup","req":false},{"s":"PersonAccount","t":"Account","f":"AccountId","r":"Lookup","req":true}]},"details":{"AWS_Connect_Settings__mdt":{"label":"AWS Connect Settings","cat":"metadata","fieldCount":5,"fields":[{"name":"Access_Key_Id__c","label":"Access Key Id","type":"Text"},{"name":"Connect_Instance_Id__c","label":"Connect Instance Id","type":"Text"},{"name":"Region__c","label":"Region","type":"Text"},{"name":"Role_ARN__c","label":"Role ARN","type":"Text"},{"name":"Secret_Access_Key__c","label":"Secret Access Key","type":"Text"}],"relationships":[],"incoming":[]},"Account":{"label":"Account (Cliente)","cat":"standard","fieldCount":45,"fields":[{"name":"AddresCompany__c","label":"Dirección empresa","type":"Text"},{"name":"Age__c","label":"Edad","type":"Number"},{"name":"Assigned_document_manager__c","label":"Gestor documentación asignado","type":"Lookup"},{"name":"CIF__c","label":"CIF","type":"Text"},{"name":"Charge__c","label":"Cargo","type":"Text"},{"name":"CompanyName__c","label":"Nombre empresa","type":"Text"},{"name":"DNI__c","label":"DNI","type":"Text"},{"name":"DNI_expiration_date__c","label":"Fecha de Caducidad del DNI","type":"Date"},{"name":"DateEmploymentStatus__c","label":"Fecha de la situación laboral","type":"Date"},{"name":"DependentPeople__c","label":"Personas dependientes","type":"Number"},{"name":"Difficult_customer__c","label":"Cliente conflictivo","type":"Checkbox"},{"name":"EnrollmentDate__c","label":"Enrollment Date","type":"Date"},{"name":"Entity__c","label":"Entidad","type":"Lookup"},{"name":"ForAddress__c","label":"Dirección completa","type":"Text"},{"name":"FullId__c","label":"IDCompleto","type":"Text"},{"name":"HaveSocioeconomicCrimes__c","label":"¿Delitos Socioeconómicos?","type":"Checkbox"},{"name":"HoldedId__c","label":"Id de Holded","type":"Text"},{"name":"Housing_situation__c","label":"Situación vivienda","type":"Picklist"},{"name":"JobPosition__c","label":"Puesto de trabajo","type":"Text"},{"name":"KmaleonCode__c","label":"Código Kmaleon","type":"Number"},{"name":"ManagerChangeDate__c","label":"Fecha cambio gestor","type":"DateTime"},{"name":"MaritalRegime__c","label":"Régimen Matrimonial","type":"Picklist"},{"name":"MaritalStatus__c","label":"Estado Civil","type":"Picklist"},{"name":"MonthlyRentalHabitualResidence__c","label":"Mensualidad Alquiler Vivienda Habitual","type":"Currency"},{"name":"MortgageBankLook__c","label":"Banco Hipoteca","type":"Lookup"},{"name":"Mortgage_date__c","label":"Fecha hipoteca","type":"Date"},{"name":"Nationality__c","label":"Nacionalidad","type":"Text"},{"name":"NetMonthlySalary__c","label":"Salario Neto Mensual","type":"Currency"},{"name":"NetValueOtherRealProperties__c","label":"Valor Neto Otros Inmuebles","type":"Currency"},{"name":"NetValueVehicles__c","label":"Valor Neto Vehículos","type":"Currency"}],"relationships":[{"field":"Assigned_document_manager__c","relType":"Lookup","referenceTo":"User","relationshipName":"Accounts","required":false},{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"AccountEntity","required":false},{"field":"MortgageBankLook__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"AccountMortgageBankLookEntity","required":false},{"field":"PayrollBank__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"AccountPayrollBankEntity","required":false},{"field":"RecoveryEntity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"RecoveryEntities","required":false},{"field":"ParentId","relType":"Lookup","referenceTo":"Account","relationshipName":null,"required":false}],"incoming":["AccountInfo__c","AccountRelationship__c","Bankruptcy__c","Billing__c","Burofax__c","Client_Profile_Change__c","ContractAccount__c","CreditDefaultFile__c","DebtHolder__c","DebtSettlement__c","Debt__c","Good__c","History__c","LSODocument__c","Lawsuit__c","Notification__c","Opportunity","PaymentGatewayToken__c","PowerAttorney__c","Process__c","Saving__c","Service__c","TicketHolded__c","Transfer__c","Vendor_Payment__c","VoiceCall","VoiceCall_Link__c","signaturit__SignatureRequestSigner__c","Contract","Case","Lead","Account","PersonAccount"]},"AccountInfo__c":{"label":"AccountInfo","cat":"custom","fieldCount":137,"fields":[{"name":"AGTConceptPenalty__c","label":"Concepto Sanción AGT","type":"TextArea"},{"name":"AGTDebtAmount__c","label":"Importe deuda AGT","type":"Currency"},{"name":"AGTDebt__c","label":"Deuda AGT","type":"Checkbox"},{"name":"AGTPenalty__c","label":"Sanción AGT","type":"Checkbox"},{"name":"AcceptanceDateIndemnity__c","label":"Fecha aceptación indemnización","type":"Date"},{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"AmountCompensated__c","label":"Cuantía indemnizada","type":"Currency"},{"name":"AmountDebtCCPP__c","label":"Importe deuda CCPP","type":"Currency"},{"name":"AmountPension__c","label":"Cuantía pensión o salario social","type":"Currency"},{"name":"AnnualPaymentsNumber__c","label":"Pagas anuales","type":"Picklist"},{"name":"BankAccountBalance__c","label":"Saldo de la cuenta bancaria","type":"Currency"},{"name":"BankCard__c","label":"Tarjeta Bancaria","type":"Number"},{"name":"BankEntityIBAN__c","label":"IBAN de la Entidad bancaria","type":"Text"},{"name":"BankEntityName__c","label":"Nombre de la Entidad bancaria","type":"Text"},{"name":"BirthCertificateBook__c","label":"Libro partida nacimiento","type":"Text"},{"name":"BirthCertificateNumber__c","label":"Número partida nacimiento","type":"Text"},{"name":"BirthCertificatePage__c","label":"Página partida nacimiento","type":"Text"},{"name":"BirthCertificateVolume__c","label":"Tomo partida nacimiento","type":"Text"},{"name":"BirthProvinceAndMunicipality__c","label":"Provincia y municipio nacimiento","type":"LongTextArea"},{"name":"Capitulations__c","label":"Capitulaciones","type":"Checkbox"},{"name":"CommunityRegistrationPH__c","label":"Comunidad autónoma registro PH","type":"Picklist"},{"name":"CompanyName__c","label":"Nombre empresa","type":"Text"},{"name":"CompanyRegisteredAddress__c","label":"Domicilio social de la Empresa","type":"Text"},{"name":"ConceptIndemnity__c","label":"Concepto indemnización","type":"TextArea"},{"name":"ConceptPension__c","label":"Concepto pensión o salario social","type":"TextArea"},{"name":"CountryOfBirth__c","label":"País de nacimiento","type":"Picklist"},{"name":"DateDivorceDecree__c","label":"Fecha sentencia divorcio","type":"Date"},{"name":"DateSentence__c","label":"Fecha resolución","type":"Date"},{"name":"DebtCCPP__c","label":"Deuda CCPP","type":"Checkbox"},{"name":"DependencyType__c","label":"Tipo de dependencia","type":"Picklist"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Informaciones_del_Cliente","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Informaciones_del_Cliente","required":false}],"incoming":["Client_Profile_Change__c"]},"AccountRelationship__c":{"label":"AccountRelationship","cat":"custom","fieldCount":7,"fields":[{"name":"AccountFrom__c","label":"Cuenta de origen","type":"Lookup"},{"name":"AccountTo__c","label":"Cuenta de destino","type":"Lookup"},{"name":"EndDate__c","label":"Fecha de fin","type":"Date"},{"name":"InverseAccountRelationship__c","label":"Cuenta relacionada inversa","type":"Lookup"},{"name":"IsInverse__c","label":"¿Es inversa?","type":"Checkbox"},{"name":"RelationshipType__c","label":"Tipo de relación","type":"Picklist"},{"name":"StartDate__c","label":"Fecha de inicio","type":"Date"}],"relationships":[{"field":"AccountFrom__c","relType":"Lookup","referenceTo":"Account","relationshipName":"AccountsFrom","required":false},{"field":"AccountTo__c","relType":"Lookup","referenceTo":"Account","relationshipName":"AccountsTo","required":false},{"field":"InverseAccountRelationship__c","relType":"Lookup","referenceTo":"AccountRelationship__c","relationshipName":"InverseAccountRelationships","required":false}],"incoming":["AccountRelationship__c"]},"Activity":{"label":"Activity","cat":"standard","fieldCount":5,"fields":[{"name":"Detailed_call_type__c","label":"Detailed call type","type":"Picklist"},{"name":"ExternalReferenceId__c","label":"External Reference Id","type":"Text"},{"name":"Missed_Call_Reason__c","label":"Missed Call Reason","type":"Picklist"},{"name":"StatusClientAppointment__c","label":"Cita Cliente Estado","type":"Picklist"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[],"incoming":[]},"BankCIF__mdt":{"label":"BankCIF","cat":"metadata","fieldCount":3,"fields":[{"name":"CIF__c","label":"CIF","type":"Text"},{"name":"Postal__c","label":"Postal","type":"Checkbox"},{"name":"SpecialFund__c","label":"Fondo Especial","type":"Checkbox"}],"relationships":[],"incoming":[]},"BankHolded__c":{"label":"BankHolded","cat":"custom","fieldCount":2,"fields":[{"name":"HoldedId__c","label":"Id de Holded","type":"Text"},{"name":"IBAN__c","label":"IBAN","type":"EncryptedText"}],"relationships":[],"incoming":["Saving__c"]},"Bankruptcy__c":{"label":"Bankruptcy","cat":"custom","fieldCount":63,"fields":[{"name":"AccountPartner__c","label":"Pareja Cliente","type":"Lookup"},{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"AssignedTo__c","label":"Asignado a","type":"Lookup"},{"name":"AssignmentDatePartner__c","label":"Fecha de asignación Pareja","type":"Date"},{"name":"AssignmentDate__c","label":"Fecha de asignación","type":"Date"},{"name":"BOEPublicationDatePartner__c","label":"Fecha de Publicación Boe Pareja","type":"Date"},{"name":"BOEPublicationDate__c","label":"Fecha de Publicación Boe","type":"Date"},{"name":"BankruptcyProcess__c","label":"Proceso del concurso","type":"Picklist"},{"name":"BankruptcyType__c","label":"Tipo de concurso","type":"Picklist"},{"name":"Bankruptcy__c","label":"Concurso","type":"Picklist"},{"name":"CommunicationEPIDate__c","label":"Fecha comunicación EPi","type":"Date"},{"name":"CorrectionAppliedPartner__c","label":"Aplicada Corrección Pareja","type":"Date"},{"name":"CorrectionApplied__c","label":"Aplicada Corrección","type":"Date"},{"name":"CourtPartner__c","label":"Juzgado Pareja","type":"Text"},{"name":"Court__c","label":"Juzgado","type":"Text"},{"name":"CreatedDateKmaleon__c","label":"Fecha creación Kmaleon","type":"Date"},{"name":"DateFact__c","label":"Fecha Hecho","type":"Date"},{"name":"DeclarationDatePartner__c","label":"Fecha de declaración Pareja","type":"Date"},{"name":"DeclarationDate__c","label":"Fecha de declaración","type":"Date"},{"name":"EPICreditorCommunicationDate__c","label":"Fecha Comunicación EPI al Acreedor","type":"Date"},{"name":"EPIDatePartner__c","label":"Fecha EPI Pareja","type":"Date"},{"name":"EPIDate__c","label":"Fecha EPI","type":"Date"},{"name":"EPIDueDatePartner__c","label":"Fecha vencimiento EPI Pareja","type":"Date"},{"name":"EPIDueDate__c","label":"Fecha vencimiento EPI","type":"Date"},{"name":"EPISubmissionPartner__c","label":"Presentación EPI Pareja","type":"Date"},{"name":"EPISubmission__c","label":"Presentación EPI","type":"Date"},{"name":"Exonerated__c","label":"Exonerado","type":"Checkbox"},{"name":"HaltedRequestDate__c","label":"Fecha de Solicitud paralizada","type":"Date"},{"name":"InternalCaseNumberPartner__c","label":"Nº Expediente Interno Pareja","type":"Text"},{"name":"InternalCaseNumber__c","label":"Nº Expediente Interno","type":"Text"}],"relationships":[{"field":"AccountPartner__c","relType":"Lookup","referenceTo":"Account","relationshipName":"BankruptciesPartner","required":false},{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Bankruptcies","required":false},{"field":"AssignedTo__c","relType":"Lookup","referenceTo":"User","relationshipName":"Demandas2","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Bankruptcies","required":false}],"incoming":["History__c","Vendor_Payment__c","VoiceCall_Link__c"]},"Batch_settings__mdt":{"label":"Batch settings","cat":"metadata","fieldCount":8,"fields":[{"name":"Batch_name__c","label":"Batch name","type":"Text"},{"name":"Batch_size__c","label":"Batch size","type":"Number"},{"name":"Email_notification__c","label":"Email notification","type":"Checkbox"},{"name":"End_Date__c","label":"End Date","type":"Date"},{"name":"Interval_Days__c","label":"Interval (days)","type":"Number"},{"name":"Interval_Minutes__c","label":"Interval (minutes)","type":"Number"},{"name":"Max_retries__c","label":"Max retries","type":"Number"},{"name":"Query__c","label":"Query","type":"LongTextArea"}],"relationships":[],"incoming":[]},"Billing__c":{"label":"Billing","cat":"custom","fieldCount":12,"fields":[{"name":"Account__c","label":"Cuenta","type":"Lookup"},{"name":"Amount__c","label":"Importe","type":"Currency"},{"name":"BillingType__c","label":"Tipo facturación","type":"Picklist"},{"name":"CorrectedInvoiceNumber__c","label":"Nº de Factura rectificadas","type":"Text"},{"name":"IVA__c","label":"IVA","type":"Currency"},{"name":"InvoiceNumer__c","label":"Nº de factura","type":"Text"},{"name":"InvoicedDate__c","label":"Fecha de facturación","type":"Date"},{"name":"Invoiced__c","label":"Facturado","type":"Checkbox"},{"name":"Saving__c","label":"Ahorro","type":"Lookup"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"TaxBase__c","label":"Base imponible","type":"Currency"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Facturaciones","required":false},{"field":"Saving__c","relType":"Lookup","referenceTo":"Saving__c","relationshipName":"Facturaciones","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Facturaciones","required":false}],"incoming":[]},"Burofax__c":{"label":"Burofax","cat":"custom","fieldCount":54,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"AcknowledgementOfReceipt__c","label":"Acuse de recibo","type":"Checkbox"},{"name":"BurofaxNotificadosReference__c","label":"Referencia Burofax notificados","type":"Text"},{"name":"Burofax_Attempt_Count__c","label":"Nº Intentos burofax","type":"Number"},{"name":"Burofax_Reference_Date__c","label":"Fecha Referencia Burofax","type":"DateTime"},{"name":"Burofax_Sent_Date__c","label":"Fecha Envío Burofax","type":"Date"},{"name":"Burofax_Status__c","label":"Estado Burofax","type":"Picklist"},{"name":"Burofax_Type__c","label":"Tipo","type":"Picklist"},{"name":"CertifiedCopyNotarialTestimony__c","label":"Copia certificada / Testimonio notarial","type":"Checkbox"},{"name":"Correction_Date__c","label":"Fecha Subsanación","type":"DateTime"},{"name":"Correction_Type__c","label":"Tipo de Subsanación","type":"Picklist"},{"name":"Date__c","label":"Date","type":"Date"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Early_Maturity__c","label":"Vencimiento anticipado","type":"Checkbox"},{"name":"Early_Termination_Penalty__c","label":"Penalización por vencimiento anticipado","type":"Checkbox"},{"name":"End_Date__c","label":"Fecha Finalizado","type":"DateTime"},{"name":"End_Type__c","label":"Tipo Finalizado","type":"Picklist"},{"name":"Entity__c","label":"Entidad","type":"Lookup"},{"name":"Error__c","label":"Error","type":"TextArea"},{"name":"Expiration_Date__c","label":"Fecha Expiración","type":"DateTime"},{"name":"FullId__c","label":"IDCompleto","type":"Text"},{"name":"IdEnvio__c","label":"Id envio","type":"Text"},{"name":"Late_Payment_Interest__c","label":"Interés de demora","type":"Checkbox"},{"name":"NonPayment_Clause__c","label":"Cláusula de impago","type":"Checkbox"},{"name":"NotificadosBurofaxReference__c","label":"Referencia Burofax Notificados","type":"Text"},{"name":"Origination_Fee__c","label":"Comisión de apertura","type":"Checkbox"},{"name":"Payment_Protection_Insurance__c","label":"Seguro de protección de pagos","type":"Checkbox"},{"name":"Previous_Status__c","label":"Estado anterior","type":"Picklist"},{"name":"Processed__c","label":"Procesado","type":"Checkbox"},{"name":"ProductEntity__c","label":"Producto Entidad Bancaria","type":"Lookup"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Burofaxes","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Burofaxes","required":false},{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Burofaxes","required":false},{"field":"ProductEntity__c","relType":"Lookup","referenceTo":"EntityProduct__c","relationshipName":"Burofaxes","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Burofaxes","required":false}],"incoming":["CertifiedEmail__c","History__c","Lawsuit__c"]},"Campaign":{"label":"Campaign","cat":"standard","fieldCount":12,"fields":[{"name":"CampaignId__c","label":"Identificador de Campaña","type":"Text"},{"name":"CampaignMemberRecordTypeId","label":"CampaignMemberRecordTypeId","type":"Lookup"},{"name":"CatchmentBranding__c","label":"Marca de captación","type":"Picklist"},{"name":"CatchmentCampaign__c","label":"Campaña de captación","type":"TextArea"},{"name":"CatchmentMode__c","label":"Modalidad de captación","type":"Picklist"},{"name":"CatchmentSource__c","label":"Fuente de captación","type":"Picklist"},{"name":"EntryType__c","label":"Tipo de entrada","type":"Picklist"},{"name":"LeadOrigin__c","label":"Origen del lead","type":"Picklist"},{"name":"OwnerId","label":"OwnerId","type":"Lookup"},{"name":"ParentId","label":"ParentId","type":"Lookup"},{"name":"Status","label":"Status","type":"Picklist"},{"name":"Type","label":"Type","type":"Picklist"}],"relationships":[],"incoming":["Opportunity","CampaignMember"]},"CampaignMember":{"label":"Campaign Member","cat":"standard","fieldCount":19,"fields":[{"name":"CampaignId","label":"CampaignId","type":"Lookup"},{"name":"ClosingReasonLead__c","label":"Motivo de Cierre Lead","type":"Text"},{"name":"ContactId","label":"ContactId","type":"Lookup"},{"name":"GC_SCREEN_POP__c","label":"GC_SCREEN_POP","type":"Text"},{"name":"LeadId","label":"LeadId","type":"Lookup"},{"name":"LeadSource","label":"LeadSource","type":"Picklist"},{"name":"LossReasonLead__c","label":"Motivo Pérdida Lead","type":"Text"},{"name":"Numero_Intentos__c","label":"Numero Intentos","type":"Number"},{"name":"OwnerAgent__c","label":"Agente Propietario","type":"Text"},{"name":"Salutation","label":"Salutation","type":"Picklist"},{"name":"Status","label":"Status","type":"Picklist"},{"name":"genesyscloud__Genesys_Cloud_Contact_Priority__c","label":"Genesys Cloud Contact Priority","type":"Checkbox"},{"name":"genesyscloud__Last_Attempt__c","label":"Last Attempt","type":"DateTime"},{"name":"genesyscloud__Last_Result__c","label":"Last Result","type":"Text"},{"name":"genesyscloud__Last_Sync__c","label":"Last Sync","type":"DateTime"},{"name":"genesyscloud__Reconciled_Result__c","label":"Reconciled Result","type":"Checkbox"},{"name":"genesyscloud__Sort_Order__c","label":"Sort Order","type":"Text"},{"name":"genesyscloud__Synced_to_Genesys_Cloud__c","label":"Synced to Genesys Cloud","type":"Checkbox"},{"name":"genesyscloud__Time_Zone__c","label":"Time Zone","type":"Picklist"}],"relationships":[{"field":"CampaignId","relType":"MasterDetail","referenceTo":"Campaign","relationshipName":null,"required":true},{"field":"LeadId","relType":"Lookup","referenceTo":"Lead","relationshipName":null,"required":false}],"incoming":[]},"Case":{"label":"Case","cat":"standard","fieldCount":35,"fields":[{"name":"AccountId","label":"AccountId","type":"Lookup"},{"name":"Area__c","label":"Área","type":"Picklist"},{"name":"AssetId","label":"AssetId","type":"Lookup"},{"name":"BusinessHoursId","label":"BusinessHoursId","type":"Lookup"},{"name":"ClosingComments__c","label":"Comentarios cierre","type":"TextArea"},{"name":"ContactId","label":"ContactId","type":"Lookup"},{"name":"DebtCasePriority__c","label":"Prioridad de caso por deuda","type":"Text"},{"name":"DebtCaseReason__c","label":"Motivo de casos de deuda","type":"Lookup"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"DestinationDepartmentName__c","label":"Departamento destino","type":"Text"},{"name":"DestinationDepartment__c","label":"Departamento destino de caso por deudas","type":"Lookup"},{"name":"DueDate__c","label":"Vencimiento","type":"Date"},{"name":"Email_Harassment__c","label":"Correo acoso","type":"Checkbox"},{"name":"EntitlementId","label":"EntitlementId","type":"Lookup"},{"name":"ExpirationDate__c","label":"Fecha vencimiento","type":"Date"},{"name":"FullId__c","label":"IDCompleto","type":"Text"},{"name":"LossReason__c","label":"Motivo de pérdida","type":"Picklist"},{"name":"Origin","label":"Origin","type":"Picklist"},{"name":"OriginDepartmentName__c","label":"Departamento origen","type":"Text"},{"name":"OriginDepartment__c","label":"Departamento solicitantde de caso","type":"Lookup"},{"name":"OwnerId","label":"OwnerId","type":"Lookup"},{"name":"ParentId","label":"ParentId","type":"Lookup"},{"name":"Priority","label":"Priority","type":"Picklist"},{"name":"ProductId","label":"ProductId","type":"Lookup"},{"name":"RFT__c","label":"RFT","type":"Picklist"},{"name":"Reason","label":"Reason","type":"Picklist"},{"name":"RequestedBy__c","label":"Solicitado por","type":"Lookup"},{"name":"RequestingDepartment__c","label":"Departamento Solicitante","type":"Picklist"},{"name":"ServiceContractId","label":"ServiceContractId","type":"Lookup"},{"name":"SoftwareDescription__c","label":"Descripcion del software","type":"LongTextArea"}],"relationships":[{"field":"DebtCaseReason__c","relType":"Lookup","referenceTo":"DebtCaseReasonCatalog__c","relationshipName":"Cases","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Cases","required":false},{"field":"DestinationDepartment__c","relType":"Lookup","referenceTo":"DebtCaseDepartment__c","relationshipName":"Cases","required":false},{"field":"OriginDepartment__c","relType":"Lookup","referenceTo":"DebtCaseDepartment__c","relationshipName":"Cases1","required":false},{"field":"RequestedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"RequestedCases","required":false},{"field":"AccountId","relType":"Lookup","referenceTo":"Account","relationshipName":null,"required":false},{"field":"OpportunityId","relType":"Lookup","referenceTo":"Opportunity","relationshipName":null,"required":false}],"incoming":[]},"CatchmentSourceEquivalence__c":{"label":"CatchmentSourceEquivalence","cat":"custom","fieldCount":6,"fields":[{"name":"Channel__c","label":"Canal","type":"TextArea"},{"name":"Source__c","label":"Fuente","type":"TextArea"},{"name":"UTMCampaign__c","label":"UTM Campaign","type":"TextArea"},{"name":"UTMMedium__c","label":"UTM Medium","type":"TextArea"},{"name":"UTMSource__c","label":"UTM Source","type":"TextArea"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[],"incoming":[]},"CertifiedEmailFiles__c":{"label":"CertifiedEmailFiles","cat":"custom","fieldCount":4,"fields":[{"name":"CertifiedEmail__c","label":"Email certificado","type":"Lookup"},{"name":"Signaturit_Id__c","label":"Signaturit Id","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"},{"name":"signaturit_Status__c","label":"Estado Signaturit","type":"Picklist"}],"relationships":[{"field":"CertifiedEmail__c","relType":"Lookup","referenceTo":"CertifiedEmail__c","relationshipName":"Archivos_emails_certificados","required":false}],"incoming":[]},"CertifiedEmail__c":{"label":"CertifiedEmail","cat":"custom","fieldCount":6,"fields":[{"name":"Audit_Trail_Downloaded__c","label":"Audit Trail descargado","type":"Checkbox"},{"name":"Burofax__c","label":"Burofax","type":"Lookup"},{"name":"Entity_name__c","label":"Entidad","type":"Text"},{"name":"Signaturit_Id__c","label":"Signaturit Id","type":"Text"},{"name":"Signaturit_status__c","label":"Estado Signaturit","type":"Picklist"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Burofax__c","relType":"Lookup","referenceTo":"Burofax__c","relationshipName":"Emails_certificados","required":false}],"incoming":["CertifiedEmailFiles__c"]},"Client_Profile_Change__c":{"label":"Client Profile Change","cat":"custom","fieldCount":8,"fields":[{"name":"AccountInfo__c","label":"Información del Cliente","type":"Lookup"},{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"Field_Name__c","label":"Nombre Campo","type":"Text"},{"name":"New_Value__c","label":"Nuevo valor del campo","type":"LongTextArea"},{"name":"Old_Value__c","label":"Valor anterior del campo","type":"LongTextArea"},{"name":"Pending_validation__c","label":"Pendiente de validar","type":"Checkbox"},{"name":"ReasonRejection__c","label":"Motivo del rechazo","type":"LongTextArea"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"AccountInfo__c","relType":"Lookup","referenceTo":"AccountInfo__c","relationshipName":"Cambios_de_perfil_de_comunidad","required":false},{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Cambios_de_perfil_de_comunidad","required":false}],"incoming":[]},"CommunityGate__mdt":{"label":"CommunityGate","cat":"metadata","fieldCount":2,"fields":[{"name":"IsMaintenance__c","label":"Manteniendo","type":"Checkbox"},{"name":"Message__c","label":"Mensaje","type":"Text"}],"relationships":[],"incoming":[]},"Configuracion_Slots_Disponibles__mdt":{"label":"Configuracion Slots Disponibles","cat":"metadata","fieldCount":7,"fields":[{"name":"Activo__c","label":"Activo","type":"Checkbox"},{"name":"Duracion_Reunion_Minutos__c","label":"Duracion_Reunion_Minutos","type":"Number"},{"name":"Grupo_Publico__c","label":"Grupo_Publico","type":"Text"},{"name":"Hora_Fin__c","label":"Hora_Fin","type":"Text"},{"name":"Hora_Inicio__c","label":"Hora_Inicio","type":"Text"},{"name":"Intervalo_Minutos__c","label":"Intervalo_Minutos","type":"Number"},{"name":"Rol_Asesor__c","label":"Rol_Asesor","type":"Text"}],"relationships":[],"incoming":[]},"ContentDocument":{"label":"ContentDocument","cat":"standard","fieldCount":0,"fields":[],"relationships":[],"incoming":["ContentVersion"]},"ContentVersion":{"label":"ContentVersion","cat":"standard","fieldCount":29,"fields":[{"name":"Agreement_Type__c","label":"Tipo de acuerdo","type":"Picklist"},{"name":"AuditTrail__c","label":"AuditTrail","type":"Checkbox"},{"name":"Automatic_Response_Type__c","label":"Tipo de respuesta automatica","type":"Picklist"},{"name":"Claim_Admitted__c","label":"Pretensión allanada","type":"MultiselectPicklist"},{"name":"Communication_Source__c","label":"Origen comunicación","type":"Picklist"},{"name":"Contract_Resolution_Type__c","label":"Tipo de resolución contrato","type":"Picklist"},{"name":"Contract_Type__c","label":"Tipo de contrato","type":"Picklist"},{"name":"Contractual_Document_Type__c","label":"Tipo de documento contractual","type":"Text"},{"name":"Correction_Type__c","label":"Tipo subsanación","type":"Picklist"},{"name":"Discrepancy_Reason__c","label":"Motivo discrepancia","type":"Picklist"},{"name":"DocumentationDate__c","label":"Fecha Recepción Documento","type":"DateTime"},{"name":"Entity__c","label":"Entidad","type":"Lookup"},{"name":"File_Type__c","label":"Tipo de archivo","type":"Picklist"},{"name":"OldId__c","label":"oldId","type":"Text"},{"name":"OwnerId","label":"OwnerId","type":"Lookup"},{"name":"Received_By__c","label":"Quien lo ha recibido","type":"Picklist"},{"name":"Resolution_Type__c","label":"Tipo de resolución","type":"Picklist"},{"name":"Response_Type__c","label":"Tipo de contestación","type":"Picklist"},{"name":"Rude_Response_Type__c","label":"Tipo de respuesta burda","type":"Picklist"},{"name":"SignaturitType__c","label":"Tipo Signaturit","type":"Picklist"},{"name":"SignedContract__c","label":"Contrato firmado","type":"Checkbox"},{"name":"Statement_Type__c","label":"Tipo de extracto","type":"Picklist"},{"name":"Status__c","label":"Status","type":"Picklist"},{"name":"Subtype__c","label":"Subtipo","type":"Picklist"},{"name":"To_Be_Paid__c","label":"A pagar","type":"Currency"},{"name":"Type__c","label":"Type","type":"Picklist"},{"name":"Upload_Date__c","label":"Fecha Validación Documento","type":"DateTime"},{"name":"ValidatedBy__c","label":"Validado por","type":"Lookup"},{"name":"Validated__c","label":"Validado","type":"Checkbox"}],"relationships":[{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Content_Versions","required":false},{"field":"ValidatedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"Content_Versions","required":false},{"field":"ContentDocumentId","relType":"MasterDetail","referenceTo":"ContentDocument","relationshipName":null,"required":true}],"incoming":[]},"Contract":{"label":"Contract","cat":"standard","fieldCount":17,"fields":[{"name":"AccountId","label":"AccountId","type":"Lookup"},{"name":"ActivatedById","label":"ActivatedById","type":"Lookup"},{"name":"CompanySignedId","label":"CompanySignedId","type":"Lookup"},{"name":"CustomerSignedId","label":"CustomerSignedId","type":"Lookup"},{"name":"DateContractSent__c","label":"Fecha Contrato Enviado","type":"Date"},{"name":"DayDateContribution__c","label":"Día fecha aportación","type":"Date"},{"name":"DebtIdsList__c","label":"Lista Ids Deudas","type":"LongTextArea"},{"name":"IsDivided__c","label":"¿Está dividido?","type":"Checkbox"},{"name":"OfficeContractSigned__c","label":"Contrato firmado en la Oficina","type":"Checkbox"},{"name":"Opportunity__c","label":"Opportunity","type":"Lookup"},{"name":"OwnerId","label":"OwnerId","type":"Lookup"},{"name":"PendingIntegrationHolded__c","label":"Pendiente Integración Holded","type":"Checkbox"},{"name":"Pricebook2Id","label":"Pricebook2Id","type":"Lookup"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"Status","label":"Status","type":"Picklist"},{"name":"Type__c","label":"Tipo","type":"Picklist"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Opportunity__c","relType":"Lookup","referenceTo":"Opportunity","relationshipName":"Contracts","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Contracts","required":false},{"field":"AccountId","relType":"Lookup","referenceTo":"Account","relationshipName":null,"required":true},{"field":"Pricebook2Id","relType":"Lookup","referenceTo":"Pricebook2","relationshipName":null,"required":false}],"incoming":["ContractAccount__c","SalesDebt__c","Service__c","TicketHolded__c"]},"ContractAccount__c":{"label":"ContractAccount","cat":"custom","fieldCount":2,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"Contract__c","label":"Contract","type":"Lookup"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"ContractAccounts","required":false},{"field":"Contract__c","relType":"Lookup","referenceTo":"Contract","relationshipName":"ContractAccounts","required":false}],"incoming":[]},"Conversation_History__b":{"label":"Conversation History","cat":"bigobject","fieldCount":8,"fields":[{"name":"AppType__c","label":"AppType","type":"Text"},{"name":"ClientTimestamp__c","label":"ClientTimestamp","type":"DateTime"},{"name":"MessageIdentifier__c","label":"MessageIdentifier","type":"Text"},{"name":"MessageText__c","label":"MessageText","type":"LongTextArea"},{"name":"MessagingSessionId__c","label":"MessagingSessionId","type":"Text"},{"name":"ParentId__c","label":"ParentId","type":"Text"},{"name":"SenderRole__c","label":"SenderRole","type":"Text"},{"name":"SenderSubject__c","label":"SenderSubject","type":"Text"}],"relationships":[],"incoming":[]},"CreditDefaultFileAnalysis__c":{"label":"CreditDefaultFileAnalysis","cat":"custom","fieldCount":17,"fields":[{"name":"AnalysisDate__c","label":"Fecha análisis","type":"DateTime"},{"name":"AnalysisStatus__c","label":"Estado análisis","type":"Picklist"},{"name":"Analyzer__c","label":"Analista","type":"Lookup"},{"name":"CIRBEAmount__c","label":"Importe CIRBE","type":"Currency"},{"name":"CIRBEOperationSituationCode__c","label":"Código situación operación CIRBE","type":"Text"},{"name":"CIRBEOperationTypeCode__c","label":"Código tipo operación CIRBE","type":"Text"},{"name":"CIRBEReportDate__c","label":"Fecha informe CIRBE","type":"Date"},{"name":"CreditDefaultFile__c","label":"Fichero de Morosidad","type":"MasterDetail"},{"name":"DebtAmount__c","label":"Importe deuda","type":"Currency"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"EeportingEntity__c","label":"Entidad informante","type":"Lookup"},{"name":"FileAmount__c","label":"Importe fichero","type":"Currency"},{"name":"IncludedInFile__c","label":"Incluido en el fichero","type":"Checkbox"},{"name":"PreanalysisSituation__c","label":"Situación preanálisis","type":"MultiselectPicklist"},{"name":"RegistrationDate__c","label":"Fecha de alta","type":"Date"},{"name":"SignatoryEntity__c","label":"Entidad informante","type":"Lookup"},{"name":"TypeOfNonOpportunityDebts__c","label":"Tipo deudas fuera de la oportunidad","type":"MultiselectPicklist"}],"relationships":[{"field":"Analyzer__c","relType":"Lookup","referenceTo":"User","relationshipName":"CreditDefaultFileAnalysis","required":false},{"field":"CreditDefaultFile__c","relType":"MasterDetail","referenceTo":"CreditDefaultFile__c","relationshipName":"CreditDefaultFileAnalysis","required":true},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"CreditDefaultFileAnalysis","required":false},{"field":"EeportingEntity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"An_lisis_de_morosidad","required":false},{"field":"SignatoryEntity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Analisis_de_morosidad","required":false}],"incoming":[]},"CreditDefaultFileResult__e":{"label":"CreditDefaultFileResult","cat":"event","fieldCount":3,"fields":[{"name":"ErrorMessage__c","label":"Mensaje de Error","type":"LongTextArea"},{"name":"IsSuccess__c","label":"Es correcto","type":"Checkbox"},{"name":"RecordId__c","label":"Id de Registro","type":"Text"}],"relationships":[],"incoming":[]},"CreditDefaultFile__c":{"label":"CreditDefaultFile","cat":"custom","fieldCount":24,"fields":[{"name":"APIErrorMessage__c","label":"Mensaje Error API","type":"LongTextArea"},{"name":"ASNEFFeasibilityAnalysis__c","label":"Análisis de Viabilidad ASNEF","type":"MultiselectPicklist"},{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"BADEXCUGFeasibilityAnalysis__c","label":"Análisis de Viabilidad BADEXCUG","type":"MultiselectPicklist"},{"name":"CorrectionDate__c","label":"Fecha Subsanación","type":"Date"},{"name":"DatetimeFileReceivedASNEF__c","label":"Fecha Recepción del Fichero ASNEF","type":"DateTime"},{"name":"DatetimeFileReceivedBADEXCUG__c","label":"Fecha Recepción del Fichero BADEXCUG","type":"DateTime"},{"name":"DatetimeRequest__c","label":"Día y Hora de la Consulta","type":"DateTime"},{"name":"DebtType__c","label":"Tipología de las Deudas","type":"MultiselectPicklist"},{"name":"DownloadASNEF__c","label":"ASNEF Descargado","type":"Checkbox"},{"name":"DownloadBADEXCUG__c","label":"BADEXCUG Descargado","type":"Checkbox"},{"name":"IdAuditTrailRightAccess__c","label":"Id Fichero Auditoría Derecho de Acceso","type":"Text"},{"name":"IdDni__c","label":"Id Fichero DNI","type":"Text"},{"name":"IdRightAccess__c","label":"Id Fichero Derecho de Acceso","type":"Text"},{"name":"IncidentReason__c","label":"Motivo Incidencia","type":"Picklist"},{"name":"NumberDebtsOutsideService__c","label":"Nº Deudas Fuera del Servicio","type":"Number"},{"name":"NumberRequest__c","label":"Nº de Consultas","type":"Number"},{"name":"OldIdBadexcug__c","label":"Old Id Badexcug","type":"Text"},{"name":"Opportunity__c","label":"Oportunidad","type":"Lookup"},{"name":"RequestIncidentDate__c","label":"Fecha Incidencia Consulta","type":"Date"},{"name":"SentRequest__c","label":"Consulta Enviada","type":"Checkbox"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"Status__c","label":"Estado","type":"Picklist"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"CreditDefaultFile","required":false},{"field":"Opportunity__c","relType":"Lookup","referenceTo":"Opportunity","relationshipName":"CreditDefaultFile","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"CreditDefaultFile","required":false}],"incoming":["CreditDefaultFileAnalysis__c"]},"DebtCaseDepartment__c":{"label":"DebtCaseDepartment","cat":"custom","fieldCount":1,"fields":[{"name":"IsActive__c","label":"Activo","type":"Checkbox"}],"relationships":[],"incoming":["Case","DebtCaseReasonCatalog__c","DebtCaseTypeCatalog__c"]},"DebtCaseReasonCatalog__c":{"label":"DebtCaseReasonCatalog","cat":"custom","fieldCount":13,"fields":[{"name":"AllowPriorityEdit__c","label":"Prioridad editable","type":"Checkbox"},{"name":"DebtCaseType__c","label":"Tipos de casos de deudas","type":"MasterDetail"},{"name":"DestinationDepartment__c","label":"Departamento de casos de deuda","type":"Lookup"},{"name":"DueMeasure__c","label":"Medida de vencimiento","type":"Picklist"},{"name":"DueTime__c","label":"Tiempo de vencimiento","type":"Number"},{"name":"IsActive__c","label":"Activo","type":"Checkbox"},{"name":"IsDebtRequired__c","label":"Requiere deuda","type":"Checkbox"},{"name":"Observations__c","label":"Observations","type":"Text"},{"name":"Priority__c","label":"Prioridad","type":"Picklist"},{"name":"Reason__c","label":"Motivo","type":"Text"},{"name":"ResponsibleInCharge__c","label":"Responsable","type":"Lookup"},{"name":"ResponsibleQueue__c","label":"Responsables encolados","type":"Text"},{"name":"Subject__c","label":"Asunto","type":"Text"}],"relationships":[{"field":"DebtCaseType__c","relType":"MasterDetail","referenceTo":"DebtCaseTypeCatalog__c","relationshipName":"DebtCaseType","required":true},{"field":"DestinationDepartment__c","relType":"Lookup","referenceTo":"DebtCaseDepartment__c","relationshipName":"Motivos_de_casos_de_deuda","required":false},{"field":"ResponsibleInCharge__c","relType":"Lookup","referenceTo":"User","relationshipName":"Motivos_de_casos_de_deuda","required":false}],"incoming":["Case"]},"DebtCaseTypeCatalog__c":{"label":"DebtCaseTypeCatalog","cat":"custom","fieldCount":2,"fields":[{"name":"DebtCaseRequestingDepartment__c","label":"Departamento solicitante","type":"MasterDetail"},{"name":"IsActive__c","label":"Activo","type":"Checkbox"}],"relationships":[{"field":"DebtCaseRequestingDepartment__c","relType":"MasterDetail","referenceTo":"DebtCaseDepartment__c","relationshipName":"DebtCaseRequestingDepartment","required":true}],"incoming":["DebtCaseReasonCatalog__c"]},"DebtHolder__c":{"label":"DebtHolder","cat":"custom","fieldCount":4,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"DNI__c","label":"DNI","type":"Text"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"HolderType__c","label":"Tipo de titular","type":"Picklist"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"DebtHolders","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"DebtHolders","required":false}],"incoming":[]},"DebtLawsuit__c":{"label":"DebtLawsuit","cat":"custom","fieldCount":10,"fields":[{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Entity__c","label":"Entidad","type":"Text"},{"name":"LawsuitDate__c","label":"Fecha demanda","type":"Date"},{"name":"LawsuitStatus__c","label":"Estado Demanda","type":"Text"},{"name":"LawsuitType__c","label":"Tipo de Demanda","type":"Text"},{"name":"Lawsuit__c","label":"Demanda","type":"Lookup"},{"name":"MainDebt__c","label":"Deuda Principal","type":"Checkbox"},{"name":"Product__c","label":"Producto","type":"Text"},{"name":"RecordType_Lawsuit__c","label":"Tipo de registro Demanda","type":"Text"},{"name":"Service__c","label":"Servicio","type":"Text"}],"relationships":[{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Demandas_de_la_deuda","required":false},{"field":"Lawsuit__c","relType":"Lookup","referenceTo":"Lawsuit__c","relationshipName":"Demandas_de_la_deuda","required":false}],"incoming":[]},"DebtSettlement__c":{"label":"DebtSettlement","cat":"custom","fieldCount":60,"fields":[{"name":"ARNotes__c","label":"Notas AR","type":"LongTextArea"},{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"AgreementWaitingDateOK__c","label":"Fecha espera acuerdo OK","type":"Date"},{"name":"Amount2__c","label":"Importe","type":"Currency"},{"name":"BankPaymentABS__c","label":"Pago Banco ABS","type":"Currency"},{"name":"BankPayment__c","label":"Pago Banco","type":"Currency"},{"name":"Beneficiary__c","label":"Beneficiario","type":"Text"},{"name":"Concept__c","label":"Concepto","type":"Text"},{"name":"CorrectBankAccountNumber__c","label":"Corregir Nº Cuenta Bancaria","type":"Checkbox"},{"name":"CustomerConfirmationDate__c","label":"Fecha OK cliente","type":"Date"},{"name":"DateOfAgreementFirstPay__c","label":"Fecha acuerdo 1er pago","type":"Date"},{"name":"Discount__c","label":"Descuento %","type":"Percent"},{"name":"DoYouHaveSavingsAvailable__c","label":"Tiene ahorro disponible","type":"Checkbox"},{"name":"EntityContact__c","label":"Contacto entidad","type":"Lookup"},{"name":"Entity__c","label":"Entidad","type":"Lookup"},{"name":"FullID__c","label":"IDCompleto","type":"Text"},{"name":"HasLiquidationDocument__c","label":"Tiene documento de liquidación","type":"Checkbox"},{"name":"HasOpenLiquidations__c","label":"Tiene Liquidaciones Abiertas","type":"Checkbox"},{"name":"HasReadARNotesField__c","label":"Has leído el campo Notas AR","type":"Checkbox"},{"name":"HasRecoveryComeIn__c","label":"Ha entrado el recupero","type":"Date"},{"name":"IBAN__c","label":"IBAN","type":"Lookup"},{"name":"InstallmentAgreementStatus__c","label":"Estado Pagos Fraccionado","type":"Picklist"},{"name":"IsDummy__c","label":"Is Dummy","type":"Checkbox"},{"name":"IsExternalLiquidation__c","label":"Es Liquidación Por Fuera","type":"Checkbox"},{"name":"IsRecoverable__c","label":"Es recupero","type":"Date"},{"name":"Lawsuit__c","label":"Demanda","type":"Lookup"},{"name":"LiquidationStatusChangeDate__c","label":"Fecha estado liquidacion cambio","type":"Date"},{"name":"LiquidationTeam__c","label":"Equipo Liquidación","type":"Picklist"},{"name":"ManualLiquidationFee__c","label":"Comisión liquidación manual","type":"Checkbox"},{"name":"MaximumGracePeriod__c","label":"Máxima carencia (Meses)","type":"Number"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"DebtSettlements","required":false},{"field":"EntityContact__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Liquidaciones","required":false},{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Liquidaciones","required":false},{"field":"IBAN__c","relType":"Lookup","referenceTo":"IBAN__c","relationshipName":"Liquidaci_n_deuda","required":false},{"field":"Lawsuit__c","relType":"Lookup","referenceTo":"Lawsuit__c","relationshipName":"Debtsettlement","required":false},{"field":"ReceiptRequestedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"Liquidaci_n_deuda","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Liquidaci_n_deuda","required":false}],"incoming":["DebtsettlementLine__c","DeletedJournalEntry__c","History__c","Saving__c","SettlementInstallment__c","SettlementPayment__c","VoiceCall_Link__c"]},"DebtStatusExperienceSite__mdt":{"label":"DebtStatusExperienceSite","cat":"metadata","fieldCount":3,"fields":[{"name":"CustomerDescription__c","label":"Customer Description","type":"LongTextArea"},{"name":"CustomerText__c","label":"Customer Text","type":"Text"},{"name":"OriginalText__c","label":"Original Text","type":"Text"}],"relationships":[],"incoming":[]},"DebtStatusHistory__c":{"label":"DebtStatusHistory","cat":"custom","fieldCount":6,"fields":[{"name":"DebtStatus__c","label":"Estado Deuda","type":"TextArea"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"ModifiedBy__c","label":"Modificado Por","type":"TextArea"},{"name":"ModifiedDate__c","label":"Fecha de Modificación","type":"Date"},{"name":"ZohoID__c","label":"Zoho ID","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"DebtStatusHistory","required":false}],"incoming":[]},"Debt__c":{"label":"Debt","cat":"custom","fieldCount":204,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"ActionPlan__c","label":"Plan de Acción","type":"Picklist"},{"name":"Action_Plan_Documentary__c","label":"Plan de acción (Documental)","type":"Picklist"},{"name":"Action_plan_Banking__c","label":"Plan de acción (Bancario)","type":"Picklist"},{"name":"AgreementType__c","label":"Tipo Acuerdo","type":"Picklist"},{"name":"AmountPaidEntity__c","label":"Cantidad pagada a Entidad","type":"Currency"},{"name":"AmountToPay__c","label":"A pagar","type":"Currency"},{"name":"Amount__c","label":"Cantidad","type":"Currency"},{"name":"AnalysisComments__c","label":"Comentarios análisis","type":"LongTextArea"},{"name":"AnalysisDate__c","label":"Fecha Análisis (Bancario)","type":"Date"},{"name":"Analysis_Commentary_Documentary__c","label":"Comentarios Análisis (Documental)","type":"LongTextArea"},{"name":"Answer_LO_1_2025_MASC__c","label":"Respuesta LO 1/2025 (MASC)","type":"Date"},{"name":"Answer_Type_LO_1_2025_MASC__c","label":"Tipo Respuesta LO 1/2025 (MASC)","type":"Picklist"},{"name":"ApproxTimePaying__c","label":"Tiempo pagando aprox.","type":"Picklist"},{"name":"AverageDiscount__c","label":"Promedio Descuento","type":"Number"},{"name":"BorrowedCapital__c","label":"Capital Prestado","type":"Currency"},{"name":"BurofaxDate__c","label":"Fecha Burofax","type":"Date"},{"name":"BurofaxReferenceNumber__c","label":"Nº referencia burofax","type":"Number"},{"name":"BurofaxResponseDate__c","label":"Fecha respuesta burofax","type":"Date"},{"name":"ClaimDateMail__c","label":"Fecha reclamación mail","type":"Date"},{"name":"CompleteID__c","label":"ID Completo","type":"Text"},{"name":"CompletedInstallmentPaymentAgreementDate__c","label":"Fecha acuerdo de pagos fracc. finalizado","type":"Date"},{"name":"ConsumerCredit1to5Debt__c","label":"Créditos consumo 1 a 5 deuda","type":"Number"},{"name":"ConsumerCreditMoreto5Debt__c","label":"Créditos consumo + de 5 deuda","type":"Number"},{"name":"ConsumerCreditYearDebt__c","label":"Créditos consumo 1 año deuda","type":"Number"},{"name":"ContractYear__c","label":"Año Contratación","type":"Text"},{"name":"Contract_Notes__c","label":"Notas Contrato","type":"LongTextArea"},{"name":"CreatedLawsuitRevolving__c","label":"CreatedLawsuitRevolving","type":"Checkbox"},{"name":"CreditNumberObtainedDate__c","label":"Fecha Nº Crédito conseguido","type":"Date"},{"name":"CreditNumberObtained__c","label":"Nº Crédito conseguido","type":"Checkbox"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Debts","required":false},{"field":"DebtRecovery__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Deudas","required":false},{"field":"DocManager__c","relType":"Lookup","referenceTo":"User","relationshipName":"Debts_DocManager","required":false},{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"DebtsFromEntity","required":false},{"field":"Head_Of_Banking_Analysis__c","relType":"Lookup","referenceTo":"User","relationshipName":"Deudas","required":false},{"field":"Head_of_analysis_Documentary__c","relType":"Lookup","referenceTo":"User","relationshipName":"Deudas1","required":false},{"field":"IBAN__c","relType":"Lookup","referenceTo":"IBAN__c","relationshipName":"DebtsWithIBAN","required":false},{"field":"InstallmentPaymentAgreementEntity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"InstallmentAgreements","required":false},{"field":"Lead__c","relType":"Lookup","referenceTo":"Lead","relationshipName":"Debts","required":false},{"field":"ManualBurofaxManager__c","relType":"Lookup","referenceTo":"User","relationshipName":"ManualBurofaxDebts","required":false},{"field":"MarkedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"MarkedByDebts","required":false},{"field":"Marked_By_Documentary__c","relType":"Lookup","referenceTo":"User","relationshipName":"Deudas3","required":false},{"field":"ParentDebt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Deudas","required":false},{"field":"PreviousEntity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"PreviousEntityDebts","required":false},{"field":"ProductEntity__c","relType":"Lookup","referenceTo":"EntityProduct__c","relationshipName":"ProductEntityDebts","required":false},{"field":"ProductPreviousEntity__c","relType":"Lookup","referenceTo":"EntityProduct__c","relationshipName":"ProductPreviousEntityDebts","required":false},{"field":"ResponsibleReviewer__c","relType":"Lookup","referenceTo":"User","relationshipName":"ResponsibleReviewerDebts","required":false},{"field":"Review_Manager_Banking__c","relType":"Lookup","referenceTo":"User","relationshipName":"Deudas4","required":false},{"field":"Review_Manager_Documentary__c","relType":"Lookup","referenceTo":"User","relationshipName":"Deudas2","required":false},{"field":"ReviewedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"ReviewedByDebts","required":false}],"incoming":["Burofax__c","Case","CreditDefaultFileAnalysis__c","DebtHolder__c","DebtLawsuit__c","DebtStatusHistory__c","Debt__c","DebtsettlementLine__c","DeletedJournalEntry__c","History__c","Lawsuit__c","Notification__c","SalesDebt__c","Saving__c","SettlementInstallment__c","SettlementPayment__c","VoiceCall_Link__c"]},"DebtsettlementLine__c":{"label":"DebtsettlementLine","cat":"custom","fieldCount":17,"fields":[{"name":"Account__c","label":"Cliente","type":"Text"},{"name":"AmountToPay__c","label":"A pagar","type":"Currency"},{"name":"Amount__c","label":"Cantidad","type":"Currency"},{"name":"BankPayment__c","label":"Pago Banco","type":"Currency"},{"name":"DebtNumber__c","label":"Nº Deuda","type":"Text"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Debtsettlement__c","label":"Liquidación","type":"Lookup"},{"name":"Discount__c","label":"% descuento","type":"Percent"},{"name":"LiquidationTeam__c","label":"Equipo Liquidación","type":"Text"},{"name":"PaymentDate__c","label":"Fecha a pagar","type":"Date"},{"name":"PaymentType__c","label":"Tipo de pago","type":"Text"},{"name":"ProductEntity__c","label":"Producto Entidad","type":"Text"},{"name":"ReasonLostSettlement__c","label":"Motivo liquidación perdida","type":"Text"},{"name":"SettlementClosingDate__c","label":"Fecha Cierre liquidación","type":"Date"},{"name":"SettlementCommission__c","label":"Comisión liquidación calculada","type":"Currency"},{"name":"Stage__c","label":"Etapa","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"DebtsettlementLine","required":false},{"field":"Debtsettlement__c","relType":"Lookup","referenceTo":"DebtSettlement__c","relationshipName":"DebtsettlementLine","required":false}],"incoming":[]},"DeletedJournalEntry__c":{"label":"DeletedJournalEntry","cat":"custom","fieldCount":23,"fields":[{"name":"Balance__c","label":"Saldo","type":"Currency"},{"name":"BankHolded__c","label":"Banco Holded","type":"Text"},{"name":"DateSaving__c","label":"Fecha Ahorro","type":"Date"},{"name":"DebtSettlement__c","label":"Liquidación deuda","type":"Lookup"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"DoYouHaveEnoughSavings__c","label":"¿Tiene Ahorro suficiente?","type":"Checkbox"},{"name":"Entity__c","label":"Entidad Bancaria","type":"Lookup"},{"name":"HoldedId__c","label":"Id Holded","type":"TextArea"},{"name":"InitialCommission__c","label":"Comisión Inicial","type":"Currency"},{"name":"Invoiced__c","label":"Facturado","type":"Checkbox"},{"name":"MonthlyCommission__c","label":"Comisión Mensual","type":"Currency"},{"name":"MonthlyContribution__c","label":"Aportación Mensual","type":"Currency"},{"name":"PaymentGateway__c","label":"¿Pago por Pasarela?","type":"Checkbox"},{"name":"Provision__c","label":"Provisión","type":"Currency"},{"name":"RealTotalSaving__c","label":"Total Ahorro Real","type":"Currency"},{"name":"Record_type__c","label":"Record Type","type":"Text"},{"name":"RectifiedEntryDate__c","label":"Fecha asiento rectificado","type":"Date"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"SettlementCommission__c","label":"Comisión Liquidación","type":"Currency"},{"name":"Settlement__c","label":"Liquidación","type":"Currency"},{"name":"TotalAvailableSavings__c","label":"Total Ahorro Disponible","type":"Currency"},{"name":"Type__c","label":"Tipo","type":"Picklist"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"DebtSettlement__c","relType":"Lookup","referenceTo":"DebtSettlement__c","relationshipName":"Ahorros_eliminados","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Ahorros_eliminados","required":false},{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Ahorros_eliminados","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Ahorros_eliminados","required":false}],"incoming":[]},"DepartmentalManagers__mdt":{"label":"DepartmentalManagers","cat":"metadata","fieldCount":1,"fields":[{"name":"ResponsableUserId__c","label":"Id de Responsable","type":"Text"}],"relationships":[],"incoming":[]},"DocumentSignatureConfiguration__mdt":{"label":"DocumentSignatureConfiguration","cat":"metadata","fieldCount":8,"fields":[{"name":"ApiNameVisualforcePDF__c","label":"ApiName Visualforce PDF","type":"Text"},{"name":"ExpirationDays__c","label":"Duración de caducidad","type":"Number"},{"name":"IsActive__c","label":"Activo","type":"Checkbox"},{"name":"MasterObjectShipping__c","label":"Objeto Maestro Envío","type":"Text"},{"name":"Order__c","label":"Orden","type":"Text"},{"name":"ReminderFrequency__c","label":"Frecuencia Recordatorio (en días)","type":"Number"},{"name":"Subject__c","label":"Asunto","type":"Text"},{"name":"VisualforceEmailTemplate__c","label":"VisualforceEmailTemplate","type":"Text"}],"relationships":[],"incoming":[]},"EmailMessage":{"label":"Email Message","cat":"standard","fieldCount":1,"fields":[{"name":"OldId__c","label":"OldId","type":"Text"}],"relationships":[{"field":"RelatedToId","relType":"Lookup","referenceTo":"Opportunity","relationshipName":null,"required":false}],"incoming":[]},"Email_To_Case_Settings__mdt":{"label":"Email To Case Settings","cat":"metadata","fieldCount":4,"fields":[{"name":"Department__c","label":"Departamento","type":"Picklist"},{"name":"Email__c","label":"Email","type":"Text"},{"name":"Excluded_Emails__c","label":"Excluded Emails","type":"LongTextArea"},{"name":"Sandbox_Email__c","label":"Email Sandbox","type":"Text"}],"relationships":[],"incoming":[]},"EntityProduct__c":{"label":"EntityProduct","cat":"custom","fieldCount":18,"fields":[{"name":"ClaimEmail__c","label":"Correo Reclamación","type":"Email"},{"name":"Discount__c","label":"% descuento","type":"Percent"},{"name":"DocumentationManagement__c","label":"Gestión Documentación","type":"Picklist"},{"name":"Entity__c","label":"Entidad","type":"MasterDetail"},{"name":"FullID__c","label":"IDCompleto","type":"Text"},{"name":"IsAvailable__c","label":"Habilitado","type":"Checkbox"},{"name":"IsToLittleProgram__c","label":"¿Es para Miniprograma?","type":"Checkbox"},{"name":"LiquidationManagement__c","label":"Gestión Liquidación","type":"Picklist"},{"name":"Management__c","label":"Gestión","type":"Picklist"},{"name":"NeedContract__c","label":"Contrato","type":"Checkbox"},{"name":"NeedPowerOfAttorney__c","label":"Poder","type":"Checkbox"},{"name":"NeedPreviousFinance__c","label":"Necesita Financiera anterior","type":"Checkbox"},{"name":"ProductType__c","label":"Tipo de Producto","type":"Picklist"},{"name":"RightAccessEmail__c","label":"Correo Derecho de Acceso","type":"Email"},{"name":"Services__c","label":"Servicios","type":"MultiselectPicklist"},{"name":"WhoReceivesRecovery__c","label":"Quien recibe el recupero","type":"Picklist"},{"name":"oldEntity__c","label":"oldEntity","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Entity__c","relType":"MasterDetail","referenceTo":"Entity__c","relationshipName":"EntityProducts","required":true}],"incoming":["Burofax__c","Debt__c"]},"Entity__c":{"label":"Entity","cat":"custom","fieldCount":24,"fields":[{"name":"AccountType__c","label":"Tipo de Entidad","type":"Picklist"},{"name":"Active__c","label":"Habilitada","type":"Checkbox"},{"name":"Assigned_document_manager__c","label":"Gestor documentación asignado","type":"Lookup"},{"name":"AverageDiscount__c","label":"Promedio Descuento (%)","type":"Percent"},{"name":"CIF__c","label":"CIF","type":"Text"},{"name":"CompanyName__c","label":"Razón Social","type":"Text"},{"name":"Email_DPD__c","label":"Correo DPD","type":"Email"},{"name":"Email__c","label":"Email","type":"Email"},{"name":"FriendlyManagement__c","label":"Gestión Amistosa","type":"Picklist"},{"name":"FullId__c","label":"IDCompleto","type":"Text"},{"name":"IsProgramProduct__c","label":"IsProgramProduct","type":"Checkbox"},{"name":"KmaleonCode__c","label":"Código Kmaleon","type":"Number"},{"name":"Negotiator__c","label":"Negociador","type":"Lookup"},{"name":"PJEntityCode__c","label":"Código PJ Entidad","type":"Text"},{"name":"PJEntity__c","label":"PJ Entidad","type":"Text"},{"name":"Phone__c","label":"Teléfono","type":"Phone"},{"name":"PjCodeEntity__c","label":"Código PJ Entidad","type":"Text"},{"name":"Province__c","label":"Provincia","type":"Text"},{"name":"SACAddress__c","label":"Dirección SAC","type":"TextArea"},{"name":"SocialAddress__c","label":"Dirección Social","type":"TextArea"},{"name":"ZohoId__c","label":"Zoho Id","type":"Text"},{"name":"oldDocumentManager__c","label":"oldDocumentManager","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"},{"name":"oldNegotiator__c","label":"oldNegotiator","type":"Text"}],"relationships":[{"field":"Assigned_document_manager__c","relType":"Lookup","referenceTo":"User","relationshipName":"Entidades_Bancarias1","required":false},{"field":"Negotiator__c","relType":"Lookup","referenceTo":"User","relationshipName":"Entidades_Bancarias","required":false}],"incoming":["Account","Burofax__c","ContentVersion","CreditDefaultFileAnalysis__c","DebtSettlement__c","Debt__c","DeletedJournalEntry__c","EntityProduct__c","IBAN__c","Lawsuit__c","Lead","Saving__c","signaturit__SignatureRequest__c"]},"ErrorLog__c":{"label":"ErrorLog","cat":"custom","fieldCount":12,"fields":[{"name":"Cause__c","label":"Causa","type":"Text"},{"name":"Class__c","label":"Clase","type":"Text"},{"name":"Description__c","label":"Descripción","type":"LongTextArea"},{"name":"EndPoint__c","label":"EndPoint","type":"Text"},{"name":"ErrorCode__c","label":"Código error","type":"Number"},{"name":"HttpMethod__c","label":"Método Http","type":"Text"},{"name":"LineNumber__c","label":"Número de línea","type":"Number"},{"name":"Message__c","label":"Mensaje","type":"Text"},{"name":"Method__c","label":"Método","type":"Text"},{"name":"StackTrace__c","label":"Traza de pila","type":"LongTextArea"},{"name":"Type__c","label":"Tipo","type":"Text"},{"name":"WebServiceName__c","label":"Nombre del Servicio Web","type":"Text"}],"relationships":[],"incoming":[]},"Good__c":{"label":"Good","cat":"custom","fieldCount":108,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"AcquisitionPrice__c","label":"Precio adquisición","type":"Currency"},{"name":"AcquisitionTitleRegistered__c","label":"Título de adquisición I. Inscrito","type":"Picklist"},{"name":"AcquisitionTitle__c","label":"Título de adquisición","type":"Picklist"},{"name":"Address__c","label":"Dirección","type":"Address"},{"name":"BookRegistered__c","label":"Libro I. Inscrito","type":"Text"},{"name":"Book__c","label":"Libro","type":"Text"},{"name":"CompensationExtinctionRegistered__c","label":"Contrapres. ext. o liquid. I. Inscrito","type":"Currency"},{"name":"CompensationExtinction__c","label":"Contraprestación extinción o liquidación","type":"Currency"},{"name":"Condition__c","label":"Condición","type":"Text"},{"name":"CurrentHomeValue__c","label":"Valor Actual Vivienda","type":"Currency"},{"name":"DateAcceptancePartition__c","label":"Fecha aceptación y partición, o renuncia","type":"Date"},{"name":"DatePHGrantRegistered__c","label":"Fecha otorgamiento PH I. Inscrito","type":"Date"},{"name":"DatePHGrant__c","label":"Fecha Otorgamiento PH","type":"Date"},{"name":"Description_Good__c","label":"Descripción del bien","type":"Text"},{"name":"DonationDate__c","label":"Fecha donación","type":"Date"},{"name":"DonationValue__c","label":"Valor donación","type":"Currency"},{"name":"DwellingType__c","label":"Tipo de Vivienda","type":"Picklist"},{"name":"ExpirationDateRegistered__c","label":"Fecha extinción o liquidación I. Inscrit","type":"Date"},{"name":"ExpirationDate__c","label":"Fecha extinción o liquidación","type":"Date"},{"name":"GoodAmount__c","label":"Importe Bien","type":"Currency"},{"name":"GoodType__c","label":"Tipo de Propiedad","type":"Picklist"},{"name":"InheritedAsset__c","label":"Bien Heredado","type":"Checkbox"},{"name":"Lead__c","label":"Lead","type":"Lookup"},{"name":"LeaseContract__c","label":"Contrato de alquiler","type":"Checkbox"},{"name":"LeasedProperty__c","label":"Vivienda alquilada","type":"Checkbox"},{"name":"MaxLoanAmount__c","label":"Maximo Dinero a Prestar","type":"Currency"},{"name":"MonthlyInstallment__c","label":"Cuota Mensual","type":"Currency"},{"name":"MonthlyRent__c","label":"Mensualidad alquiler","type":"Currency"},{"name":"MortgageBank__c","label":"Banco Hipoteca","type":"Picklist"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Goods","required":false},{"field":"Lead__c","relType":"Lookup","referenceTo":"Lead","relationshipName":"Goods","required":false}],"incoming":[]},"History__c":{"label":"History","cat":"custom","fieldCount":21,"fields":[{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"Bankruptcy__c","label":"Concurso","type":"Lookup"},{"name":"Burofax__c","label":"Burofax","type":"Lookup"},{"name":"ChangedAt__c","label":"Fecha de modificación","type":"DateTime"},{"name":"ChangedByName__c","label":"Modificado Por (Nombre)","type":"Text"},{"name":"ChangedBy__c","label":"Modificado Por","type":"Lookup"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Debtsettlement__c","label":"Liquidación","type":"Lookup"},{"name":"Field__c","label":"Campo","type":"Text"},{"name":"IsCreation__c","label":"Is Creation","type":"Checkbox"},{"name":"Lawsuit__c","label":"Demanda","type":"Lookup"},{"name":"Lead__c","label":"Candidato","type":"Lookup"},{"name":"MigrationSource__c","label":"Migration Source","type":"Text"},{"name":"NewValue__c","label":"Valor Nuevo","type":"LongTextArea"},{"name":"OldId__c","label":"Old Id","type":"Text"},{"name":"OldValue__c","label":"Valor Anterior","type":"LongTextArea"},{"name":"Opportunity__c","label":"Oportunidad","type":"Lookup"},{"name":"ParentObjectType__c","label":"Tipo de Objeto Padre","type":"Text"},{"name":"Process__c","label":"Proceso","type":"Lookup"},{"name":"Saving__c","label":"Ahorro","type":"Lookup"},{"name":"Service__c","label":"Servicio","type":"Lookup"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"HistoryRecords_Account","required":false},{"field":"Bankruptcy__c","relType":"Lookup","referenceTo":"Bankruptcy__c","relationshipName":"HistoryRecords_Bankruptcy","required":false},{"field":"Burofax__c","relType":"Lookup","referenceTo":"Burofax__c","relationshipName":"HistoryRecords_Burofax","required":false},{"field":"ChangedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"HistoryRecords_ChangedBy","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"HistoryRecords_Debt","required":false},{"field":"Debtsettlement__c","relType":"Lookup","referenceTo":"DebtSettlement__c","relationshipName":"HistoryRecords_Debtsettlement","required":false},{"field":"Lawsuit__c","relType":"Lookup","referenceTo":"Lawsuit__c","relationshipName":"HistoryRecords_Lawsuit","required":false},{"field":"Lead__c","relType":"Lookup","referenceTo":"Lead","relationshipName":"HistoryRecords_Lead","required":false},{"field":"Opportunity__c","relType":"Lookup","referenceTo":"Opportunity","relationshipName":"HistoryRecords_Opportunity","required":false},{"field":"Process__c","relType":"Lookup","referenceTo":"Process__c","relationshipName":"HistoryRecords_Process","required":false},{"field":"Saving__c","relType":"Lookup","referenceTo":"Saving__c","relationshipName":"HistoryRecords_Saving","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"HistoryRecords_Service","required":false}],"incoming":[]},"HojaDeEncargoSMD__mdt":{"label":"HojaDeEncargoSMD","cat":"metadata","fieldCount":9,"fields":[{"name":"CifraTextoNoTotal__c","label":"CifraTextoNoTotal","type":"Text"},{"name":"CifraTextoYNumerica__c","label":"CifraTextoYNumerica","type":"Text"},{"name":"CifraTexto__c","label":"Cifra Texto","type":"Text"},{"name":"CifraYTextoApertura__c","label":"CifraYTextoApertura","type":"Text"},{"name":"ImporteApertura__c","label":"ImporteApertura","type":"Number"},{"name":"ImporteNeto__c","label":"ImporteNeto","type":"Number"},{"name":"ImporteTotal__c","label":"ImporteTotal","type":"Number"},{"name":"TipoAcuerdo__c","label":"TipoAcuerdo","type":"Text"},{"name":"TipoTitular__c","label":"TipoTitular","type":"Text"}],"relationships":[],"incoming":[]},"IBAN__c":{"label":"IBAN","cat":"custom","fieldCount":6,"fields":[{"name":"AttachedTitleCertificate__c","label":"Certificado titularidad adjuntado","type":"Checkbox"},{"name":"Entity__c","label":"Entidad","type":"Lookup"},{"name":"Last4Digits__c","label":"Últimos 4 dígitos","type":"Text"},{"name":"RequiredCertificate__c","label":"Necesita certificado","type":"Checkbox"},{"name":"oldEntity__c","label":"oldEntity","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"IBANs","required":false}],"incoming":["DebtSettlement__c","Debt__c"]},"Kmaleon_Auth__mdt":{"label":"Kmaleon Auth","cat":"metadata","fieldCount":4,"fields":[{"name":"Auth__c","label":"Auth","type":"Text"},{"name":"ClientId__c","label":"Client Id","type":"Text"},{"name":"GrantType__c","label":"Grant Type","type":"Text"},{"name":"Redirect_Uri__c","label":"Redirect Uri","type":"Text"}],"relationships":[],"incoming":[]},"Kmaleon_Setting__mdt":{"label":"Kmaleon Setting","cat":"metadata","fieldCount":7,"fields":[{"name":"Active__c","label":"Active","type":"Checkbox"},{"name":"End_Point_PROD__c","label":"End Point PROD","type":"LongTextArea"},{"name":"End_Point__c","label":"End Point","type":"LongTextArea"},{"name":"Headers__c","label":"Headers","type":"LongTextArea"},{"name":"Method__c","label":"Method","type":"Text"},{"name":"TimeOut__c","label":"TimeOut","type":"Number"},{"name":"Token__c","label":"Token","type":"LongTextArea"}],"relationships":[],"incoming":[]},"LSODocumentEquivalence__c":{"label":"LSODocumentEquivalence","cat":"custom","fieldCount":3,"fields":[{"name":"Description__c","label":"Descripción","type":"TextArea"},{"name":"Document__c","label":"Documento","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[],"incoming":[]},"LSODocument__c":{"label":"LSODocument","cat":"custom","fieldCount":13,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"AttachedBy__c","label":"Adjuntado por","type":"Lookup"},{"name":"AttachedDocument__c","label":"Documento Adjuntado","type":"Checkbox"},{"name":"Comment__c","label":"Comentario","type":"LongTextArea"},{"name":"DateRequest__c","label":"Fecha solicitud","type":"Date"},{"name":"DiscardDocument__c","label":"Descartar documento","type":"Checkbox"},{"name":"DocumentType__c","label":"Tipo Documento","type":"Picklist"},{"name":"Order__c","label":"Orden","type":"Number"},{"name":"Responsible__c","label":"Responsable","type":"Picklist"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"TimeRequest__c","label":"Tiempo petición","type":"Number"},{"name":"UniqueId__c","label":"Id Único","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"LSODocuments","required":false},{"field":"AttachedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"LSODocuments","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Documentos_LSO","required":false}],"incoming":[]},"LSOWordDocumentType__mdt":{"label":"LSOWordDocumentType","cat":"metadata","fieldCount":5,"fields":[{"name":"FileName__c","label":"File Name","type":"Text"},{"name":"IsActive__c","label":"Is Active","type":"Checkbox"},{"name":"MemoriaType__c","label":"Memoria Type","type":"Text"},{"name":"Order__c","label":"Order","type":"Number"},{"name":"PageName__c","label":"Page Name","type":"Text"}],"relationships":[],"incoming":[]},"LSO_cmt_Document__mdt":{"label":"LSO cmt Document","cat":"metadata","fieldCount":3,"fields":[{"name":"JsonData__c","label":"JSON","type":"LongTextArea"},{"name":"PDFMetadataCouple__c","label":"Tipificación y Nombre pdf Pareja","type":"Picklist"},{"name":"PDFMetadata__c","label":"Tipificación y Nombre pdf","type":"Picklist"}],"relationships":[],"incoming":[]},"LawsuitLog__c":{"label":"LawsuitLog","cat":"custom","fieldCount":8,"fields":[{"name":"API_Field__c","label":"Campo API","type":"Text"},{"name":"DateCreationComplete__c","label":"Fecha Creación Completa","type":"DateTime"},{"name":"Error__c","label":"Error","type":"LongTextArea"},{"name":"Field__c","label":"Campo","type":"Text"},{"name":"Lawsuit__c","label":"Demanda","type":"MasterDetail"},{"name":"NewValue__c","label":"Nuevo Valor","type":"Text"},{"name":"OldValue__c","label":"Antiguo Valor","type":"Text"},{"name":"Retry__c","label":"Reintentado","type":"Checkbox"}],"relationships":[{"field":"Lawsuit__c","relType":"MasterDetail","referenceTo":"Lawsuit__c","relationshipName":"Log_Demandas","required":true}],"incoming":[]},"Lawsuit__c":{"label":"Lawsuit","cat":"custom","fieldCount":181,"fields":[{"name":"APVISTADate__c","label":"Fecha AP/VISTA","type":"Date"},{"name":"APVISTANotificationDate__c","label":"Fecha notif. señalamiento AP/VISTA","type":"Date"},{"name":"APVISTASuspensionDate__c","label":"Fecha suspensión AP/VISTA","type":"Date"},{"name":"ASNEFConsultationDate__c","label":"Fecha de la consulta Asnef","type":"Date"},{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"AdmissionDate__c","label":"Fecha admision a tramite","type":"Date"},{"name":"AdmissionTransferDate__c","label":"Fecha traslado allanamiento","type":"Date"},{"name":"AffectedCIDLawsuit__c","label":"Demanda CI afectada","type":"Checkbox"},{"name":"AgreementDate__c","label":"Fecha acuerdo","type":"Date"},{"name":"Agreement__c","label":"Acuerdo","type":"Text"},{"name":"AmountRequestedMonitoringPayment__c","label":"Cuantía requeri. de pago monitorio","type":"Currency"},{"name":"AnnualLatePenaltyPercentage__c","label":"% anual de la penalización por mora","type":"Text"},{"name":"AppealOppositionDate__c","label":"Fecha oposición apelación presentada","type":"Date"},{"name":"AppealPosition__c","label":"Posición Apelación","type":"Text"},{"name":"AppealSubmittedDate__c","label":"Fecha apelación presentada","type":"Date"},{"name":"AppliedCommission__c","label":"Comision aplicada","type":"Picklist"},{"name":"ApprovedCostsDate__c","label":"Fecha costas aprobadas","type":"Date"},{"name":"ApprovedCostsWithVAT__c","label":"Cuantía costas aprobadas con IVA","type":"Currency"},{"name":"AssignedTo__c","label":"Asignado a","type":"Lookup"},{"name":"AuxiliarySubmissionDate__c","label":"Fecha presentación Aux","type":"Date"},{"name":"BankruptcySubmissionDate__c","label":"Fecha presentación concurso","type":"Date"},{"name":"BurofaxDate__c","label":"Fecha Burofax","type":"Date"},{"name":"Burofax__c","label":"Burofax","type":"Lookup"},{"name":"City__c","label":"Poblacion","type":"Text"},{"name":"ClaimManager__c","label":"Responsable reclamadora","type":"Lookup"},{"name":"ClaimNonPayment__c","label":"C. Reclamación impago","type":"Checkbox"},{"name":"ClaimantReviewDate__c","label":"Fecha revisión reclamadora","type":"Date"},{"name":"ClaimingDate__c","label":"Fecha Reclamadora","type":"Date"},{"name":"ClientOHDeclaration__c","label":"Declaración cliente OH","type":"Picklist"},{"name":"CollectedCostsDate__c","label":"Fecha costas cobradas","type":"Date"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Demandas","required":false},{"field":"AssignedTo__c","relType":"Lookup","referenceTo":"User","relationshipName":"LawsuitsAssigned","required":false},{"field":"Burofax__c","relType":"Lookup","referenceTo":"Burofax__c","relationshipName":"Lawsuits","required":false},{"field":"ClaimManager__c","relType":"Lookup","referenceTo":"User","relationshipName":"Demandas1","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Lawsuits","required":false},{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Lawsuits","required":false},{"field":"MadeBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"Demandas","required":false},{"field":"ReportingEntity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"LawsuitsReporting","required":false},{"field":"ReviewedBy__c","relType":"Lookup","referenceTo":"User","relationshipName":"LawsuitsReviewed","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Lawsuits","required":false}],"incoming":["DebtLawsuit__c","DebtSettlement__c","History__c","LawsuitLog__c","Saving__c","VoiceCall_Link__c"]},"Lead":{"label":"Lead","cat":"standard","fieldCount":152,"fields":[{"name":"AgreeDataProcessing__c","label":"Acepto tratamiento de mis datos","type":"Checkbox"},{"name":"AgreeReceiveInformation__c","label":"Acepto recibir información","type":"Checkbox"},{"name":"AmountOtherPrivateDebts__c","label":"Cuantía Otras Deudas Privadas","type":"Currency"},{"name":"AnsweredCalls__c","label":"Llamadas contestadas","type":"Number"},{"name":"AnyDebtsHaveGuaranteeResponded__c","label":"Deuda con aval - Respondido","type":"Checkbox"},{"name":"AnyDebtsHaveGuaranteeResult__c","label":"¿Deudas con Aval? - Resultado","type":"Text"},{"name":"AnyDebtsHaveGuarantee__c","label":"¿Alguna de Estas Deudas Tiene Aval?","type":"Checkbox"},{"name":"Birthdate__c","label":"Fecha de nacimiento","type":"Date"},{"name":"BoughtHouseFiveYearsPayingMortgage__c","label":"Compró Casa Hace 5 Años y Paga Hipoteca","type":"Picklist"},{"name":"CallLaterAccumulated__c","label":"Acumulado Estado - Llamar más tarde","type":"Number"},{"name":"CanCeaseSelfEmployedActivity__c","label":"¿Puede cesar su actividad como autónomo?","type":"Checkbox"},{"name":"CanProvideProofResponded__c","label":"¿Puede Demos. con Justif.? - Respondido","type":"Checkbox"},{"name":"CanProvideProofResult__c","label":"¿Puede Demos. con Justif.? - Resultado","type":"Text"},{"name":"CanProvideProof__c","label":"¿Puede Demostrarlo con Justificante?","type":"Checkbox"},{"name":"CatchmentChannel__c","label":"Canal de captación","type":"Picklist"},{"name":"CatchmentKeywords__c","label":"Adset de captación / Keywords","type":"Text"},{"name":"ClearedDebtWithFundsResponded__c","label":"¿Dicho Dinero Liq. Deuda? -  Respondido","type":"Checkbox"},{"name":"ClearedDebtWithFundsResult__c","label":"¿Dicho Dinero Liquidó Deuda? - Resultado","type":"Text"},{"name":"ClearedDebtWithFunds__c","label":"¿Dicho Dinero Liquidó Deuda?","type":"Checkbox"},{"name":"CloseDate__c","label":"Fecha de cierre","type":"Date"},{"name":"ClosingReason__c","label":"Motivo de Cierre","type":"Picklist"},{"name":"CohabitantsOtherIncome__c","label":"Ingresos de Otras Personas Convivientes","type":"Currency"},{"name":"Collaborator__c","label":"Colaborador","type":"Lookup"},{"name":"ContactTime__c","label":"Hora de contacto","type":"Text"},{"name":"ContactedAccumulated__c","label":"Acumulado Estado - Contactado","type":"Number"},{"name":"ContactedDate__c","label":"Fecha Contacto","type":"Date"},{"name":"ContractType__c","label":"Tipo de contrato","type":"Picklist"},{"name":"DateTimeNextStep__c","label":"Fecha y Hora Siguiente Paso","type":"DateTime"},{"name":"DebtAmount__c","label":"Cantidad Deuda Formulario","type":"Currency"},{"name":"DependentPeople__c","label":"Personas dependientes","type":"Number"}],"relationships":[{"field":"Collaborator__c","relType":"Lookup","referenceTo":"User","relationshipName":"Leads","required":false},{"field":"MasterLead__c","relType":"Lookup","referenceTo":"Lead","relationshipName":"DuplicatedLeads","required":false},{"field":"MortgageBankLook__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"MortgageBankEntity","required":false},{"field":"OwnerOppDuplicateLead__c","relType":"Lookup","referenceTo":"User","relationshipName":"OwnerOppDuplicateLead","required":false},{"field":"PayrollBank__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"PayrollBankEntity","required":false},{"field":"ConvertedAccountId","relType":"Lookup","referenceTo":"Account","relationshipName":null,"required":false},{"field":"ConvertedOpportunityId","relType":"Lookup","referenceTo":"Opportunity","relationshipName":null,"required":false}],"incoming":["Debt__c","Good__c","History__c","Lead","VoiceCall_Link__c","signaturit__SignatureRequestSigner__c","CampaignMember"]},"LegalProfessional__mdt":{"label":"LegalProfessional","cat":"metadata","fieldCount":5,"fields":[{"name":"Area__c","label":"Area","type":"Picklist"},{"name":"DNI_lawyer__c","label":"DNI_lawyer","type":"Text"},{"name":"Description__c","label":"Descripcion","type":"TextArea"},{"name":"RightToHonor__c","label":"Derecho al honor","type":"Checkbox"},{"name":"Role__c","label":"Rol","type":"Picklist"}],"relationships":[],"incoming":[]},"MASC_Configuration_Email__mdt":{"label":"MASC Configuration Email","cat":"metadata","fieldCount":2,"fields":[{"name":"Email_Template_Name__c","label":"Email Template Name","type":"Text"},{"name":"From_Email__c","label":"From Email","type":"Email"}],"relationships":[],"incoming":[]},"ManagerSlotConfiguration__mdt":{"label":"ManagerSlotConfiguration","cat":"metadata","fieldCount":5,"fields":[{"name":"LookAheadDays__c","label":"Look Ahead Days","type":"Number"},{"name":"SlotsPerDay__c","label":"Slots Per Day","type":"Number"},{"name":"StartHour__c","label":"Start Hour","type":"Number"},{"name":"StartMinute__c","label":"Start Minute","type":"Number"},{"name":"TotalWorkMinutes__c","label":"Total Work Minutes","type":"Number"}],"relationships":[],"incoming":[]},"Notificados__mdt":{"label":"Notificados","cat":"metadata","fieldCount":15,"fields":[{"name":"Password_Test__c","label":"Password Test","type":"Text"},{"name":"Password__c","label":"Password","type":"Text"},{"name":"SenderAddress__c","label":"Dirección Remitente","type":"Text"},{"name":"SenderCity__c","label":"Localidad Remitente","type":"Text"},{"name":"SenderCompany__c","label":"Empresa Remitente","type":"Text"},{"name":"SenderLastName__c","label":"Apellidos Remitente","type":"Text"},{"name":"SenderName__c","label":"Nombre Remitente","type":"Text"},{"name":"SenderPostalCode__c","label":"Código Postal Remitente","type":"Text"},{"name":"SenderProvince__c","label":"Provincia Remitente","type":"Text"},{"name":"Term__c","label":"Plazo","type":"Text"},{"name":"URL_Prod__c","label":"URL Prod","type":"Url"},{"name":"URL_Test__c","label":"URL Test","type":"Url"},{"name":"URL__c","label":"URL","type":"Url"},{"name":"User_Test__c","label":"Usuario Test","type":"Text"},{"name":"User__c","label":"Usuario","type":"Text"}],"relationships":[],"incoming":[]},"NotificationMessageSetting__mdt":{"label":"NotificationMessageSetting","cat":"metadata","fieldCount":1,"fields":[{"name":"NotificationBodyTemplate__c","label":"Notification Body Template","type":"TextArea"}],"relationships":[],"incoming":[]},"NotificationTypeIds__mdt":{"label":"NotificationTypeIds","cat":"metadata","fieldCount":1,"fields":[{"name":"NotificationTypeId__c","label":"Tipo de notificación","type":"Text"}],"relationships":[],"incoming":[]},"Notification__c":{"label":"Notification","cat":"custom","fieldCount":9,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"ExplanatoryBanner__c","label":"Banner Explicativo","type":"TextArea"},{"name":"LongName__c","label":"Nombre Largo","type":"Text"},{"name":"NotificationDate__c","label":"Fecha Notificación","type":"Date"},{"name":"NotificationType__c","label":"Tipo Notificación","type":"Picklist"},{"name":"Notification__c","label":"Notificación","type":"Checkbox"},{"name":"Portal__c","label":"Portal","type":"Checkbox"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Notification","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Notifications","required":false}],"incoming":[]},"Opportunity":{"label":"Opportunity","cat":"standard","fieldCount":20,"fields":[{"name":"Advisor_closing_date__c","label":"Fecha cierre asesor","type":"Date"},{"name":"AssociatedAccount__c","label":"Cuenta Asociada","type":"Lookup"},{"name":"Budget_Confirmed__c","label":"Budget Confirmed","type":"Checkbox"},{"name":"ClosingReason__c","label":"Motivo de Cierre","type":"Picklist"},{"name":"ContractId","label":"ContractId","type":"Lookup"},{"name":"DayDateContribution__c","label":"Día fecha aportación","type":"Date"},{"name":"Discovery_Completed__c","label":"Discovery Completed","type":"Checkbox"},{"name":"First_contribution_date__c","label":"Fecha primera aportación","type":"Date"},{"name":"FullId__c","label":"IDCompleto","type":"Text"},{"name":"IsAddendum__c","label":"Es Adenda","type":"Checkbox"},{"name":"LossReason__c","label":"Motivo Pérdida","type":"Picklist"},{"name":"Loss_Reason__c","label":"Loss Reason","type":"Picklist"},{"name":"OwnerId","label":"OwnerId","type":"Lookup"},{"name":"PerfiledOption__c","label":"Opción Perfilada","type":"Picklist"},{"name":"Poorly_classified_motive__c","label":"Motivo mal tipificada","type":"LongTextArea"},{"name":"ROI_Analysis_Completed__c","label":"ROI Analysis Completed","type":"Checkbox"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"StageName","label":"StageName","type":"Picklist"},{"name":"TotalDebtOutstanding__c","label":"Deuda Total Activa Preventa","type":"Currency"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"AssociatedAccount__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Opportunities","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Opportunities","required":false},{"field":"AccountId","relType":"Lookup","referenceTo":"Account","relationshipName":null,"required":false},{"field":"CampaignId","relType":"Lookup","referenceTo":"Campaign","relationshipName":null,"required":false},{"field":"Pricebook2Id","relType":"Lookup","referenceTo":"Pricebook2","relationshipName":null,"required":false}],"incoming":["Contract","CreditDefaultFile__c","History__c","Quote","SalesDebt__c","VoiceCall_Link__c","Case","Lead","EmailMessage"]},"OwnerAssignment__mdt":{"label":"OwnerAssignment","cat":"metadata","fieldCount":6,"fields":[{"name":"Area__c","label":"Área","type":"Picklist"},{"name":"AssignedPublicGroups__c","label":"Public Group Asignados","type":"LongTextArea"},{"name":"AssignedRoles__c","label":"Roles Asignados","type":"TextArea"},{"name":"BusinessHourName__c","label":"Nombre Business Hour","type":"Text"},{"name":"Country__c","label":"País","type":"Picklist"},{"name":"Type__c","label":"Tipo","type":"Picklist"}],"relationships":[],"incoming":[]},"PaymentGatewayInstallment__c":{"label":"PaymentGatewayInstallment","cat":"custom","fieldCount":11,"fields":[{"name":"Amount__c","label":"Cantidad","type":"Number"},{"name":"Concept__c","label":"Concepto","type":"Text"},{"name":"ErrorCode__c","label":"Código de error","type":"Text"},{"name":"ErrorMessage__c","label":"Mensaje de Error","type":"Text"},{"name":"PaymentDate__c","label":"Fecha de cobro","type":"Date"},{"name":"PaymentURL__c","label":"URL Pago","type":"Url"},{"name":"Saving__c","label":"Ahorro","type":"Lookup"},{"name":"Service__c","label":"Servicio","type":"MasterDetail"},{"name":"Status__c","label":"Estado de Cobro","type":"Picklist"},{"name":"UsedToken__c","label":"Token Usado","type":"Lookup"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Saving__c","relType":"Lookup","referenceTo":"Saving__c","relationshipName":"Cuotas_Pasarela_Pago","required":false},{"field":"Service__c","relType":"MasterDetail","referenceTo":"Service__c","relationshipName":"PaymentGatewayInstallment","required":true},{"field":"UsedToken__c","relType":"Lookup","referenceTo":"PaymentGatewayToken__c","relationshipName":"PaymentGatewayInstallment","required":false}],"incoming":[]},"PaymentGatewayToken__c":{"label":"PaymentGatewayToken","cat":"custom","fieldCount":24,"fields":[{"name":"Account__c","label":"Cliente","type":"MasterDetail"},{"name":"Active__c","label":"Pasarela de pago activa","type":"Checkbox"},{"name":"AuthResultCode__c","label":"Resultado autorización","type":"Text"},{"name":"AuthResultMsg__c","label":"Mensaje autorización","type":"Text"},{"name":"AuthorizationURL__c","label":"Autorización URL","type":"Url"},{"name":"CardExpirationMonth__c","label":"Mes de caducidad la tarjeta","type":"Number"},{"name":"CardExpirationYear__c","label":"Año de caducidad la tarjeta","type":"Number"},{"name":"CardInfo__c","label":"Número de la tarjeta","type":"Text"},{"name":"CardName__c","label":"Titular de la tarjeta","type":"Text"},{"name":"CardType__c","label":"Tipo de tarjeta","type":"Text"},{"name":"CorrectedAmount__c","label":"Cantidad corregida","type":"Number"},{"name":"CorrectedAmount_c__c","label":"Cantidad corregida","type":"Currency"},{"name":"DepositAccount__c","label":"Cuenta Ingreso","type":"Picklist"},{"name":"ErrorCode__c","label":"Código de error","type":"Text"},{"name":"ErrorCountThisMonth__c","label":"Errores este mes","type":"Number"},{"name":"ErrorMessage__c","label":"Mensaje de Error","type":"Text"},{"name":"InstallmentErrorNotified__c","label":"Error Cuota Avisado","type":"Checkbox"},{"name":"NextPaymentDate__c","label":"Fecha próximo pago","type":"Date"},{"name":"PayerRef__c","label":"Referencia del pagador","type":"Text"},{"name":"PaymentMethod__c","label":"Payment Method","type":"Text"},{"name":"SRD__c","label":"SRD","type":"Text"},{"name":"TokenCode__c","label":"Token code","type":"Text"},{"name":"Token__c","label":"Token","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"MasterDetail","referenceTo":"Account","relationshipName":"Tokens_Pasarela_Pago","required":true}],"incoming":["PaymentGatewayInstallment__c","Service__c"]},"PaymentIntegrationConfiguration__mdt":{"label":"PaymentIntegrationConfiguration","cat":"metadata","fieldCount":8,"fields":[{"name":"MerchantAccountName__c","label":"Merchant Account Name","type":"Text"},{"name":"MerchantID__c","label":"Merchant ID","type":"Text"},{"name":"RequestMethod__c","label":"Request Method","type":"Text"},{"name":"SharedSecret__c","label":"Shared Secret","type":"Text"},{"name":"URLCallbackSandbox__c","label":"URL Callback","type":"Text"},{"name":"URLCallback__c","label":"URL Callback","type":"Url"},{"name":"URLRequestSandbox__c","label":"URL_Request","type":"Text"},{"name":"URLRequest__c","label":"URL Request","type":"Url"}],"relationships":[],"incoming":[]},"PersonAccount":{"label":"Person Account","cat":"standard","fieldCount":0,"fields":[],"relationships":[{"field":"AccountId","relType":"Lookup","referenceTo":"Account","relationshipName":null,"required":true}],"incoming":[]},"PowerAttorney__c":{"label":"PowerAttorney","cat":"custom","fieldCount":34,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"ApudActaUploadedDate__c","label":"Fecha de Apud Acta Subido","type":"Date"},{"name":"ClaimStatus__c","label":"Estado de reclamadora","type":"Checkbox"},{"name":"CommunicationDate__c","label":"Fecha de comunicación","type":"Date"},{"name":"ContactType__c","label":"Tipo de contacto","type":"Picklist"},{"name":"Cost__c","label":"Coste Poder Notarial","type":"Currency"},{"name":"Date__c","label":"Fecha","type":"Date"},{"name":"DeniedTelematicPower__c","label":"Se Niega Poder Telemático","type":"Checkbox"},{"name":"Denied__c","label":"Denegado","type":"Checkbox"},{"name":"DocumentationStatus__c","label":"Estado documentación","type":"Checkbox"},{"name":"GrantedByClient__c","label":"Otorgado por cliente","type":"Checkbox"},{"name":"HasNewApudActaAuth__c","label":"Autorización Apud Acta nueva","type":"Checkbox"},{"name":"HasNewTelematicApudActa__c","label":"Apud Acta Tel. Nuevo","type":"Checkbox"},{"name":"HasOpenProcedure__c","label":"Procedimiento Abierto","type":"Checkbox"},{"name":"HasTelematicPower__c","label":"Poder Telemático SMD","type":"Checkbox"},{"name":"IsInPersonApudActa__c","label":"Apud Acta Presencial","type":"Checkbox"},{"name":"IsNecesary__c","label":"Necesario","type":"Checkbox"},{"name":"IsValidated__c","label":"Validación","type":"Checkbox"},{"name":"NewApudActaAuthSendDate__c","label":"Fecha envío nueva Autorización Apud Acta","type":"Date"},{"name":"NotaryAppointment__c","label":"Cita notaría","type":"DateTime"},{"name":"NotaryName__c","label":"Nombre notaría","type":"Text"},{"name":"NotaryPhone__c","label":"Teléfono notaría","type":"Phone"},{"name":"Notes__c","label":"Notas","type":"LongTextArea"},{"name":"OpenProcedureType__c","label":"Tipo Procedimiento Abierto","type":"Picklist"},{"name":"SignedAuthorization__c","label":"Autorización firmada","type":"Checkbox"},{"name":"TelematicPowerDate__c","label":"Fecha Poder Telemático","type":"Date"},{"name":"TelematicPowerFlow__c","label":"Flujo Poder Telemático","type":"Picklist"},{"name":"TelematicPowerStatus__c","label":"Estado Poder Telemático","type":"Picklist"},{"name":"UpdatePower__c","label":"Actualizar Poder","type":"Checkbox"},{"name":"Uploaded__c","label":"Subido","type":"Checkbox"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Poderes_Notariales","required":false}],"incoming":[]},"Pricebook2":{"label":"Pricebook","cat":"standard","fieldCount":4,"fields":[{"name":"MaritalStatus__c","label":"Estado civil","type":"Picklist"},{"name":"PerfiledOption__c","label":"Opción Perfilada","type":"Picklist"},{"name":"ProfessionalStatus__c","label":"Situación profesional","type":"Picklist"},{"name":"Type__c","label":"Tipo","type":"Picklist"}],"relationships":[],"incoming":["Opportunity","Contract","PricebookEntry"]},"PricebookEntry":{"label":"Pricebook Entry","cat":"standard","fieldCount":2,"fields":[{"name":"Duration__c","label":"Duración (Meses)","type":"Number"},{"name":"MonthlyPayment__c","label":"Cuota Mensual","type":"Currency"}],"relationships":[{"field":"Pricebook2Id","relType":"MasterDetail","referenceTo":"Pricebook2","relationshipName":null,"required":true},{"field":"Product2Id","relType":"MasterDetail","referenceTo":"Product2","relationshipName":null,"required":true}],"incoming":[]},"Process__c":{"label":"Process","cat":"custom","fieldCount":56,"fields":[{"name":"Agree_amount__c","label":"Cuota acordada","type":"TextArea"},{"name":"AttachedDocuments__c","label":"Documentos Adjuntos","type":"Checkbox"},{"name":"CloseDate__c","label":"Close Date","type":"Date"},{"name":"ClosingReason__c","label":"Motivo de Cierre","type":"Picklist"},{"name":"CompletedPersonalInfo__c","label":"Inf. personal y patrimonial completada","type":"Checkbox"},{"name":"ContactedDate__c","label":"Fecha contactado","type":"Date"},{"name":"Contacted__c","label":"Contactado","type":"Checkbox"},{"name":"ContractDebtDone__c","label":"Deudas con contrato u OH de hacer hecha","type":"Checkbox"},{"name":"CreditorsFilled__c","label":"R. Acreedores","type":"Checkbox"},{"name":"CustomerDocumentationEndDate__c","label":"Fecha de fin Documentación Cliente","type":"Date"},{"name":"CustomerDocumentationStartDate__c","label":"Fecha de inicio Documentación Cliente","type":"Date"},{"name":"CustomerDocumentsAttachedPercent__c","label":"% de Documentos Cliente adjuntados","type":"Percent"},{"name":"DateFirstCallAttempt__c","label":"Fecha intento 1ª llamada","type":"Date"},{"name":"DateFirstSavings__c","label":"Fecha 1er Ahorro","type":"Date"},{"name":"DriveFolderLink__c","label":"Enlace a carpeta Drive","type":"TextArea"},{"name":"FinancialDocumentationEndDate__c","label":"Fecha de fin Documentacion Financiera","type":"Date"},{"name":"FinancialDocumentationStartDate__c","label":"Fecha de inicio Documentación Financiera","type":"Date"},{"name":"FinancialDocumentsAttachedPercent__c","label":"% de Documentos Financiera adjuntados","type":"Percent"},{"name":"FirstCallURL__c","label":"URL Primera llamada","type":"Url"},{"name":"FirstMadeCallDate__c","label":"Fecha 1ª llamada realizada","type":"Date"},{"name":"FirstReScheduledCallDateTime__c","label":"Día y Hora 1ª llamada reagendada","type":"DateTime"},{"name":"FirstScheduledCallDateTime__c","label":"Día y Hora 1ª llamada agendada","type":"DateTime"},{"name":"ForecastCategory__c","label":"Forecast Category","type":"Picklist"},{"name":"InactiveManagerCloseDate__c","label":"Fecha de cierre gestor inactivos","type":"Date"},{"name":"InactivityReason__c","label":"Motivo inactividad","type":"Picklist"},{"name":"LossReason__c","label":"Motivo Pérdida","type":"Picklist"},{"name":"MailSendingDate__c","label":"Fecha envío correo","type":"Date"},{"name":"Notes__c","label":"Notas","type":"LongTextArea"},{"name":"Other_reasons_for_inactivity__c","label":"Otros motivos inactividad","type":"TextArea"},{"name":"PaidAmountInactives__c","label":"Importe Abonado Inactivos","type":"Currency"}],"relationships":[{"field":"RelatedAccount__c","relType":"Lookup","referenceTo":"Account","relationshipName":"ProcessesByAccount","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Processes","required":false}],"incoming":["History__c"]},"Product2":{"label":"Product","cat":"standard","fieldCount":1,"fields":[{"name":"Subtype__c","label":"Subtipo","type":"Picklist"}],"relationships":[],"incoming":["PricebookEntry"]},"Quote":{"label":"Quote","cat":"standard","fieldCount":36,"fields":[{"name":"ActiveDate__c","label":"Fecha activo","type":"DateTime"},{"name":"Active__c","label":"Activo","type":"Checkbox"},{"name":"AddendumOpportunity__c","label":"Oportunidad adenda","type":"Lookup"},{"name":"DebtIdsList__c","label":"Lista Ids Deudas","type":"LongTextArea"},{"name":"Discount__c","label":"% Descuento","type":"Percent"},{"name":"Duration__c","label":"Duración","type":"Number"},{"name":"Fee__c","label":"Honorarios","type":"Percent"},{"name":"FixedFees__c","label":"Honorarios Fijos","type":"Currency"},{"name":"FullID__c","label":"IDCompleto","type":"Text"},{"name":"InitialComission__c","label":"Comisión inicial","type":"Currency"},{"name":"InitialCommissionAddendum__c","label":"Comisión inicial adenda","type":"Currency"},{"name":"LiquidationCommission__c","label":"Comisión Liquidación","type":"Currency"},{"name":"MonthlyPay__c","label":"Cuota Mensual","type":"Currency"},{"name":"NeedsPowerOfAttorney__c","label":"¿Necesita poder notarial?","type":"Checkbox"},{"name":"NumberOfDebts__c","label":"Número de deudas","type":"Number"},{"name":"OthersSales__c","label":"Otros descuentos","type":"Currency"},{"name":"PercentagePaymentBanks__c","label":"Porcentaje Pago Bancos","type":"Percent"},{"name":"PlanCompleted__c","label":"Plan Realizado","type":"Checkbox"},{"name":"PreviousLiquidationPlan__c","label":"Plan de Liquidacion antiguo","type":"Lookup"},{"name":"ProviderInitialFees__c","label":"Honorarios iniciales del proveedor","type":"Currency"},{"name":"ProviderTotalFees__c","label":"Honorarios totales del proveedor","type":"Currency"},{"name":"Saving__c","label":"Ahorro","type":"Currency"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"StartingPay__c","label":"Pago inicial","type":"Currency"},{"name":"TotalActiveDebts__c","label":"Total Deudas Activas","type":"Currency"},{"name":"TotalAdendaDebt__c","label":"Cantidad deuda agregada","type":"Currency"},{"name":"TotalComissions__c","label":"Total Comisiones","type":"Currency"},{"name":"TotalInactiveDebts__c","label":"Cantidad deuda eliminada","type":"Currency"},{"name":"TotalLsoFee__c","label":"Total Honorarios LSO","type":"Currency"},{"name":"TotalMonthlyCommissions__c","label":"Total comisiones mensuales","type":"Currency"}],"relationships":[{"field":"AddendumOpportunity__c","relType":"Lookup","referenceTo":"Opportunity","relationshipName":"OportunidadAdenda","required":false},{"field":"PreviousLiquidationPlan__c","relType":"Lookup","referenceTo":"Quote","relationshipName":"PlanesAnteriores","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Quote","required":false},{"field":"OpportunityId","relType":"Lookup","referenceTo":"Opportunity","relationshipName":null,"required":false}],"incoming":["Quote","Service__c"]},"RoundRobinLastAssignment__c":{"label":"RoundRobinLastAssignment","cat":"custom","fieldCount":1,"fields":[{"name":"LastUserId__c","label":"Id Último Usuario","type":"Text"}],"relationships":[],"incoming":[]},"SalesDebt__c":{"label":"SalesDebt","cat":"custom","fieldCount":34,"fields":[{"name":"AddendumOpportunity__c","label":"Modificado por Adenda","type":"Lookup"},{"name":"AmountModifiedOnAddendum__c","label":"Cantidad Modificada en Adenda","type":"Checkbox"},{"name":"Amount__c","label":"Cantidad","type":"Currency"},{"name":"BurofaxDate__c","label":"Fecha Burofax","type":"Date"},{"name":"Contract__c","label":"Contract","type":"Lookup"},{"name":"CreditNumber__c","label":"Nº de Crédito","type":"Text"},{"name":"DebtNumberNumeric__c","label":"Número de deuda","type":"Number"},{"name":"DebtNumber__c","label":"Número de Deuda (link)","type":"Text"},{"name":"Debt_IsActive__c","label":"Deuda Activa","type":"Checkbox"},{"name":"Debt_Name__c","label":"Nombre de la Deuda","type":"Text"},{"name":"Debt_RecordTypeName__c","label":"Deuda RecordType","type":"Text"},{"name":"Debt_Settled__c","label":"Liquidado","type":"Checkbox"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Debt_claim_status__c","label":"Estado reclamación deuda","type":"Text"},{"name":"Entity__c","label":"Entidad","type":"Text"},{"name":"HasContract__c","label":"¿Tiene contrato?","type":"Checkbox"},{"name":"IsActive__c","label":"Activo en Servicio","type":"Checkbox"},{"name":"IsHolder__c","label":"¿Es Titular?","type":"Checkbox"},{"name":"MonthlyFee__c","label":"Cuota Mensual","type":"Currency"},{"name":"Opportunity__c","label":"Venta","type":"Lookup"},{"name":"Origin__c","label":"Origen","type":"Text"},{"name":"PreviousEntity__c","label":"Entidad anterior","type":"Text"},{"name":"ProductEntity__c","label":"Producto","type":"Text"},{"name":"RemovedOnAddendum__c","label":"Eliminado en Adenda","type":"Checkbox"},{"name":"SalesDebtAmount__c","label":"Cantidad Venta Deuda","type":"Currency"},{"name":"ServiceNOutstandingInstalments__c","label":"Nª de cuotas pendientes","type":"Number"},{"name":"ServiceProvisionDateCompleted__c","label":"Fecha provisión completada","type":"Date"},{"name":"ServiceRecordType__c","label":"Tipo de registro del servicio","type":"Text"},{"name":"ServiceStatus__c","label":"Estado del servicio","type":"Text"},{"name":"Service__c","label":"Servicio","type":"Lookup"}],"relationships":[{"field":"AddendumOpportunity__c","relType":"Lookup","referenceTo":"Opportunity","relationshipName":"Deudas_Modificadas_por_Adenda","required":false},{"field":"Contract__c","relType":"Lookup","referenceTo":"Contract","relationshipName":"SalesDebt","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"SalesDebt","required":false},{"field":"Opportunity__c","relType":"Lookup","referenceTo":"Opportunity","relationshipName":"Deudas_de_la_venta","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Deudas_de_la_venta","required":false}],"incoming":[]},"Saving__c":{"label":"Saving","cat":"custom","fieldCount":39,"fields":[{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"Balance__c","label":"Saldo","type":"Currency"},{"name":"BankHolded__c","label":"Banco Holded","type":"Lookup"},{"name":"CorrectedInvoiceNumber__c","label":"Nº de Factura rectificadas","type":"Text"},{"name":"DateSavingAndCreation__c","label":"Fecha Ahorro + Creación","type":"Text"},{"name":"DateSaving__c","label":"Fecha Ahorro","type":"Date"},{"name":"DebtSettlement__c","label":"Liquidación deuda","type":"Lookup"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Description__c","label":"Descripción","type":"LongTextArea"},{"name":"DoYouHaveEnoughSavings__c","label":"¿Tiene Ahorro suficiente?","type":"Checkbox"},{"name":"Entity__c","label":"Entidad","type":"Lookup"},{"name":"HoldedId__c","label":"Id de Holded","type":"Text"},{"name":"InitialCommission__c","label":"Comisión Inicial","type":"Currency"},{"name":"InvoicedDate__c","label":"Fecha facturación","type":"Date"},{"name":"Invoiced__c","label":"Facturado","type":"Checkbox"},{"name":"Lawsuit__c","label":"Demanda","type":"Lookup"},{"name":"ModificationComments__c","label":"Comentarios de modificación","type":"LongTextArea"},{"name":"MonthlyCommission__c","label":"Comisión Mensual","type":"Currency"},{"name":"MonthlyContribution__c","label":"Aportación Mensual","type":"Currency"},{"name":"PaymentGateway__c","label":"¿Pago por Pasarela?","type":"Checkbox"},{"name":"Provision__c","label":"Provisión","type":"Currency"},{"name":"RealTotalSaving__c","label":"Total Ahorro Real","type":"Currency"},{"name":"Reconversion_service__c","label":"Servicio de reconversión","type":"Lookup"},{"name":"RecordTypeLabel__c","label":"Record Type Label","type":"Text"},{"name":"RectifiedEntryDate__c","label":"Fecha asiento rectificado","type":"Date"},{"name":"SMD_Fees_Adjustment__c","label":"Rectificacion Honorarios SMD","type":"Currency"},{"name":"SMD_Fees__c","label":"Honorarios SMD","type":"Currency"},{"name":"Service__c","label":"Servicio","type":"MasterDetail"},{"name":"SettlementCommission__c","label":"Comisión Liquidación","type":"Currency"},{"name":"Settlement__c","label":"Liquidación","type":"Currency"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Ahorros","required":false},{"field":"BankHolded__c","relType":"Lookup","referenceTo":"BankHolded__c","relationshipName":"Ahorros","required":false},{"field":"DebtSettlement__c","relType":"Lookup","referenceTo":"DebtSettlement__c","relationshipName":"Ahorros","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Ahorros","required":false},{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Ahorros","required":false},{"field":"Lawsuit__c","relType":"Lookup","referenceTo":"Lawsuit__c","relationshipName":"Ahorros","required":false},{"field":"Reconversion_service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Ahorros1","required":false},{"field":"Service__c","relType":"MasterDetail","referenceTo":"Service__c","relationshipName":"Ahorros","required":true},{"field":"TicketHolded__c","relType":"Lookup","referenceTo":"TicketHolded__c","relationshipName":"Ahorros","required":false}],"incoming":["Billing__c","History__c","PaymentGatewayInstallment__c","Vendor_Payment__c"]},"Service__c":{"label":"Service","cat":"custom","fieldCount":92,"fields":[{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"ActiveUntil__c","label":"Activo hasta","type":"Date"},{"name":"AssignedSeniorManager__c","label":"Gestor senior asignado","type":"Lookup"},{"name":"AssociatedAccount__c","label":"Cuenta Asociada","type":"Lookup"},{"name":"AverageMonthlyContribution3Months__c","label":"Promedio Aportaciones Mensuales 3 Meses","type":"Number"},{"name":"Certificate__c","label":"Certificado","type":"Checkbox"},{"name":"CompliancePercentageRecoverys__c","label":"% Cumplimiento plan + recuperos","type":"Percent"},{"name":"Contract__c","label":"Contrato","type":"Lookup"},{"name":"ContributionsPaid__c","label":"Aportaciones Abonadas","type":"Summary"},{"name":"ConversionProposal__c","label":"Propuesta conversión","type":"Picklist"},{"name":"ConversionService__c","label":"Servicio reconversión","type":"Lookup"},{"name":"DateFixedFeesCompleted__c","label":"Fecha Honorarios Fijos Completados","type":"Date"},{"name":"DateGraduated__c","label":"Fecha Graduado","type":"Date"},{"name":"DateLastContact__c","label":"Fecha último contacto cliente","type":"Date"},{"name":"DateNotificationNonPayment__c","label":"Fecha aviso impago","type":"Date"},{"name":"DateOfFirstContribution__c","label":"Fecha Primera Contribución","type":"Summary"},{"name":"Date_of_graduate_review__c","label":"Fecha revisión graduado","type":"Date"},{"name":"Date_of_receipt_of_payment__c","label":"fecha comprobante de pago recibido","type":"Date"},{"name":"Date_review_Google__c","label":"Fecha review Google","type":"Date"},{"name":"Date_review_Trustpilot__c","label":"Fecha review Trustpilot","type":"Date"},{"name":"Days_until_leave__c","label":"Días hasta baja","type":"Number"},{"name":"Document_stage__c","label":"Etapa documento","type":"Picklist"},{"name":"Email__c","label":"correo","type":"Checkbox"},{"name":"FixedFees__c","label":"Honorarios Fijos","type":"Currency"},{"name":"Formulario_conversion_LSO__c","label":"Formulario conversión LSO","type":"Date"},{"name":"FullId__c","label":"IDCompleto","type":"Text"},{"name":"GatewayBlockedUntil__c","label":"Pasarela bloqueada hasta","type":"Date"},{"name":"HasSufficientSavings__c","label":"¿Tiene Ahorro suficiente?","type":"Checkbox"},{"name":"IsDummy__c","label":"Is Dummy","type":"Checkbox"},{"name":"JointInsolvencyProceedings__c","label":"Concursos de acreedores conjunto","type":"Checkbox"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Servicios","required":false},{"field":"AssignedSeniorManager__c","relType":"Lookup","referenceTo":"User","relationshipName":"Services","required":false},{"field":"AssociatedAccount__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Servicios1","required":false},{"field":"Contract__c","relType":"Lookup","referenceTo":"Contract","relationshipName":"Servicios","required":false},{"field":"ConversionService__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Servicios","required":false},{"field":"ManagerAssigned__c","relType":"Lookup","referenceTo":"User","relationshipName":"Servicios","required":false},{"field":"PaymentGatewayToken__c","relType":"Lookup","referenceTo":"PaymentGatewayToken__c","relationshipName":"Servicios","required":false},{"field":"Quote__c","relType":"Lookup","referenceTo":"Quote","relationshipName":"Services","required":false}],"incoming":["AccountInfo__c","Bankruptcy__c","Billing__c","Burofax__c","Contract","CreditDefaultFile__c","DebtSettlement__c","DeletedJournalEntry__c","History__c","LSODocument__c","Lawsuit__c","Opportunity","PaymentGatewayInstallment__c","Process__c","Quote","SalesDebt__c","Saving__c","Service__c","TicketHolded__c","Transfer__c","Vendor_Payment__c","VoiceCall_Link__c"]},"SettlementInstallment__c":{"label":"SettlementInstallment","cat":"custom","fieldCount":10,"fields":[{"name":"Amount__c","label":"Cantidad","type":"Number"},{"name":"DebtSettlement__c","label":"Liquidación de la deuda","type":"MasterDetail"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"InstallmentAmount__c","label":"Importe de la cuota","type":"Currency"},{"name":"InstallmentsNumber__c","label":"Número de cuotas","type":"Number"},{"name":"IsDummy__c","label":"Is Dummy","type":"Checkbox"},{"name":"LostSettlement__c","label":"Liquidación Perdida","type":"Checkbox"},{"name":"Order__c","label":"Orden","type":"Number"},{"name":"RemainingInstallments__c","label":"Número de cuotas restantes","type":"Number"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"DebtSettlement__c","relType":"MasterDetail","referenceTo":"DebtSettlement__c","relationshipName":"Installments","required":true},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Cuotas_Liquidaciones","required":false}],"incoming":["SettlementPayment__c"]},"SettlementPayment__c":{"label":"SettlementPayment","cat":"custom","fieldCount":7,"fields":[{"name":"Amount__c","label":"Cantidad","type":"Currency"},{"name":"Date__c","label":"Fecha","type":"Date"},{"name":"DebtSettlement__c","label":"Liquidación deuda","type":"MasterDetail"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Order__c","label":"Orden","type":"Number"},{"name":"SettlementInstallment__c","label":"Cuota Liquidación","type":"MasterDetail"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"DebtSettlement__c","relType":"MasterDetail","referenceTo":"DebtSettlement__c","relationshipName":"Payment","required":true},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"Pagos_Cuotas_Liquidaciones","required":false},{"field":"SettlementInstallment__c","relType":"MasterDetail","referenceTo":"SettlementInstallment__c","relationshipName":"SettlementPayment","required":true}],"incoming":[]},"SignaturitContracts__mdt":{"label":"SignaturitContracts","cat":"metadata","fieldCount":16,"fields":[{"name":"EmailTemplate__c","label":"Email Template","type":"Text"},{"name":"ExpirationDays__c","label":"ExpirationDays","type":"Number"},{"name":"IsAddendum__c","label":"¿Es Adenda?","type":"Checkbox"},{"name":"Label__c","label":"Label","type":"Text"},{"name":"MaritalStatus__c","label":"Estado civil","type":"Text"},{"name":"OrderSheet__c","label":"Hoja de encargo","type":"Text"},{"name":"ProductFamily__c","label":"Familia Producto","type":"Text"},{"name":"ProductSubtype__c","label":"Subtipo Producto","type":"Text"},{"name":"ProfessionalStatus__c","label":"Situación profesional","type":"Text"},{"name":"RecordTypeDeveloperName__c","label":"RecordTypeDeveloperName","type":"Text"},{"name":"ReminderDays__c","label":"ReminderDays","type":"Number"},{"name":"ReminderType__c","label":"ReminderType","type":"Text"},{"name":"SignType__c","label":"SignType","type":"Text"},{"name":"SignatureType__c","label":"SignatureType","type":"Text"},{"name":"SourceObject__c","label":"Source Object","type":"Text"},{"name":"VisualForceName__c","label":"Nombre VisualForce","type":"Text"}],"relationships":[],"incoming":[]},"SpanishBank__mdt":{"label":"SpanishBank","cat":"metadata","fieldCount":7,"fields":[{"name":"ConsumerCredit1to5__c","label":"Créditos al consumo de 1 a 5","type":"Number"},{"name":"ConsumerCreditMoreto5__c","label":"Créditos al consumo + de 5","type":"Number"},{"name":"ConsumerCreditYear__c","label":"Créditos al consumo hasta 1 año","type":"Number"},{"name":"Month__c","label":"Mes","type":"Number"},{"name":"RegisterId__c","label":"Id registro","type":"Text"},{"name":"Revolving__c","label":"I. Revolving","type":"Number"},{"name":"Year__c","label":"Año","type":"Number"}],"relationships":[],"incoming":[]},"StageTransition__mdt":{"label":"StageTransition","cat":"metadata","fieldCount":7,"fields":[{"name":"CurrentStage__c","label":"CurrentStage","type":"Text"},{"name":"ErrorMessage__c","label":"Mensaje error","type":"Text"},{"name":"NextStage__c","label":"NextStage","type":"Text"},{"name":"ObjectApiName__c","label":"ObjectApiName","type":"Text"},{"name":"ProfilePermission__c","label":"ProfilePermission","type":"Text"},{"name":"StageFieldName__c","label":"StageFieldName","type":"Text"},{"name":"SuccessMessage__c","label":"Mensaje éxito","type":"Text"}],"relationships":[],"incoming":[]},"Task__mdt":{"label":"Task","cat":"metadata","fieldCount":6,"fields":[{"name":"Description__c","label":"Description","type":"LongTextArea"},{"name":"PeriodDays__c","label":"Period (Days from today)","type":"Number"},{"name":"Priority__c","label":"Priority","type":"Text"},{"name":"Status__c","label":"Status","type":"Text"},{"name":"Subject__c","label":"Subject","type":"Text"},{"name":"Type__c","label":"Type","type":"Text"}],"relationships":[],"incoming":[]},"TemplateWhatsappMetadata__mdt":{"label":"TemplateWhatsappMetadata","cat":"metadata","fieldCount":2,"fields":[{"name":"PicklistMetadata__c","label":"PicklistMetadata","type":"Picklist"},{"name":"templateContent__c","label":"Template Content","type":"Text"}],"relationships":[],"incoming":[]},"TicketHolded__c":{"label":"TicketHolded","cat":"custom","fieldCount":6,"fields":[{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"Amount__c","label":"Cantidad","type":"Currency"},{"name":"Contract__c","label":"Contrato","type":"Lookup"},{"name":"DueDate__c","label":"Fecha Vencimiento","type":"Date"},{"name":"HoldedId__c","label":"Id de Holded","type":"Text"},{"name":"Service__c","label":"Servicio","type":"MasterDetail"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"TicketsHolded","required":false},{"field":"Contract__c","relType":"Lookup","referenceTo":"Contract","relationshipName":"TicketsHolded","required":false},{"field":"Service__c","relType":"MasterDetail","referenceTo":"Service__c","relationshipName":"TicketsHolded","required":true}],"incoming":["Saving__c"]},"TransferType__mdt":{"label":"TransferType","cat":"metadata","fieldCount":4,"fields":[{"name":"CertificateRequired__c","label":"Necesita certificado","type":"Checkbox"},{"name":"SourceAccount__c","label":"Cuenta de origen","type":"Picklist"},{"name":"TransferDescription__c","label":"Concepto de transferencia","type":"TextArea"},{"name":"TransferType__c","label":"Tipo de transferencia","type":"Picklist"}],"relationships":[],"incoming":[]},"Transfer__c":{"label":"Transfer","cat":"custom","fieldCount":13,"fields":[{"name":"Account__c","label":"Cuenta","type":"Lookup"},{"name":"Amount__c","label":"Importe","type":"Currency"},{"name":"BankAccount__c","label":"Cuenta bancaria","type":"Text"},{"name":"Beneficiary__c","label":"Beneficiario","type":"Text"},{"name":"IBAN__c","label":"IBAN","type":"Text"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"SourceAccount__c","label":"Cuenta de origen","type":"Picklist"},{"name":"SpanishAccount__c","label":"Cuenta española","type":"Checkbox"},{"name":"TransferCompleted__c","label":"Transferencia realizada","type":"Checkbox"},{"name":"TransferDate__c","label":"Fecha de transferencia realizada","type":"Date"},{"name":"TransferDescription__c","label":"Concepto de transferencia","type":"TextArea"},{"name":"TransferType__c","label":"Tipo de transferencia","type":"Picklist"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Transferencias","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Transferencias","required":false}],"incoming":[]},"User":{"label":"User","cat":"standard","fieldCount":9,"fields":[{"name":"CollaboratorLicense__c","label":"Licencia Colaboradores","type":"MultiselectPicklist"},{"name":"EndVacations__c","label":"Fin Vacaciones","type":"DateTime"},{"name":"IsOnVacation__c","label":"¿Está de Vacaciones?","type":"Checkbox"},{"name":"KmaleonCode__c","label":"Código Kmaleon","type":"Number"},{"name":"NameAndRole__c","label":"Nombre y Rol","type":"Text"},{"name":"StartVacations__c","label":"Comienzo Vacaciones","type":"DateTime"},{"name":"SubstituteManagers__c","label":"Gestores Sustitutos","type":"TextArea"},{"name":"oldId__c","label":"oldId","type":"Text"},{"name":"signaturit__SIG_SignaturitToken__c","label":"API key","type":"Text"}],"relationships":[],"incoming":["Account","Bankruptcy__c","Case","ContentVersion","CreditDefaultFileAnalysis__c","DebtCaseReasonCatalog__c","DebtSettlement__c","Debt__c","Entity__c","History__c","LSODocument__c","Lawsuit__c","Lead","Service__c","signaturit__SignatureRequestSigner__c","VoiceCall"]},"Vendor_Config__mdt":{"label":"Vendor Config","cat":"metadata","fieldCount":4,"fields":[{"name":"Concept__c","label":"Concepto","type":"Text"},{"name":"Default__c","label":"Default","type":"Checkbox"},{"name":"Email__c","label":"Email","type":"Email"},{"name":"IBAN__c","label":"IBAN","type":"Text"}],"relationships":[],"incoming":[]},"Vendor_Payment__c":{"label":"Vendor Payment","cat":"custom","fieldCount":11,"fields":[{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"Amount__c","label":"Importe","type":"Currency"},{"name":"Bankruptcy__c","label":"Concurso presentado","type":"Lookup"},{"name":"Concept__c","label":"Concepto","type":"Text"},{"name":"IBAN__c","label":"IBAN","type":"Text"},{"name":"Payment_Date__c","label":"Fecha Pago","type":"Date"},{"name":"Payment_Type__c","label":"Tipo de pago","type":"Picklist"},{"name":"Saving__c","label":"Ahorro","type":"Lookup"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"Transfer_Completed__c","label":"Transferencia realizada","type":"Checkbox"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Vendor_Payments","required":false},{"field":"Bankruptcy__c","relType":"Lookup","referenceTo":"Bankruptcy__c","relationshipName":"Vendor_Payments","required":false},{"field":"Saving__c","relType":"Lookup","referenceTo":"Saving__c","relationshipName":"Vendor_Payments","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"Vendor_Payments","required":false}],"incoming":[]},"VoiceCall":{"label":"Voice Call","cat":"standard","fieldCount":25,"fields":[{"name":"Account__c","label":"Account","type":"Lookup"},{"name":"Actions__c","label":"Acciones o compromisos","type":"Text"},{"name":"ActivityId","label":"ActivityId","type":"Lookup"},{"name":"CallAnswered__c","label":"Llamada Atendida","type":"Checkbox"},{"name":"CallResolution","label":"CallResolution","type":"Picklist"},{"name":"CallerId","label":"CallerId","type":"Lookup"},{"name":"ClientIntention__c","label":"Intención principal del cliente","type":"Picklist"},{"name":"ConversationURL__c","label":"Conversation URL","type":"Text"},{"name":"DateCallAnswered__c","label":"Fecha Llamada Atendida","type":"DateTime"},{"name":"Dialer__c","label":"Dialer","type":"Checkbox"},{"name":"EndUserId","label":"EndUserId","type":"Lookup"},{"name":"GlobalSentiment__c","label":"Sentimiento global de la llamada","type":"Picklist"},{"name":"Keywords__c","label":"Palabras clave relevantes","type":"Text"},{"name":"NextCallId","label":"NextCallId","type":"Lookup"},{"name":"OwnerId","label":"OwnerId","type":"Lookup"},{"name":"PhrasesFrictionConflict__c","label":"Frases de fricción o conflicto detectada","type":"Text"},{"name":"PreviousCallId","label":"PreviousCallId","type":"Lookup"},{"name":"ReasonCall__c","label":"Motivo Llamada","type":"MultiselectPicklist"},{"name":"RecipientId","label":"RecipientId","type":"Lookup"},{"name":"RelatedRecordId","label":"RelatedRecordId","type":"Lookup"},{"name":"Summary__c","label":"Resumen de la llamada","type":"Text"},{"name":"TimeAgentClient__c","label":"Tiempo estimado de participación cliente","type":"Text"},{"name":"UserId","label":"UserId","type":"Lookup"},{"name":"genesysps__Last_utterance__c","label":"Last utterance","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Voice_Calls","required":false},{"field":"OwnerId","relType":"Lookup","referenceTo":"User","relationshipName":null,"required":false}],"incoming":["VoiceCall_Link__c"]},"VoiceCall_Link__c":{"label":"VoiceCall Link","cat":"custom","fieldCount":9,"fields":[{"name":"Account__c","label":"Cliente","type":"Lookup"},{"name":"Bankruptcy__c","label":"Concurso","type":"Lookup"},{"name":"Debt__c","label":"Deuda","type":"Lookup"},{"name":"Debtsettlement__c","label":"Liquidación","type":"Lookup"},{"name":"Lawsuit__c","label":"Demanda","type":"Lookup"},{"name":"Lead__c","label":"Lead","type":"Lookup"},{"name":"Opportunity__c","label":"Oportunidad","type":"Lookup"},{"name":"Service__c","label":"Servicio","type":"Lookup"},{"name":"Voice_Call__c","label":"Voice Call","type":"MasterDetail"}],"relationships":[{"field":"Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"VoiceCall_Links","required":false},{"field":"Bankruptcy__c","relType":"Lookup","referenceTo":"Bankruptcy__c","relationshipName":"VoiceCall_Links","required":false},{"field":"Debt__c","relType":"Lookup","referenceTo":"Debt__c","relationshipName":"VoiceCall_Links","required":false},{"field":"Debtsettlement__c","relType":"Lookup","referenceTo":"DebtSettlement__c","relationshipName":"VoiceCall_Links","required":false},{"field":"Lawsuit__c","relType":"Lookup","referenceTo":"Lawsuit__c","relationshipName":"VoiceCall_Links","required":false},{"field":"Lead__c","relType":"Lookup","referenceTo":"Lead","relationshipName":"VoiceCall_Links","required":false},{"field":"Opportunity__c","relType":"Lookup","referenceTo":"Opportunity","relationshipName":"VoiceCall_Links","required":false},{"field":"Service__c","relType":"Lookup","referenceTo":"Service__c","relationshipName":"VoiceCall_Links","required":false},{"field":"Voice_Call__c","relType":"MasterDetail","referenceTo":"VoiceCall","relationshipName":"VoiceCall_Links","required":true}],"incoming":[]},"WhatsApp_Queue_Mapping__mdt":{"label":"WhatsApp Queue Mapping","cat":"metadata","fieldCount":3,"fields":[{"name":"Queue_Developer_Name__c","label":"Queue Developer Name","type":"Text"},{"name":"Queue_Name__c","label":"Queue Name","type":"Text"},{"name":"Role_Name__c","label":"Role Name","type":"Text"}],"relationships":[],"incoming":[]},"signaturit__SignatureRequestFile__c":{"label":"SignatureRequestFile","cat":"custom","fieldCount":14,"fields":[{"name":"SignIdApi__c","label":"Signaturit API Signer File","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"},{"name":"signaturit__FileId__c","label":"File Id","type":"Text"},{"name":"signaturit__FileName__c","label":"File Name","type":"Text"},{"name":"signaturit__LongName__c","label":"Long name","type":"TextArea"},{"name":"signaturit__NumberCancelDocuments__c","label":"Number Cancel Signatures","type":"Summary"},{"name":"signaturit__NumberCompletedDocuments__c","label":"Number Completed Signatures","type":"Summary"},{"name":"signaturit__NumberDocuments__c","label":"Number Signatures","type":"Summary"},{"name":"signaturit__OrderIndex__c","label":"Order Index","type":"Number"},{"name":"signaturit__Origin__c","label":"Origin","type":"Picklist"},{"name":"signaturit__Progress__c","label":"Progress","type":"Text"},{"name":"signaturit__SignatureRequest__c","label":"Signature Request","type":"MasterDetail"},{"name":"signaturit__TemplateName__c","label":"Template Name","type":"Text"},{"name":"signaturit__Type__c","label":"Type","type":"Picklist"}],"relationships":[{"field":"signaturit__SignatureRequest__c","relType":"MasterDetail","referenceTo":"signaturit__SignatureRequest__c","relationshipName":"SignatureRequestFile","required":true}],"incoming":[]},"signaturit__SignatureRequestSigner__c":{"label":"SignatureRequestSigner","cat":"custom","fieldCount":17,"fields":[{"name":"DNI__c","label":"DNI","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"},{"name":"signaturit__Account__c","label":"Account","type":"Lookup"},{"name":"signaturit__Contact__c","label":"Contact","type":"Lookup"},{"name":"signaturit__Email__c","label":"Email","type":"Email"},{"name":"signaturit__Lead__c","label":"Lead","type":"Lookup"},{"name":"signaturit__LongName__c","label":"Long name","type":"TextArea"},{"name":"signaturit__Name__c","label":"Name","type":"Text"},{"name":"signaturit__NumberCancelDocuments__c","label":"Number Cancel Signatures","type":"Summary"},{"name":"signaturit__NumberCompletedDocuments__c","label":"Number Completed Signatures","type":"Summary"},{"name":"signaturit__NumberDocuments__c","label":"Number Signatures","type":"Summary"},{"name":"signaturit__Phone__c","label":"Phone","type":"Phone"},{"name":"signaturit__Progress__c","label":"Progress","type":"Text"},{"name":"signaturit__SignatureRequest__c","label":"Signature Request","type":"MasterDetail"},{"name":"signaturit__SignerName__c","label":"Signer Name","type":"Text"},{"name":"signaturit__Type__c","label":"Type","type":"Picklist"},{"name":"signaturit__User__c","label":"User","type":"Lookup"}],"relationships":[{"field":"signaturit__Account__c","relType":"Lookup","referenceTo":"Account","relationshipName":"Signature_Request_signers","required":false},{"field":"signaturit__Contact__c","relType":"Lookup","referenceTo":"Contact","relationshipName":"SignatureRequestSigners","required":false},{"field":"signaturit__Lead__c","relType":"Lookup","referenceTo":"Lead","relationshipName":"SignatureRequestSigners","required":false},{"field":"signaturit__SignatureRequest__c","relType":"MasterDetail","referenceTo":"signaturit__SignatureRequest__c","relationshipName":"SignatureRequestSigners","required":true},{"field":"signaturit__User__c","relType":"Lookup","referenceTo":"User","relationshipName":"SignatureRequestSigners","required":false}],"incoming":[]},"signaturit__SignatureRequest__c":{"label":"SignatureRequest","cat":"custom","fieldCount":52,"fields":[{"name":"DownloadedAuditTrail__c","label":"AuditTrail descargado","type":"Checkbox"},{"name":"DownloadedSignedDocment__c","label":"Descargado documento firmado","type":"Checkbox"},{"name":"EmailType__c","label":"Tipo email","type":"Picklist"},{"name":"Entity__c","label":"Entidad Bancaria","type":"Lookup"},{"name":"Error__c","label":"Error","type":"Text"},{"name":"InactiveCustomer__c","label":"Inactive Customer","type":"Checkbox"},{"name":"NullSignaturitId__c","label":"¿Nulo signaturit Id?","type":"Checkbox"},{"name":"NumberPages__c","label":"Número de páginas","type":"Number"},{"name":"TypeSign__c","label":"Tipo de firma","type":"Text"},{"name":"oldId__c","label":"oldId","type":"Text"},{"name":"signaturit__APIError__c","label":"API error","type":"LongTextArea"},{"name":"signaturit__Archive_Content__c","label":"Archive Content","type":"Html"},{"name":"signaturit__Attachment_Name__c","label":"Attachment Name","type":"Text"},{"name":"signaturit__Attachment_Type__c","label":"Attachment Type","type":"Picklist"},{"name":"signaturit__AuditTrailAttachmentId__c","label":"Audit Trail Attachment Id","type":"Text"},{"name":"signaturit__AuditTrailURL__c","label":"Audit Trail","type":"Text"},{"name":"signaturit__BackToParent__c","label":"Back to parent","type":"Text"},{"name":"signaturit__Body__c","label":"Body","type":"TextArea"},{"name":"signaturit__CompletedCount__c","label":"Completed Count","type":"Number"},{"name":"signaturit__Contact__c","label":"Contact","type":"Lookup"},{"name":"signaturit__CreatedMonthYear__c","label":"Created Month Year","type":"Text"},{"name":"signaturit__Debug__c","label":"Debug log","type":"LongTextArea"},{"name":"signaturit__DeliveryType__c","label":"Delivery Type","type":"Picklist"},{"name":"signaturit__EndUserError__c","label":"End User Error","type":"LongTextArea"},{"name":"signaturit__Expiration__c","label":"Expiration","type":"Picklist"},{"name":"signaturit__Expiration_date__c","label":"Days to Expiration","type":"Number"},{"name":"signaturit__Last_Status_Date__c","label":"Last Status Date","type":"DateTime"},{"name":"signaturit__MinutesToSign__c","label":"Minutes To Sign","type":"Number"},{"name":"signaturit__NumberCancelDocuments__c","label":"Number Cancel Signatures","type":"Summary"},{"name":"signaturit__NumberCompletedDocuments__c","label":"Number Completed Signatures","type":"Summary"}],"relationships":[{"field":"Entity__c","relType":"Lookup","referenceTo":"Entity__c","relationshipName":"Signature_Requests","required":false},{"field":"signaturit__Contact__c","relType":"Lookup","referenceTo":"Contact","relationshipName":"Signature_requests","required":false}],"incoming":["signaturit__SignatureRequestFile__c","signaturit__SignatureRequestSigner__c"]},"signaturit__SignatureSignerFile__c":{"label":"SignatureSignerFile","cat":"custom","fieldCount":1,"fields":[{"name":"oldId__c","label":"oldId","type":"Text"}],"relationships":[],"incoming":[]}},"context":"SCHEMA ELCANO SALESFORCE — 102 objetos, 209 relaciones\n\nAWS_Connect_Settings__mdt (metadata) | AWS Connect Settings\nAccount (standard) | Account (Cliente) → User(Lookup), Entity__c(Lookup), Entity__c(Lookup), Entity__c(Lookup), Entity__c(Lookup)\nAccountInfo__c (custom) | AccountInfo → Account(Lookup), Service__c(Lookup)\nAccountRelationship__c (custom) | AccountRelationship → Account(Lookup), Account(Lookup), AccountRelationship__c(Lookup)\nActivity (standard) | Activity\nBankCIF__mdt (metadata) | BankCIF\nBankHolded__c (custom) | BankHolded\nBankruptcy__c (custom) | Bankruptcy → Account(Lookup), Account(Lookup), User(Lookup), Service__c(Lookup)\nBatch_settings__mdt (metadata) | Batch settings\nBilling__c (custom) | Billing → Account(Lookup), Saving__c(Lookup), Service__c(Lookup)\nBurofax__c (custom) | Burofax → Account(Lookup), Debt__c(Lookup), Entity__c(Lookup), EntityProduct__c(Lookup), Service__c(Lookup)\nCampaign (standard) | Campaign\nCampaignMember (standard) | Campaign Member → Campaign(MasterDetail), Lead(Lookup)\nCase (standard) | Case → DebtCaseReasonCatalog__c(Lookup), Debt__c(Lookup), DebtCaseDepartment__c(Lookup), DebtCaseDepartment__c(Lookup), User(Lookup)\nCatchmentSourceEquivalence__c (custom) | CatchmentSourceEquivalence\nCertifiedEmailFiles__c (custom) | CertifiedEmailFiles → CertifiedEmail__c(Lookup)\nCertifiedEmail__c (custom) | CertifiedEmail → Burofax__c(Lookup)\nClient_Profile_Change__c (custom) | Client Profile Change → AccountInfo__c(Lookup), Account(Lookup)\nCommunityGate__mdt (metadata) | CommunityGate\nConfiguracion_Slots_Disponibles__mdt (metadata) | Configuracion Slots Disponibles\nContentDocument (standard) | ContentDocument\nContentVersion (standard) | ContentVersion → Entity__c(Lookup), User(Lookup), ContentDocument(MasterDetail)\nContract (standard) | Contract → Opportunity(Lookup), Service__c(Lookup), Account(Lookup), Pricebook2(Lookup)\nContractAccount__c (custom) | ContractAccount → Account(Lookup), Contract(Lookup)\nConversation_History__b (bigobject) | Conversation History\nCreditDefaultFileAnalysis__c (custom) | CreditDefaultFileAnalysis → User(Lookup), CreditDefaultFile__c(MasterDetail), Debt__c(Lookup), Entity__c(Lookup), Entity__c(Lookup)\nCreditDefaultFileResult__e (event) | CreditDefaultFileResult\nCreditDefaultFile__c (custom) | CreditDefaultFile → Account(Lookup), Opportunity(Lookup), Service__c(Lookup)\nDebtCaseDepartment__c (custom) | DebtCaseDepartment\nDebtCaseReasonCatalog__c (custom) | DebtCaseReasonCatalog → DebtCaseTypeCatalog__c(MasterDetail), DebtCaseDepartment__c(Lookup), User(Lookup)\nDebtCaseTypeCatalog__c (custom) | DebtCaseTypeCatalog → DebtCaseDepartment__c(MasterDetail)\nDebtHolder__c (custom) | DebtHolder → Account(Lookup), Debt__c(Lookup)\nDebtLawsuit__c (custom) | DebtLawsuit → Debt__c(Lookup), Lawsuit__c(Lookup)\nDebtSettlement__c (custom) | DebtSettlement → Account(Lookup), Account(Lookup), Entity__c(Lookup), IBAN__c(Lookup), Lawsuit__c(Lookup)\nDebtStatusExperienceSite__mdt (metadata) | DebtStatusExperienceSite\nDebtStatusHistory__c (custom) | DebtStatusHistory → Debt__c(Lookup)\nDebt__c (custom) | Debt → Account(Lookup), Entity__c(Lookup), User(Lookup), Entity__c(Lookup), User(Lookup)\nDebtsettlementLine__c (custom) | DebtsettlementLine → Debt__c(Lookup), DebtSettlement__c(Lookup)\nDeletedJournalEntry__c (custom) | DeletedJournalEntry → DebtSettlement__c(Lookup), Debt__c(Lookup), Entity__c(Lookup), Service__c(Lookup)\nDepartmentalManagers__mdt (metadata) | DepartmentalManagers\nDocumentSignatureConfiguration__mdt (metadata) | DocumentSignatureConfiguration\nEmailMessage (standard) | Email Message → Opportunity(Lookup)\nEmail_To_Case_Settings__mdt (metadata) | Email To Case Settings\nEntityProduct__c (custom) | EntityProduct → Entity__c(MasterDetail)\nEntity__c (custom) | Entity → User(Lookup), User(Lookup)\nErrorLog__c (custom) | ErrorLog\nGood__c (custom) | Good → Account(Lookup), Lead(Lookup)\nHistory__c (custom) | History → Account(Lookup), Bankruptcy__c(Lookup), Burofax__c(Lookup), User(Lookup), Debt__c(Lookup)\nHojaDeEncargoSMD__mdt (metadata) | HojaDeEncargoSMD\nIBAN__c (custom) | IBAN → Entity__c(Lookup)\nKmaleon_Auth__mdt (metadata) | Kmaleon Auth\nKmaleon_Setting__mdt (metadata) | Kmaleon Setting\nLSODocumentEquivalence__c (custom) | LSODocumentEquivalence\nLSODocument__c (custom) | LSODocument → Account(Lookup), User(Lookup), Service__c(Lookup)\nLSOWordDocumentType__mdt (metadata) | LSOWordDocumentType\nLSO_cmt_Document__mdt (metadata) | LSO cmt Document\nLawsuitLog__c (custom) | LawsuitLog → Lawsuit__c(MasterDetail)\nLawsuit__c (custom) | Lawsuit → Account(Lookup), User(Lookup), Burofax__c(Lookup), User(Lookup), Debt__c(Lookup)\nLead (standard) | Lead → User(Lookup), Lead(Lookup), Entity__c(Lookup), User(Lookup), Entity__c(Lookup)\nLegalProfessional__mdt (metadata) | LegalProfessional\nMASC_Configuration_Email__mdt (metadata) | MASC Configuration Email\nManagerSlotConfiguration__mdt (metadata) | ManagerSlotConfiguration\nNotificados__mdt (metadata) | Notificados\nNotificationMessageSetting__mdt (metadata) | NotificationMessageSetting\nNotificationTypeIds__mdt (metadata) | NotificationTypeIds\nNotification__c (custom) | Notification → Account(Lookup), Debt__c(Lookup)\nOpportunity (standard) | Opportunity → Account(Lookup), Service__c(Lookup), Account(Lookup), Campaign(Lookup), Pricebook2(Lookup)\nOwnerAssignment__mdt (metadata) | OwnerAssignment\nPaymentGatewayInstallment__c (custom) | PaymentGatewayInstallment → Saving__c(Lookup), Service__c(MasterDetail), PaymentGatewayToken__c(Lookup)\nPaymentGatewayToken__c (custom) | PaymentGatewayToken → Account(MasterDetail)\nPaymentIntegrationConfiguration__mdt (metadata) | PaymentIntegrationConfiguration\nPersonAccount (standard) | Person Account → Account(Lookup)\nPowerAttorney__c (custom) | PowerAttorney → Account(Lookup)\nPricebook2 (standard) | Pricebook\nPricebookEntry (standard) | Pricebook Entry → Pricebook2(MasterDetail), Product2(MasterDetail)\nProcess__c (custom) | Process → Account(Lookup), Service__c(Lookup)\nProduct2 (standard) | Product\nQuote (standard) | Quote → Opportunity(Lookup), Quote(Lookup), Service__c(Lookup), Opportunity(Lookup)\nRoundRobinLastAssignment__c (custom) | RoundRobinLastAssignment\nSalesDebt__c (custom) | SalesDebt → Opportunity(Lookup), Contract(Lookup), Debt__c(Lookup), Opportunity(Lookup), Service__c(Lookup)\nSaving__c (custom) | Saving → Account(Lookup), BankHolded__c(Lookup), DebtSettlement__c(Lookup), Debt__c(Lookup), Entity__c(Lookup)\nService__c (custom) | Service → Account(Lookup), User(Lookup), Account(Lookup), Contract(Lookup), Service__c(Lookup)\nSettlementInstallment__c (custom) | SettlementInstallment → DebtSettlement__c(MasterDetail), Debt__c(Lookup)\nSettlementPayment__c (custom) | SettlementPayment → DebtSettlement__c(MasterDetail), Debt__c(Lookup), SettlementInstallment__c(MasterDetail)\nSignaturitContracts__mdt (metadata) | SignaturitContracts\nSpanishBank__mdt (metadata) | SpanishBank\nStageTransition__mdt (metadata) | StageTransition\nTask__mdt (metadata) | Task\nTemplateWhatsappMetadata__mdt (metadata) | TemplateWhatsappMetadata\nTicketHolded__c (custom) | TicketHolded → Account(Lookup), Contract(Lookup), Service__c(MasterDetail)\nTransferType__mdt (metadata) | TransferType\nTransfer__c (custom) | Transfer → Account(Lookup), Service__c(Lookup)\nUser (standard) | User\nVendor_Config__mdt (metadata) | Vendor Config\nVendor_Payment__c (custom) | Vendor Payment → Account(Lookup), Bankruptcy__c(Lookup), Saving__c(Lookup), Service__c(Lookup)\nVoiceCall (standard) | Voice Call → Account(Lookup), User(Lookup)\nVoiceCall_Link__c (custom) | VoiceCall Link → Account(Lookup), Bankruptcy__c(Lookup), Debt__c(Lookup), DebtSettlement__c(Lookup), Lawsuit__c(Lookup)\nWhatsApp_Queue_Mapping__mdt (metadata) | WhatsApp Queue Mapping\nsignaturit__SignatureRequestFile__c (custom) | SignatureRequestFile → signaturit__SignatureRequest__c(MasterDetail)\nsignaturit__SignatureRequestSigner__c (custom) | SignatureRequestSigner → Account(Lookup), Contact(Lookup), Lead(Lookup), signaturit__SignatureRequest__c(MasterDetail), User(Lookup)\nsignaturit__SignatureRequest__c (custom) | SignatureRequest → Entity__c(Lookup), Contact(Lookup)\nsignaturit__SignatureSignerFile__c (custom) | SignatureSignerFile"};</script>
<script>

const GRAPH   = window.__ELCANO_DATA__.graph;
const DETAILS = window.__ELCANO_DATA__.details;

// ── THEME ────────────────────────────────────────────────────
const CAT_COLOR  = {custom:'#276749',standard:'#2b6cb0',metadata:'#c05621',bigobject:'#6b46c1',event:'#c53030'};
const CAT_FILL_L = {custom:'#c6f6d5',standard:'#bee3f8',metadata:'#feebc8',bigobject:'#e9d8fd',event:'#fed7d7'};
const CAT_FILL_D = {custom:'#1a4731',standard:'#1e3a5f',metadata:'#7b341e',bigobject:'#44337a',event:'#742a2a'};
const CAT_CLR_D  = {custom:'#6ee7b7',standard:'#93c5fd',metadata:'#fdba74',bigobject:'#c4b5fd',event:'#fca5a5'};
const CAT_R      = {custom:9,standard:8,metadata:6,bigobject:8,event:8};
const PATH_COLORS = ['#f97316','#a855f7','#22c55e','#ec4899','#14b8a6','#eab308'];

function isDark()      { return document.body.classList.contains('dark'); }
function catFill(c)    { return (isDark()?CAT_FILL_D:CAT_FILL_L)[c]||(isDark()?'#21262d':'#edf2f7'); }
function catColor(c)   { return (isDark()?CAT_CLR_D:CAT_COLOR)[c]||'#4a5568'; }
function textColor()   { return isDark()?'#e6edf3':'#1a202c'; }
function elColor()     { return isDark()?'#9198a1':'#4a5568'; }

// ── STATE ────────────────────────────────────────────────────
var visibleCats   = new Set(['custom','standard','metadata','bigobject','event']);
var searchTerm    = '';
var showLabels    = false;
var selectedIds   = new Set();   // up to 4
var lastSel       = null;        // for detail panel
var pathCache     = null;
var simNodes = [], simEdges = [];
var alpha = 1.0, animId = null;
var transform = {x:0,y:0,k:1};
var drag = null, panStart = null, panMoved = false;

var canvas = document.getElementById('c');
var ctx    = canvas.getContext('2d');

// ── PROFILES ─────────────────────────────────────────────────
var PRESET_AREAS = {
  'Programa — Transversal (Business Partner)': [
    'Debt__c','DebtHolder__c','DebtSettlement__c','DebtsettlementLine__c',
    'DebtStatusHistory__c','DebtCaseDepartment__c','DebtCaseReasonCatalog__c','DebtCaseTypeCatalog__c',
    'SalesDebt__c','Service__c','Account','Entity__c','Contract','Billing__c','Process__c','History__c',
    'EntityProduct__c','CatchmentSourceEquivalence__c','Client_Profile_Change__c','AccountInfo__c',
    'AccountRelationship__c','SettlementInstallment__c','SettlementPayment__c',
    'PaymentGatewayInstallment__c','PaymentGatewayToken__c','IBAN__c','Transfer__c',
    'Saving__c','Lawsuit__c','DebtLawsuit__c','LawsuitLog__c','Bankruptcy__c','PowerAttorney__c',
    'LSODocument__c','LSODocumentEquivalence__c','Good__c',
    'signaturit__SignatureRequest__c','signaturit__SignatureRequestFile__c',
    'signaturit__SignatureRequestSigner__c','signaturit__SignatureSignerFile__c',
    'Burofax__c','CertifiedEmail__c','CertifiedEmailFiles__c','Notification__c',
    'Case','Activity','EmailMessage','Opportunity','Quote','Lead',
    'Campaign','CampaignMember','Vendor_Payment__c','DebtHolder__c','User'
  ],
  'Programa — Ventas': [
    'Service__c','Debt__c','Account','Entity__c','Opportunity','Lead','Campaign','CampaignMember',
    'CatchmentSourceEquivalence__c','EntityProduct__c','Contract','Pricebook2',
    'PricebookEntry','Product2','Quote','AccountRelationship__c','AccountInfo__c',
    'Client_Profile_Change__c','User'
  ],
  'Programa — Legal Backoffice': [
    'LSODocument__c','LSODocumentEquivalence__c','PowerAttorney__c',
    'signaturit__SignatureRequest__c','signaturit__SignatureRequestFile__c',
    'signaturit__SignatureRequestSigner__c','signaturit__SignatureSignerFile__c',
    'Burofax__c','CertifiedEmail__c','CertifiedEmailFiles__c',
    'Debt__c','Account','Contract','Case','ContentDocument','ContentVersion',
    'HojaDeEncargoSMD__mdt','DocumentSignatureConfiguration__mdt','SignaturitContracts__mdt',
    'LSOWordDocumentType__mdt','LSO_cmt_Document__mdt'
  ],
  'Programa — Atención al Cliente': [
    'Debt__c','DebtStatusHistory__c','Service__c','Account','Process__c','History__c',
    'Case','Activity','EmailMessage','Notification__c','DebtCaseDepartment__c',
    'DebtCaseReasonCatalog__c','DebtCaseTypeCatalog__c','Client_Profile_Change__c',
    'Saving__c','CreditDefaultFile__c','AccountInfo__c','VoiceCall','VoiceCall_Link__c',
    'Conversation_History__b','BankHolded__c','TicketHolded__c','User'
  ],
  'Programa — Negociación': [
    'DebtSettlement__c','DebtsettlementLine__c','SettlementInstallment__c','SettlementPayment__c',
    'Lawsuit__c','DebtLawsuit__c','LawsuitLog__c','Bankruptcy__c','PowerAttorney__c',
    'Good__c','DebtCaseReasonCatalog__c','DebtCaseTypeCatalog__c',
    'Debt__c','Account','Service__c','IBAN__c','Contract','User'
  ],
  'Programa — Dirección Jurídica': [
    'Lawsuit__c','DebtLawsuit__c','LawsuitLog__c','Bankruptcy__c','PowerAttorney__c',
    'LSODocument__c','LSODocumentEquivalence__c','Good__c','Case',
    'Debt__c','Account','Contract',
    'signaturit__SignatureRequest__c','signaturit__SignatureRequestFile__c',
    'LegalProfessional__mdt','LSOWordDocumentType__mdt','LSO_cmt_Document__mdt'
  ],
  'Programa — Controller': [
    'Billing__c','Transfer__c','IBAN__c','Vendor_Payment__c','DebtHolder__c','SalesDebt__c',
    'PaymentGatewayInstallment__c','PaymentGatewayToken__c','DeletedJournalEntry__c',
    'Debt__c','Account','Contract','Entity__c','Opportunity',
    'PaymentIntegrationConfiguration__mdt','Vendor_Config__mdt',
    'BankHolded__c','BankCIF__mdt','SpanishBank__mdt'
  ],
  'Programa — Calidad': [
    'Debt__c','Service__c','Account','Process__c','History__c',
    'DebtCaseDepartment__c','DebtCaseReasonCatalog__c','DebtCaseTypeCatalog__c',
    'DebtStatusHistory__c','Case','Activity','User','ErrorLog__c'
  ],
  'LSO': [
    'LSODocument__c','LSODocumentEquivalence__c','LSOWordDocumentType__mdt','LSO_cmt_Document__mdt',
    'LegalProfessional__mdt','Lawsuit__c','DebtLawsuit__c','LawsuitLog__c',
    'Bankruptcy__c','PowerAttorney__c','Good__c','Debt__c','Account','Contract','Case',
    'signaturit__SignatureRequest__c','signaturit__SignatureRequestFile__c',
    'signaturit__SignatureRequestSigner__c','signaturit__SignatureSignerFile__c'
  ],
  'Marketing y Preventa': [
    'Lead','Campaign','CampaignMember','Account','Opportunity','Debt__c',
    'Entity__c','CatchmentSourceEquivalence__c','Contract','Quote',
    'Product2','Pricebook2','PricebookEntry','User','EmailMessage'
  ],
  'Servicios Centrales': [
    'Billing__c','Transfer__c','IBAN__c','Vendor_Payment__c','DebtHolder__c',
    'SalesDebt__c','Contract','Account','Opportunity','User',
    'DepartmentalManagers__mdt','OwnerAssignment__mdt','Vendor_Config__mdt'
  ],
  'Reclamadora': [
    'Debt__c','Account','Case','DebtCaseDepartment__c','DebtCaseReasonCatalog__c',
    'DebtCaseTypeCatalog__c','Lawsuit__c','BankHolded__c','TicketHolded__c',
    'CreditDefaultFile__c','CreditDefaultFileAnalysis__c','CreditDefaultFileResult__e',
    'Service__c','IBAN__c','User'
  ],
  'IT / Tecnología': [
    'AWS_Connect_Settings__mdt','Kmaleon_Auth__mdt','Kmaleon_Setting__mdt',
    'CommunityGate__mdt','Batch_settings__mdt','ErrorLog__c','DeletedJournalEntry__c',
    'VoiceCall_Link__c','CreditDefaultFileResult__e','PaymentIntegrationConfiguration__mdt',
    'CreditDefaultFile__c','CreditDefaultFileAnalysis__c','RoundRobinLastAssignment__c',
    'OwnerAssignment__mdt','DepartmentalManagers__mdt','ManagerSlotConfiguration__mdt',
    'StageTransition__mdt','AccountRelationship__c','AccountInfo__c','VoiceCall',
    'DebtStatusExperienceSite__mdt','DocumentSignatureConfiguration__mdt',
    'SignaturitContracts__mdt','HojaDeEncargoSMD__mdt','TransferType__mdt','Task__mdt',
    'NotificationTypeIds__mdt','NotificationMessageSetting__mdt','WhatsApp_Queue_Mapping__mdt',
    'TemplateWhatsappMetadata__mdt','Configuracion_Slots_Disponibles__mdt',
    'MASC_Configuration_Email__mdt','Email_To_Case_Settings__mdt'
  ]
};

// ── PROFILE STORAGE ──────────────────────────────────────────
function loadProfiles() {
  try { return JSON.parse(localStorage.getItem('elcano_profiles')||'[]'); } catch(e){ return []; }
}
function saveProfiles(p) { localStorage.setItem('elcano_profiles',JSON.stringify(p)); }
function getActiveProfileId() { return localStorage.getItem('elcano_active_profile')||''; }
function setActiveProfileId(id) { localStorage.setItem('elcano_active_profile',id); }
function getProfileById(id) { return loadProfiles().find(function(p){return p.id===id;})||null; }
function getVisibleObjectSet() {
  var pid = getActiveProfileId();
  if (!pid) return null;
  var p = getProfileById(pid);
  if (!p||!p.objects||!p.objects.length) return null;
  return new Set(p.objects);
}

// Seed default profile if first run
function seedDefaultProfile() {
  var profiles = loadProfiles();
  if (profiles.some(function(p){return p.id==='bp_programa';})) return;
  profiles.unshift({
    id: 'bp_programa',
    name: 'Business Partner — Programa',
    area: 'Programa — Transversal (Business Partner)',
    objects: PRESET_AREAS['Programa — Transversal (Business Partner)']
  });
  saveProfiles(profiles);
}

// ── RESIZE ───────────────────────────────────────────────────
function resize() {
  var w = canvas.parentElement, dpr = devicePixelRatio;
  canvas.width  = w.clientWidth*dpr; canvas.height = w.clientHeight*dpr;
  canvas.style.width = w.clientWidth+'px'; canvas.style.height = w.clientHeight+'px';
}
window.addEventListener('resize', function(){ resize(); draw(); });
resize();

// ── PATH ANALYSIS ─────────────────────────────────────────────
function findPath(fromId, toId, maxHops) {
  if (fromId === toId) return [fromId];
  var queue = [[fromId]];
  var visited = new Set([fromId]);
  while (queue.length) {
    var path = queue.shift();
    if (path.length > maxHops) continue;
    var last = path[path.length-1];
    for (var k = 0; k < simEdges.length; k++) {
      var e = simEdges[k];
      if (e.src === last && !visited.has(e.tgt)) {
        var np = path.concat([e.tgt]);
        if (e.tgt === toId) return np;
        visited.add(e.tgt);
        queue.push(np);
      }
    }
  }
  return null;
}

function getPairColor(i, j, len) {
  // assign a unique color index to each pair (i,j) where i<j
  var idx = 0, c = 0;
  for (var a = 0; a < len; a++) {
    for (var b = a+1; b < len; b++) {
      if (a===i && b===j) { idx = c; break; }
      c++;
    }
  }
  return PATH_COLORS[idx % PATH_COLORS.length];
}

function computePathData() {
  var ids = Array.from(selectedIds);
  var result = [];
  for (var i = 0; i < ids.length; i++) {
    for (var j = i+1; j < ids.length; j++) {
      var color = getPairColor(i, j, ids.length);
      var fwd = findPath(ids[i], ids[j], 4);
      if (fwd) {
        result.push({from:ids[i], to:ids[j], path:fwd, color:color, dir:'fwd'});
      } else {
        var bwd = findPath(ids[j], ids[i], 4);
        if (bwd) {
          result.push({from:ids[j], to:ids[i], path:bwd, color:color, dir:'bwd'});
        } else {
          result.push({from:ids[i], to:ids[j], path:null, color:color, dir:'none'});
        }
      }
    }
  }
  return result;
}

function getPathData() {
  if (selectedIds.size < 2) return [];
  if (pathCache) return pathCache;
  pathCache = computePathData();
  return pathCache;
}

// Returns Map<edgeIdx → color>
function buildPathEdgeMap(pathData) {
  var map = new Map();
  pathData.forEach(function(pd) {
    if (!pd.path || pd.path.length < 2) return;
    for (var k = 0; k < pd.path.length-1; k++) {
      var from = pd.path[k], to = pd.path[k+1];
      for (var i = 0; i < simEdges.length; i++) {
        if (simEdges[i].src===from && simEdges[i].tgt===to) {
          if (!map.has(i)) map.set(i, pd.color);
          break;
        }
      }
    }
  });
  return map;
}

// Returns Set of all node IDs on any found path (including intermediaries)
function buildPathNodeSet(pathData) {
  var s = new Set();
  pathData.forEach(function(pd) {
    if (pd.path) pd.path.forEach(function(id){ s.add(id); });
  });
  return s;
}

// ── SIMULATION ───────────────────────────────────────────────
function initSim() {
  pathCache = null;
  var W = canvas.width/devicePixelRatio, H = canvas.height/devicePixelRatio;
  var nodeMap = {};
  var objFilter = getVisibleObjectSet();

  var visNodes = GRAPH.nodes.filter(function(n) {
    if (!visibleCats.has(n.cat)) return false;
    if (searchTerm && !n.id.toLowerCase().includes(searchTerm) && !n.label.toLowerCase().includes(searchTerm)) return false;
    if (objFilter && !objFilter.has(n.id)) return false;
    return true;
  });
  var visIds = new Set(visNodes.map(function(n){return n.id;}));

  simNodes = visNodes.map(function(n) {
    var prev = simNodes.find(function(s){return s.id===n.id;});
    return {
      id:n.id, label:n.label, cat:n.cat, fc:n.fc, out:n.out,
      x: prev ? prev.x : W/2+(Math.random()-0.5)*300,
      y: prev ? prev.y : H/2+(Math.random()-0.5)*300,
      vx: prev ? prev.vx*0.5 : 0,
      vy: prev ? prev.vy*0.5 : 0,
      fixed: false
    };
  });
  simNodes.forEach(function(n,i){ nodeMap[n.id]=i; });
  simEdges = GRAPH.edges
    .filter(function(e){ return visIds.has(e.s)&&visIds.has(e.t); })
    .map(function(e){ return {si:nodeMap[e.s],ti:nodeMap[e.t],relType:e.r,req:e.req,field:e.f,src:e.s,tgt:e.t}; })
    .filter(function(e){ return e.si!==undefined&&e.ti!==undefined; });

  // Remove selected nodes that are no longer visible
  var newSel = new Set();
  selectedIds.forEach(function(id){ if(visIds.has(id)) newSel.add(id); });
  selectedIds = newSel;
  if (lastSel && !visIds.has(lastSel)) lastSel = null;

  alpha = 1.0;
  startAnim();
}

function tick() {
  if (alpha < 0.004) { alpha = 0; return; }
  var len = simNodes.length;
  for (var i = 0; i < len; i++) {
    for (var j = i+1; j < len; j++) {
      var ni=simNodes[i], nj=simNodes[j];
      var dx=nj.x-ni.x, dy=nj.y-ni.y, d2=dx*dx+dy*dy+0.01, d=Math.sqrt(d2);
      var f=-260/d2, fx=f*dx/d, fy=f*dy/d;
      ni.vx+=fx; ni.vy+=fy; nj.vx-=fx; nj.vy-=fy;
    }
  }
  for (var k=0; k<simEdges.length; k++) {
    var e=simEdges[k], s=simNodes[e.si], t=simNodes[e.ti];
    var dx=t.x-s.x, dy=t.y-s.y, d=Math.sqrt(dx*dx+dy*dy)||1;
    var td=e.relType==='MasterDetail'?95:120, f=(d-td)*0.04, fx=f*dx/d, fy=f*dy/d;
    s.vx+=fx; s.vy+=fy; t.vx-=fx; t.vy-=fy;
  }
  var W=canvas.width/devicePixelRatio, H=canvas.height/devicePixelRatio;
  for (var m=0; m<simNodes.length; m++) {
    var nd=simNodes[m];
    nd.vx+=(W/2-nd.x)*0.003; nd.vy+=(H/2-nd.y)*0.003;
  }
  for (var i=0; i<len; i++) {
    for (var j=i+1; j<len; j++) {
      var ni=simNodes[i],nj=simNodes[j];
      var minD=(CAT_R[ni.cat]||8)+(CAT_R[nj.cat]||8)+22;
      var dx=nj.x-ni.x, dy=nj.y-ni.y, d=Math.sqrt(dx*dx+dy*dy)||1;
      if(d<minD){var f=(minD-d)/d*0.5; ni.vx-=dx*f; ni.vy-=dy*f; nj.vx+=dx*f; nj.vy+=dy*f;}
    }
  }
  for (var m=0; m<simNodes.length; m++) {
    var nd=simNodes[m];
    if(nd.fixed){nd.vx=0;nd.vy=0;continue;}
    nd.vx*=0.72; nd.vy*=0.72;
    nd.x+=nd.vx*alpha; nd.y+=nd.vy*alpha;
  }
  alpha*=0.988;
}

function startAnim() {
  if (animId) cancelAnimationFrame(animId);
  function frame(){ tick(); draw(); animId=alpha>0.004?requestAnimationFrame(frame):null; }
  animId = requestAnimationFrame(frame);
}

// ── DRAW ─────────────────────────────────────────────────────
function toScreen(x,y){ return {x:x*transform.k+transform.x, y:y*transform.k+transform.y}; }
function toWorld(sx,sy){ return {x:(sx-transform.x)/transform.k, y:(sy-transform.y)/transform.k}; }

function draw() {
  var dpr=devicePixelRatio, W=canvas.width/dpr, H=canvas.height/dpr;
  ctx.setTransform(dpr,0,0,dpr,0,0);
  ctx.fillStyle = isDark()?'#0d1117':'#ffffff';
  ctx.fillRect(0,0,W,H);

  var multiSel = selectedIds.size >= 2;
  var pathData = multiSel ? getPathData() : [];
  var pathEdgeMap  = multiSel ? buildPathEdgeMap(pathData) : new Map();
  var pathNodeSet  = multiSel ? buildPathNodeSet(pathData) : new Set();

  // "connected to any selected" for single-select dim logic
  var connNodes = new Set(), connEdgeIdx = new Set();
  if (selectedIds.size === 1) {
    var sid = Array.from(selectedIds)[0];
    simEdges.forEach(function(e,i){
      if(e.src===sid||e.tgt===sid){ connNodes.add(e.src); connNodes.add(e.tgt); connEdgeIdx.add(i); }
    });
    connNodes.add(sid);
  }

  var mdC  = isDark()?'#c4b5fd':'#6b46c1';
  var luC  = isDark()?'#93c5fd':'#2b6cb0';
  var optC = isDark()?'#4a5568':'#a0aec0';

  // ── PASS 1: regular edges (skip path edges, will overdraw) ──
  for (var i=0; i<simEdges.length; i++) {
    if (pathEdgeMap.has(i)) continue;  // drawn in pass 2
    var e=simEdges[i];
    var sn=simNodes[e.si], tn=simNodes[e.ti];
    var ss=toScreen(sn.x,sn.y), ts=toScreen(tn.x,tn.y);
    var dx=ts.x-ss.x, dy=ts.y-ss.y, d=Math.sqrt(dx*dx+dy*dy)||1;
    var nr=(CAT_R[tn.cat]||8)*transform.k+10;
    var ex=ts.x-nr*dx/d, ey=ts.y-nr*dy/d;

    var dim, baseAlpha = isDark()?0.45:0.32;
    if (multiSel) {
      dim = !pathNodeSet.has(sn.id) && !pathNodeSet.has(tn.id) && !selectedIds.has(sn.id) && !selectedIds.has(tn.id);
      ctx.globalAlpha = dim ? 0.04 : 0.18;
    } else if (selectedIds.size===1) {
      var hl = connEdgeIdx.has(i);
      ctx.globalAlpha = hl ? 1.0 : 0.04;
    } else {
      ctx.globalAlpha = baseAlpha;
    }

    if (e.relType==='MasterDetail'){ ctx.strokeStyle=mdC; ctx.lineWidth=1.5; ctx.setLineDash([]); }
    else if(e.req){ ctx.strokeStyle=luC; ctx.lineWidth=1.2; ctx.setLineDash([]); }
    else { ctx.strokeStyle=optC; ctx.lineWidth=0.8; ctx.setLineDash([5,3]); }

    ctx.beginPath(); ctx.moveTo(ss.x,ss.y); ctx.lineTo(ex,ey); ctx.stroke();
    ctx.setLineDash([]);

    // arrowhead
    if (ctx.globalAlpha > 0.05) {
      var ang=Math.atan2(ey-ss.y,ex-ss.x);
      var ac=e.relType==='MasterDetail'?mdC:e.req?luC:optC;
      ctx.fillStyle=ac;
      ctx.beginPath(); ctx.moveTo(ex,ey);
      ctx.lineTo(ex-9*Math.cos(ang-0.38),ey-9*Math.sin(ang-0.38));
      ctx.lineTo(ex-9*Math.cos(ang+0.38),ey-9*Math.sin(ang+0.38));
      ctx.closePath(); ctx.fill();
    }

    if ((showLabels || connEdgeIdx.has(i)) && selectedIds.size===1 && d>60) {
      var mx=(ss.x+ts.x)/2, my=(ss.y+ts.y)/2;
      var card=e.relType==='MasterDetail'?'N:1':e.req?'N:1':'0..N:1';
      ctx.globalAlpha=0.9; ctx.fillStyle=elColor();
      ctx.font='bold 9px Segoe UI,sans-serif'; ctx.textAlign='center';
      ctx.fillText(card,mx,my-3);
    }
  }

  // ── PASS 2: path edges (colored, thick, on top) ──────────────
  pathEdgeMap.forEach(function(color, i) {
    var e=simEdges[i];
    if (!e) return;
    var sn=simNodes[e.si], tn=simNodes[e.ti];
    var ss=toScreen(sn.x,sn.y), ts=toScreen(tn.x,tn.y);
    var dx=ts.x-ss.x, dy=ts.y-ss.y, d=Math.sqrt(dx*dx+dy*dy)||1;
    var nr=(CAT_R[tn.cat]||8)*transform.k+10;
    var ex=ts.x-nr*dx/d, ey=ts.y-nr*dy/d;

    // Glow effect
    ctx.globalAlpha=0.25; ctx.strokeStyle=color; ctx.lineWidth=6; ctx.setLineDash([]);
    ctx.beginPath(); ctx.moveTo(ss.x,ss.y); ctx.lineTo(ex,ey); ctx.stroke();

    // Core line
    ctx.globalAlpha=1.0; ctx.strokeStyle=color; ctx.lineWidth=2.5;
    ctx.beginPath(); ctx.moveTo(ss.x,ss.y); ctx.lineTo(ex,ey); ctx.stroke();

    // Arrowhead
    var ang=Math.atan2(ey-ss.y,ex-ss.x);
    ctx.fillStyle=color;
    ctx.beginPath(); ctx.moveTo(ex,ey);
    ctx.lineTo(ex-10*Math.cos(ang-0.38),ey-10*Math.sin(ang-0.38));
    ctx.lineTo(ex-10*Math.cos(ang+0.38),ey-10*Math.sin(ang+0.38));
    ctx.closePath(); ctx.fill();

    // Cardinality label on path edges
    if (d>50) {
      var mx=(ss.x+ex)/2+7, my=(ss.y+ey)/2-7;
      var card=e.relType==='MasterDetail'?'N:1':e.req?'N:1':'0..N:1';
      ctx.globalAlpha=1; ctx.fillStyle=color;
      ctx.font='bold 9px Segoe UI,sans-serif'; ctx.textAlign='center';
      ctx.fillText(card,mx,my);
    }
  });

  // ── PASS 3: nodes ─────────────────────────────────────────
  ctx.setLineDash([]);
  for (var n=0; n<simNodes.length; n++) {
    var nd=simNodes[n];
    var s=toScreen(nd.x,nd.y);
    var r=(CAT_R[nd.cat]||8)*transform.k;
    var isSel = selectedIds.has(nd.id);
    var isPath = pathNodeSet.has(nd.id) && !isSel;

    var nodeAlpha;
    if (multiSel) {
      nodeAlpha = isSel ? 1.0 : isPath ? 0.7 : 0.08;
    } else if (selectedIds.size===1) {
      nodeAlpha = connNodes.has(nd.id) ? 1.0 : 0.08;
    } else {
      nodeAlpha = 1.0;
    }
    ctx.globalAlpha = nodeAlpha;

    if (isSel) { ctx.shadowColor=catColor(nd.cat); ctx.shadowBlur=16; }
    ctx.beginPath(); ctx.arc(s.x,s.y,r,0,Math.PI*2);
    ctx.fillStyle=catFill(nd.cat); ctx.fill();
    ctx.strokeStyle=catColor(nd.cat);
    ctx.lineWidth=isSel?3:1.5; ctx.stroke();
    ctx.shadowBlur=0;

    if (transform.k>0.38 && nodeAlpha>0.05) {
      var lbl=nd.label.length>17?nd.label.slice(0,15)+'…':nd.label;
      var fs=Math.max(8,Math.min(11,10*transform.k));
      ctx.fillStyle=nodeAlpha<0.5?(isDark()?'#444c56':'#a0aec0'):textColor();
      ctx.font=(isSel?'bold ':'')+fs+'px Segoe UI,sans-serif';
      ctx.textAlign='center'; ctx.fillText(lbl,s.x,s.y-r-3);
    }
  }

  // ── PASS 4: selected node badges (numbered circles) ────────
  ctx.globalAlpha=1;
  var idsArr=Array.from(selectedIds);
  idsArr.forEach(function(id,idx){
    var nd=simNodes.find(function(n){return n.id===id;});
    if(!nd) return;
    var s=toScreen(nd.x,nd.y);
    var r=(CAT_R[nd.cat]||8)*transform.k;
    var bx=s.x+r*0.72, by=s.y-r*0.72;
    var pc=PATH_COLORS[idx%PATH_COLORS.length];
    ctx.shadowColor=pc; ctx.shadowBlur=4;
    ctx.fillStyle=pc; ctx.beginPath(); ctx.arc(bx,by,7,0,Math.PI*2); ctx.fill();
    ctx.shadowBlur=0;
    ctx.fillStyle='#fff';
    ctx.font='bold 8px Segoe UI,sans-serif'; ctx.textAlign='center';
    ctx.fillText(String(idx+1),bx,by+3);
  });

  ctx.globalAlpha=1;
}

// ── INTERACTION ──────────────────────────────────────────────
function canvasXY(e) { var r=canvas.getBoundingClientRect(); return {x:e.clientX-r.left,y:e.clientY-r.top}; }
function hitNode(cx,cy) {
  var w=toWorld(cx,cy), best=null, bestD=Infinity;
  for(var i=0;i<simNodes.length;i++){
    var nd=simNodes[i], rd=(CAT_R[nd.cat]||8)+6;
    var d=Math.sqrt((nd.x-w.x)*(nd.x-w.x)+(nd.y-w.y)*(nd.y-w.y));
    if(d<rd&&d<bestD){best=nd;bestD=d;}
  }
  return best;
}

canvas.addEventListener('mousedown',function(e){
  var c=canvasXY(e), hit=hitNode(c.x,c.y);
  panMoved=false;
  if(hit){ drag={node:hit,moved:false}; hit.fixed=true; alpha=Math.max(alpha,0.3); }
  else { panStart={mx:c.x,my:c.y,tx:transform.x,ty:transform.y}; }
});

canvas.addEventListener('mousemove',function(e){
  var c=canvasXY(e);
  if(drag){ drag.moved=true; var w=toWorld(c.x,c.y); drag.node.x=w.x; drag.node.y=w.y; alpha=Math.max(alpha,0.08); if(!animId)startAnim(); return; }
  if(panStart){ panMoved=true; transform.x=panStart.tx+(c.x-panStart.mx); transform.y=panStart.ty+(c.y-panStart.my); draw(); }
});

canvas.addEventListener('mouseup',function(e){
  if(drag){
    if(!drag.moved){
      var multi=(e.ctrlKey||e.metaKey);
      doSelect(drag.node.id,multi);
    }
    drag.node.fixed=false; drag=null; return;
  }
  if(panStart){
    if(!panMoved){
      var c=canvasXY(e), hit=hitNode(c.x,c.y);
      if(hit){ var multi=(e.ctrlKey||e.metaKey); doSelect(hit.id,multi); }
      else deselectAll();
    }
    panStart=null;
  }
});

canvas.addEventListener('wheel',function(e){
  e.preventDefault();
  var c=canvasXY(e), f=e.deltaY<0?1.12:0.89;
  var k=Math.min(4,Math.max(0.1,transform.k*f));
  transform.x=c.x-(c.x-transform.x)*k/transform.k;
  transform.y=c.y-(c.y-transform.y)*k/transform.k;
  transform.k=k; draw();
},{passive:false});

function doSelect(id, multi) {
  pathCache=null;
  if(multi){
    if(selectedIds.has(id)){
      selectedIds.delete(id);
      if(lastSel===id) lastSel=selectedIds.size?Array.from(selectedIds)[selectedIds.size-1]:null;
    } else {
      if(selectedIds.size>=4) return; // max 4
      selectedIds.add(id);
      lastSel=id;
    }
  } else {
    selectedIds=new Set([id]);
    lastSel=id;
  }
  updateMultiSelBadge();
  if(!animId) draw();
  renderPanel();
}

function deselectAll() {
  selectedIds=new Set(); lastSel=null; pathCache=null;
  updateMultiSelBadge();
  if(!animId) draw();
  document.getElementById('obj-empty').style.display='block';
  document.getElementById('obj-content').style.display='none';
}

function updateMultiSelBadge() {
  var el=document.getElementById('multisel-badge');
  if(selectedIds.size>=2){
    el.textContent=selectedIds.size+' seleccionados ✕';
    el.style.display='';
  } else {
    el.style.display='none';
  }
}

window.selectNodeById=function(id){
  if(!DETAILS[id]) return;
  doSelect(id,false);
  var nd=simNodes.find(function(n){return n.id===id;});
  if(nd){
    var W=canvas.width/devicePixelRatio,H=canvas.height/devicePixelRatio;
    transform.x=W/2-nd.x*transform.k; transform.y=H/2-nd.y*transform.k; draw();
  }
};

document.getElementById('multisel-badge').addEventListener('click',deselectAll);

// ── PANEL ────────────────────────────────────────────────────
function renderPanel() {
  if(selectedIds.size>=2){
    renderMultiPanel();
  } else if(lastSel){
    renderSinglePanel(lastSel);
  }
}

function renderSinglePanel(id) {
  var d=DETAILS[id]; if(!d) return;
  document.getElementById('obj-empty').style.display='none';
  var el=document.getElementById('obj-content'); el.style.display='block';
  var cc=catColor(d.cat), cf=catFill(d.cat);
  var html='<div id="obj-name">'+escH(d.label)+'</div>'
    +'<span class="obj-badge" style="background:'+cf+';color:'+cc+'">'+d.cat.toUpperCase()+'</span>'
    +' <span style="font-size:10px;color:var(--text2)">'+escH(id)+'</span>'
    +'<div class="obj-meta">'+d.fieldCount+' campos &nbsp;·&nbsp; '+d.relationships.length+' salientes &nbsp;·&nbsp; '+d.incoming.length+' entrantes</div>';
  if(d.relationships.length){
    html+='<div class="rel-section"><h4>→ Apunta a</h4>';
    d.relationships.forEach(function(r){
      var cls=r.relType==='MasterDetail'?'rbadge-md':'rbadge-lu';
      var lbl=r.relType==='MasterDetail'?'MD':'LU';
      var card=r.relType==='MasterDetail'?'N:1':r.required?'N:1':'0..N:1';
      html+='<div class="rel-row"><span class="rel-badge '+cls+'">'+lbl+'</span>'
        +'<span class="card-tag">'+card+'</span>'
        +'<span class="rel-link" onclick="selectNodeById(\''+escJ(r.referenceTo)+'\')">'+escH(r.referenceTo)+'</span></div>';
    });
    html+='</div>';
  }
  if(d.incoming.length){
    html+='<div class="rel-section"><h4>← Referenciado por</h4>';
    d.incoming.forEach(function(src){
      var edge=GRAPH.edges.find(function(e){return e.s===src&&e.t===id;});
      var rt=edge?edge.r:'Lookup', rreq=edge?edge.req:false;
      var cls=rt==='MasterDetail'?'rbadge-md':'rbadge-lu';
      var lbl=rt==='MasterDetail'?'MD':'LU';
      var card=rt==='MasterDetail'?'1:N':rreq?'1:N':'1:0..N';
      html+='<div class="rel-row"><span class="rel-badge '+cls+'">'+lbl+'</span>'
        +'<span class="card-tag">'+card+'</span>'
        +'<span class="rel-link" onclick="selectNodeById(\''+escJ(src)+'\')">'+escH(src)+'</span></div>';
    });
    html+='</div>';
  }
  if(d.fields&&d.fields.length){
    html+='<div class="rel-section"><h4>Campos ('+d.fieldCount+')</h4>';
    d.fields.slice(0,20).forEach(function(f){
      html+='<div class="field-row"><span>'+escH(f.label)+'</span><span class="field-type-tag">'+escH(f.type)+'</span></div>';
    });
    if(d.fieldCount>20) html+='<div style="font-size:10px;color:var(--text2);padding:2px 0">+' + (d.fieldCount-20)+' más</div>';
    html+='</div>';
  }
  el.innerHTML=html;
}

function renderMultiPanel() {
  document.getElementById('obj-empty').style.display='none';
  var el=document.getElementById('obj-content'); el.style.display='block';
  var idsArr=Array.from(selectedIds);
  var pathData=getPathData();

  var html='<div style="font-size:12px;font-weight:700;color:var(--text);margin-bottom:8px">'
    +idsArr.length+' objetos seleccionados</div>';

  // Chips for each selected node
  html+='<div style="margin-bottom:10px;display:flex;flex-wrap:wrap;gap:2px">';
  idsArr.forEach(function(id,idx){
    var pc=PATH_COLORS[idx%PATH_COLORS.length];
    var lbl=DETAILS[id]?DETAILS[id].label:id;
    html+='<button class="path-node-chip" style="background:'+pc+';color:#fff" onclick="selectNodeById(\''+escJ(id)+'\')">'
      +(idx+1)+' · '+escH(lbl.length>16?lbl.slice(0,14)+'…':lbl)+'</button>';
  });
  html+='</div>';

  html+='<div style="font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.07em;color:var(--text2);margin-bottom:6px">Rutas detectadas</div>';

  if(!pathData.length){
    html+='<div style="font-size:11px;color:var(--text2)">Selecciona 2 o más nodos.</div>';
  } else {
    pathData.forEach(function(pd){
      var fromLbl=DETAILS[pd.from]?DETAILS[pd.from].label:pd.from;
      var toLbl  =DETAILS[pd.to  ]?DETAILS[pd.to  ].label:pd.to;
      html+='<div class="path-route">';
      html+='<div class="path-route-header">'
        +'<div class="path-dot" style="background:'+pd.color+'"></div>'
        +'<span>'+escH(fromLbl)+'</span>'
        +'<span style="color:var(--text2);font-weight:400"> → </span>'
        +'<span>'+escH(toLbl)+'</span></div>';
      if(pd.path){
        var hops=pd.path.length-1;
        if(hops===1){
          var edge=simEdges.find(function(e){return e.src===pd.from&&e.tgt===pd.to;});
          var rt=edge?edge.relType:'Lookup';
          var card=rt==='MasterDetail'?'N:1':edge&&edge.req?'N:1':'0..N:1';
          html+='<div class="path-route-detail">Directo · '+rt+' <span style="color:'+pd.color+';font-weight:700">'+card+'</span></div>';
        } else {
          html+='<div class="path-route-detail">'+hops+' salto'+(hops>1?'s':'')+' via: ';
          var intermediaries=pd.path.slice(1,-1);
          html+=intermediaries.map(function(id){
            var lbl=DETAILS[id]?DETAILS[id].label:id;
            return '<span class="hop" style="cursor:pointer" onclick="selectNodeById(\''+escJ(id)+'\')">'+escH(lbl)+'</span>';
          }).join(' → ');
          html+='</div>';
        }
      } else {
        html+='<div class="path-none">Sin ruta visible en el grafo actual</div>';
      }
      html+='</div>';
    });
  }

  html+='<div style="margin-top:10px;padding-top:8px;border-top:1px solid var(--border);font-size:10px;color:var(--text2)">'
    +'Ctrl+clic para añadir/quitar (máx 4)<br>'
    +'<span style="cursor:pointer;color:var(--accent)" onclick="deselectAll()">Limpiar selección</span></div>';

  el.innerHTML=html;
}

function escH(s){return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');}
function escJ(s){return String(s).replace(/'/g,"\\'");}

// ── PROFILE MODAL ─────────────────────────────────────────────
var editingProfileId=null;

function openProfileModal(editId){
  editingProfileId=editId||null;
  var nameEl=document.getElementById('pf-name');
  var areaEl=document.getElementById('pf-area');
  document.getElementById('modal-title').textContent=editId?'Editar perfil':'Nuevo perfil';
  if(editId){
    var p=getProfileById(editId);
    nameEl.value=p?p.name:''; areaEl.value=p?(p.area||''):'';
    renderObjList(p?p.objects:[]);
  } else {
    nameEl.value=''; areaEl.value=''; renderObjList([]);
  }
  document.getElementById('profile-modal').style.display='flex';
  nameEl.focus();
}
function closeProfileModal(){
  document.getElementById('profile-modal').style.display='none';
  editingProfileId=null;
}

var NODE_GROUPS=(function(){
  var cats={};
  GRAPH.nodes.forEach(function(n){
    if(!cats[n.cat])cats[n.cat]=[];
    cats[n.cat].push({id:n.id,label:n.label});
  });
  Object.keys(cats).forEach(function(c){
    cats[c].sort(function(a,b){return a.label.localeCompare(b.label);});
  });
  return cats;
})();
var CAT_LABELS={custom:'Objetos Custom',standard:'Standard',metadata:'Metadata',bigobject:'BigObject',event:'Evento'};
var CAT_ORDER=['custom','standard','metadata','bigobject','event'];

function renderObjList(selectedIdsArr){
  var sel=new Set(selectedIdsArr||[]);
  var ft=(document.getElementById('pf-obj-search')||{}).value||'';
  ft=ft.toLowerCase().trim();
  var container=document.getElementById('pf-obj-list');
  var html='';
  CAT_ORDER.forEach(function(cat){
    var items=(NODE_GROUPS[cat]||[]).slice();
    if(ft) items=items.filter(function(it){return it.id.toLowerCase().includes(ft)||it.label.toLowerCase().includes(ft);});
    if(!items.length) return;
    html+='<div class="pf-cat-header"><span>'+(CAT_LABELS[cat]||cat)+' ('+items.length+')</span>'
      +'<div style="display:flex;gap:4px">'
      +'<button onclick="selectCatAll(\''+cat+'\',\''+ft.replace(/'/g,"\\'")+'\'  ,true)">Todo</button>'
      +'<button onclick="selectCatAll(\''+cat+'\',\''+ft.replace(/'/g,"\\'")+'\'  ,false)">Nada</button>'
      +'</div></div>';
    items.forEach(function(it){
      var chk=sel.has(it.id)?'checked':'';
      html+='<label class="pf-obj-item">'
        +'<input type="checkbox" '+chk+' data-id="'+escH(it.id)+'" onchange="updateObjCount()">'
        +'<span class="pf-obj-label">'+escH(it.label)+'</span>'
        +'<span class="pf-obj-id">'+escH(it.id)+'</span></label>';
    });
  });
  container.innerHTML=html;
  updateObjCount();
}

window.selectCatAll=function(cat,ft,checked){
  var items=(NODE_GROUPS[cat]||[]).slice();
  if(ft){var ftl=ft.toLowerCase();items=items.filter(function(it){return it.id.toLowerCase().includes(ftl)||it.label.toLowerCase().includes(ftl);});}
  var ids=new Set(items.map(function(it){return it.id;}));
  document.querySelectorAll('#pf-obj-list input[type=checkbox]').forEach(function(cb){if(ids.has(cb.dataset.id))cb.checked=checked;});
  updateObjCount();
};
window.updateObjCount=function(){
  var checked=document.querySelectorAll('#pf-obj-list input[type=checkbox]:checked').length;
  var total=document.querySelectorAll('#pf-obj-list input[type=checkbox]').length;
  var el=document.getElementById('pf-count');
  if(el)el.textContent='('+checked+' de '+total+')';
};
function getCheckedObjectIds(){
  return Array.from(document.querySelectorAll('#pf-obj-list input[type=checkbox]:checked')).map(function(cb){return cb.dataset.id;});
}

document.getElementById('pf-area').addEventListener('change',function(){
  var preset=this.value?PRESET_AREAS[this.value]:null;
  renderObjList(preset||[]);
});
document.getElementById('pf-obj-search').addEventListener('input',function(){
  renderObjList(getCheckedObjectIds());
});
document.getElementById('pf-save').addEventListener('click',function(){
  var name=document.getElementById('pf-name').value.trim();
  if(!name){document.getElementById('pf-name').focus();return;}
  var area=document.getElementById('pf-area').value;
  var objects=getCheckedObjectIds();
  var profiles=loadProfiles();
  if(editingProfileId){
    var idx=profiles.findIndex(function(p){return p.id===editingProfileId;});
    if(idx>=0){profiles[idx].name=name;profiles[idx].area=area;profiles[idx].objects=objects;}
  } else {
    profiles.push({id:'p_'+Date.now(),name:name,area:area,objects:objects});
  }
  saveProfiles(profiles);
  closeProfileModal();
  refreshProfileSelect();
  if(editingProfileId===getActiveProfileId()) initSim();
});
document.getElementById('pf-cancel').addEventListener('click',closeProfileModal);
document.getElementById('profile-modal').addEventListener('click',function(e){if(e.target===this)closeProfileModal();});

// ── PROFILE SELECT BAR ────────────────────────────────────────
function refreshProfileSelect(){
  var sel=document.getElementById('profile-select');
  sel.innerHTML='<option value="">— Todos los objetos —</option>';
  loadProfiles().forEach(function(p){
    var opt=document.createElement('option');
    opt.value=p.id;
    opt.textContent=p.name+(p.area?' ('+p.area+')':'');
    sel.appendChild(opt);
  });
  sel.value=getActiveProfileId()||'';
  var hasP=!!getActiveProfileId()&&loadProfiles().some(function(p){return p.id===getActiveProfileId();});
  document.getElementById('btn-edit-profile').style.display=hasP?'':'none';
  document.getElementById('btn-del-profile').style.display=hasP?'':'none';
}
document.getElementById('profile-select').addEventListener('change',function(){
  setActiveProfileId(this.value); refreshProfileSelect(); initSim();
});
document.getElementById('btn-new-profile').addEventListener('click',function(){openProfileModal(null);});
document.getElementById('btn-edit-profile').addEventListener('click',function(){var id=getActiveProfileId();if(id)openProfileModal(id);});
document.getElementById('btn-del-profile').addEventListener('click',function(){
  if(getActiveProfileId()) document.getElementById('confirm-modal').style.display='flex';
});
document.getElementById('confirm-no').addEventListener('click',function(){document.getElementById('confirm-modal').style.display='none';});
document.getElementById('confirm-yes').addEventListener('click',function(){
  var id=getActiveProfileId();
  saveProfiles(loadProfiles().filter(function(p){return p.id!==id;}));
  setActiveProfileId('');
  document.getElementById('confirm-modal').style.display='none';
  refreshProfileSelect(); initSim();
});

// ── CONTROLS ─────────────────────────────────────────────────
document.querySelectorAll('.filter-btn').forEach(function(btn){
  btn.addEventListener('click',function(){
    var c=btn.dataset.cat;
    if(visibleCats.has(c)){visibleCats.delete(c);btn.classList.remove('active');btn.classList.add('inactive');}
    else{visibleCats.add(c);btn.classList.remove('inactive');btn.classList.add('active');}
    initSim();
  });
});
document.getElementById('search').addEventListener('input',function(e){searchTerm=e.target.value.toLowerCase().trim();initSim();});
document.getElementById('btn-labels').addEventListener('click',function(){
  showLabels=!showLabels;this.classList.toggle('active',showLabels);if(!animId)draw();
});
document.getElementById('btn-fit').addEventListener('click',function(){
  if(!simNodes.length)return;
  var W=canvas.width/devicePixelRatio,H=canvas.height/devicePixelRatio;
  var minX=Infinity,maxX=-Infinity,minY=Infinity,maxY=-Infinity;
  simNodes.forEach(function(n){minX=Math.min(minX,n.x);maxX=Math.max(maxX,n.x);minY=Math.min(minY,n.y);maxY=Math.max(maxY,n.y);});
  var pad=50,k=Math.min((W-pad*2)/(maxX-minX||1),(H-pad*2)/(maxY-minY||1),3)*0.9;
  transform.k=k; transform.x=W/2-(minX+maxX)/2*k; transform.y=H/2-(minY+maxY)/2*k; draw();
});
document.getElementById('btn-theme').addEventListener('click',function(){
  var dark=document.body.classList.toggle('dark');
  this.textContent=dark?'☀️ Claro':'🌙 Oscuro';
  localStorage.setItem('elcano_dark',dark?'1':'0');
  if(!animId)draw();
  if(selectedIds.size>=2)renderMultiPanel(); else if(lastSel)renderSinglePanel(lastSel);
});

// ── BOOT ─────────────────────────────────────────────────────
if(localStorage.getItem('elcano_dark')==='1'){
  document.body.classList.add('dark');
  document.getElementById('btn-theme').textContent='☀️ Claro';
}
seedDefaultProfile();
resize();
var initW=canvas.width/devicePixelRatio, initH=canvas.height/devicePixelRatio;
transform={x:initW/2,y:initH/2,k:1};
refreshProfileSelect();
initSim();

</script>
</body>
</html>
