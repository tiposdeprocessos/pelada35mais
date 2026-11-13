<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pelada da Galera - Confirmação Semanal</title>
  <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
  <style>
    :root {
      --bg: #0f172a;
      --card: #1e293b;
      --border: #334155;
      --text: #f8fafc;
      --muted: #94a3b8;
      --green: #16a34a;
      --green-bg: rgba(22, 163, 74, 0.15);
      --red: #dc2626;
      --red-bg: rgba(220, 38, 38, 0.15);
      --amber: #d97706;
      --amber-bg: rgba(217, 119, 6, 0.15);
      --blue: #2563eb;
    }

    * {
      box-sizing: border-box;
      font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    }

    body {
      margin: 0;
      padding: 20px 14px;
      background-color: var(--bg);
      color: var(--text);
      display: flex;
      justify-content: center;
    }

    .container {
      width: 100%;
      max-width: 860px;
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 20px;
    }

    h1 { margin: 0 0 6px 0; font-size: 1.45rem; }
    h2 { margin: 0 0 12px 0; font-size: 1.08rem; color: #e2e8f0; }
    .subtitle { margin: 0; color: var(--muted); font-size: 0.9rem; }

    .verdict-box {
      padding: 18px 20px;
      border-radius: 12px;
      border: 1px solid;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 12px;
    }
    .verdict-yes { background: var(--green-bg); border-color: var(--green); color: #86efac; }
    .verdict-wait { background: var(--amber-bg); border-color: var(--amber); color: #fcd34d; }
    .verdict-title { font-size: 1.15rem; font-weight: 700; display: block; }
    .verdict-sub { font-size: 0.88rem; opacity: 0.9; }

    .form-grid {
      display: grid;
      grid-template-columns: 1.2fr 1.2fr;
      gap: 12px;
    }
    @media (max-width: 620px) {
      .form-grid { grid-template-columns: 1fr; }
    }

    label {
      display: block;
      font-size: 0.82rem;
      color: var(--muted);
      margin-bottom: 5px;
      font-weight: 600;
    }

    select, input {
      width: 100%;
      padding: 11px 12px;
      border-radius: 8px;
      border: 1px solid var(--border);
      background: #0f172a;
      color: var(--text);
      font-size: 0.95rem;
    }
    select:focus, input:focus { outline: none; border-color: var(--blue); }

    .choice-group {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      margin-top: 14px;
    }

    .btn-choice {
      padding: 13px;
      border-radius: 10px;
      border: 2px solid transparent;
      font-size: 0.98rem;
      font-weight: 700;
      cursor: pointer;
      transition: 0.15s;
    }
    .btn-sim { background: #14532d; color: #86efac; border-color: #16a34a; }
    .btn-sim:hover { background: #16a34a; color: #fff; }
    .btn-nao { background: #7f1d1d; color: #fca5a5; border-color: #dc2626; }
    .btn-nao:hover { background: #dc2626; color: #fff; }

    .btn-secondary {
      background: #334155;
      color: #f8fafc;
      border: none;
      padding: 9px 13px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      font-size: 0.85rem;
    }
    .btn-secondary:hover { background: #475569; }

    .tabs {
      display: flex;
      gap: 8px;
      margin-bottom: 14px;
      flex-wrap: wrap;
    }
    .tab-btn {
      padding: 7px 14px;
      border-radius: 999px;
      border: 1px solid var(--border);
      background: #0f172a;
      color: var(--muted);
      cursor: pointer;
      font-size: 0.85rem;
      font-weight: 600;
    }
    .tab-btn.active {
      background: var(--blue);
      color: white;
      border-color: var(--blue);
    }

    .player-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 14px;
      background: #0f172a;
      border: 1px solid var(--border);
      border-radius: 8px;
      margin-bottom: 8px;
      gap: 10px;
      flex-wrap: wrap;
    }

    .badge {
      padding: 4px 10px;
      border-radius: 999px;
      font-size: 0.78rem;
      font-weight: 700;
    }
    .badge-sim { background: var(--green-bg); color: #86efac; border: 1px solid var(--green); }
    .badge-nao { background: var(--red-bg); color: #fca5a5; border: 1px solid var(--red); }
    .badge-wait { background: var(--amber-bg); color: #fcd34d; border: 1px solid var(--amber); }

    .btn-wpp {
      background: #15803d;
      color: white;
      text-decoration: none;
      padding: 5px 10px;
      border-radius: 6px;
      font-size: 0.78rem;
      font-weight: 600;
    }

    .hidden { display: none; }
    .toast {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #2563eb;
      color: white;
      padding: 13px 18px;
      border-radius: 10px;
      font-size: 0.9rem;
      font-weight: 600;
      display: none;
      max-width: 350px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.45);
      z-index: 999;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="card" style="display: flex; justify-content: space-between; align-items: flex-start; gap: 12px; flex-wrap: wrap;">
      <div>
        <h1>⚽ Pelada da Galera - Confirmação Semanal</h1>
        <p class="subtitle" id="headerInfo"></p>
      </div>
      <button type="button" class="btn-secondary" onclick="toggleAdmin()">⚙️ Configurar Jogo / E-mail</button>
    </div>

    <div id="adminPanel" class="card hidden">
      <h2>⚙️ Configurações da Pelada & Disparo de E-mail (EmailJS)</h2>
      <div class="form-grid" style="margin-bottom: 12px;">
        <div>
          <label>Dia / Data do Jogo</label>
          <input type="text" id="cfgDay" oninput="saveSettings()" />
        </div>
        <div>
          <label>Horário do Jogo</label>
          <input type="time" id="cfgTime" oninput="saveSettings()" />
        </div>
        <div>
          <label>Prazo Final para Resposta</label>
          <input type="text" id="cfgDeadline" oninput="saveSettings()" />
        </div>
        <div>
          <label>Mínimo de Jogadores ("SIM")</label>
          <input type="number" id="cfgMin" min="2" max="40" oninput="saveSettings()" />
        </div>
      </div>

      <div class="form-grid">
        <div>
          <label>EmailJS Public Key</label>
          <input type="text" id="cfgPublicKey" placeholder="Ex: user_xxxxx" oninput="saveSettings()" />
        </div>
        <div>
          <label>EmailJS Service ID</label>
          <input type="text" id="cfgServiceId" placeholder="Ex: service_xxxxx" oninput="saveSettings()" />
        </div>
        <div>
          <label>EmailJS Template ID</label>
          <input type="text" id="cfgTemplateId" placeholder="Ex: template_xxxxx" oninput="saveSettings()" />
        </div>
        <div style="display: flex; align-items: end;">
          <button type="button" class="btn-secondary" style="width: 100%;" onclick="resetWeek()">🔄 Zerar Respostas da Semana</button>
        </div>
      </div>
    </div>

    <div id="verdictBox" class="verdict-box verdict-wait">
      <div>
        <span id="verdictTitle" class="verdict-title">⏳ Aguardando confirmações...</span>
        <span id="verdictSub" class="verdict-sub"></span>
      </div>
      <div style="text-align: right;">
        <strong id="verdictScore" style="font-size: 1.45rem;">0 / 12</strong>
        <div style="font-size: 0.78rem; opacity: 0.85;">Confirmados / Mínimo</div>
      </div>
    </div>

    <div class="card">
      <h2>🙋‍♂️ Selecione seu nome, informe seu e-mail e confirme</h2>
      <p class="subtitle" style="margin-bottom: 14px;">
        Seu e-mail ficará salvo para as próximas semanas. Ao clicar em <strong>SIM</strong> ou <strong>NÃO</strong>, enviaremos a notificação para você.
      </p>

      <div class="form-grid">
        <div>
          <label for="playerSelect">Escolha seu Nome na Lista do Grupo *</label>
          <select id="playerSelect" onchange="onSelectPlayer()">
            <option value="">-- Selecione seu nome --</option>
          </select>
        </div>
        <div>
          <label for="playerEmail">Seu E-mail para Notificação *</label>
          <input type="email" id="playerEmail" placeholder="seuemail@exemplo.com" />
        </div>
      </div>

      <div class="choice-group">
        <button type="button" class="btn-choice btn-sim" onclick="confirmPresence('SIM')">
          ✅ SIM, EU VOU JOGAR
        </button>
        <button type="button" class="btn-choice btn-nao" onclick="confirmPresence('NÃO')">
          ❌ NÃO VOU PODER IR
        </button>
      </div>
    </div>

    <div class="card">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; flex-wrap: wrap; gap: 8px;">
        <h2 style="margin: 0;">📋 Status dos Jogadores (<span id="totalPlayers">0</span>)</h2>
        <button type="button" class="btn-secondary" onclick="copyWhatsAppList()">📲 Copiar Lista p/ WhatsApp</button>
      </div>

      <div class="tabs">
        <button class="tab-btn active" onclick="setFilter('ALL', this)">Todos</button>
        <button class="tab-btn" onclick="setFilter('SIM', this)">✅ Confirmados (<span id="tabSim">0</span>)</button>
        <button class="tab-btn" onclick="setFilter('NAO', this)">❌ Não vão (<span id="tabNao">0</span>)</button>
        <button class="tab-btn" onclick="setFilter('PENDENTE', this)">⏳ Sem resposta (<span id="tabWait">0</span>)</button>
      </div>

      <input type="text" id="searchInput" placeholder="🔍 Filtrar jogador por nome ou telefone..." oninput="renderList()" style="margin-bottom: 12px;" />

      <div id="playersContainer"></div>
    </div>
  </div>

  <div id="toast" class="toast"></div>

  <script>
    const STORAGE_KEY = 'pelada_grupo_oficial_v1';

    const INITIAL_MEMBERS = [
      { id: 1, name: 'Organizador (Você)', phone: '', email: '', status: 'PENDENTE' },
      { id: 2, name: 'Alexandre Pereira Martins', phone: '', email: '', status: 'PENDENTE' },
      { id: 3, name: 'Beto', phone: '', email: '', status: 'PENDENTE' },
      { id: 4, name: 'Cafu', phone: '', email: '', status: 'PENDENTE' },
      { id: 5, name: 'Humberto Martins', phone: '', email: '', status: 'PENDENTE' },
      { id: 6, name: 'M. Vinicius', phone: '', email: '', status: 'PENDENTE' },
      { id: 7, name: 'Marcelo Duarte', phone: '', email: '', status: 'PENDENTE' },
      { id: 8, name: 'Rubens Rodrigues', phone: '', email: '', status: 'PENDENTE' },
      { id: 9, name: 'Uzbequistão - Felipe Vieira', phone: '', email: '', status: 'PENDENTE' },
      { id: 10, name: 'Uzbequistão - Flávio', phone: '', email: '', status: 'PENDENTE' },
      { id: 11, name: 'Uzbequistão - Francisco', phone: '', email: '', status: 'PENDENTE' },
      { id: 12, name: 'Uzbequistão - Luiz Henrique', phone: '', email: '', status: 'PENDENTE' },
      { id: 13, name: 'Uzbequistão - Ricardo', phone: '', email: '', status: 'PENDENTE' },
      { id: 14, name: 'Uzbequistão - Robson', phone: '', email: '', status: 'PENDENTE' },
      { id: 15, name: 'Uzbequistão - Tiago', phone: '', email: '', status: 'PENDENTE' },
      { id: 16, name: 'Arthur Garcia', phone: '553194030095', email: '', status: 'PENDENTE' },
      { id: 17, name: 'Bruno Aleixo', phone: '553186266657', email: '', status: 'PENDENTE' },
      { id: 18, name: 'Bruno Araujo', phone: '553191075292', email: '', status: 'PENDENTE' },
      { id: 19, name: 'Carlos', phone: '553196600576', email: '', status: 'PENDENTE' },
      { id: 20, name: 'Clever Amorim', phone: '553184913342', email: '', status: 'PENDENTE' },
      { id: 21, name: 'Edimar Rocha', phone: '553194890035', email: '', status: 'PENDENTE' },
      { id: 22, name: 'Edu Lellis', phone: '553191748556', email: '', status: 'PENDENTE' },
      { id: 23, name: 'Francisco Saldanha', phone: '553195219956', email: '', status: 'PENDENTE' },
      { id: 24, name: 'Guilherme', phone: '553194431141', email: '', status: 'PENDENTE' },
      { id: 25, name: 'Israel', phone: '553184748156', email: '', status: 'PENDENTE' },
      { id: 26, name: 'Jacques Ferraz', phone: '553186989606', email: '', status: 'PENDENTE' },
      { id: 27, name: 'Jorginho', phone: '553193400836', email: '', status: 'PENDENTE' },
      { id: 28, name: 'Júlio Lara', phone: '553186618881', email: '', status: 'PENDENTE' },
      { id: 29, name: 'Lucas Duarte', phone: '553199670916', email: '', status: 'PENDENTE' },
      { id: 30, name: 'Luis Claudio', phone: '553188407463', email: '', status: 'PENDENTE' },
      { id: 31, name: 'Marcelo Duarte (2)', phone: '553188164284', email: '', status: 'PENDENTE' },
      { id: 32, name: 'Mvcar', phone: '553196270759', email: '', status: 'PENDENTE' },
      { id: 33, name: 'Raimundo', phone: '553199813858', email: '', status: 'PENDENTE' },
      { id: 34, name: 'Rodrigo Resende', phone: '553185619799', email: '', status: 'PENDENTE' },
      { id: 35, name: 'Rondinelli', phone: '553199163540', email: '', status: 'PENDENTE' },
      { id: 36, name: 'Tatu', phone: '553175263318', email: '', status: 'PENDENTE' },
      { id: 37, name: 'Tudo Pelos Meus Filhos', phone: '553184974038', email: '', status: 'PENDENTE' },
      { id: 38, name: 'Vinícius Moreira', phone: '553191721720', email: '', status: 'PENDENTE' },
      { id: 39, name: 'Warley', phone: '553186219023', email: '', status: 'PENDENTE' },
      { id: 40, name: 'William Braga', phone: '553188910104', email: '', status: 'PENDENTE' },
      { id: 41, name: 'Contato +55 31 9217-5088', phone: '553192175088', email: '', status: 'PENDENTE' },
      { id: 42, name: 'Contato +55 31 8886-3941', phone: '553188863941', email: '', status: 'PENDENTE' },
      { id: 43, name: 'Contato +55 31 9272-8727', phone: '553192728727', email: '', status: 'PENDENTE' },
      { id: 44, name: 'Contato +55 31 9777-2120', phone: '553197772120', email: '', status: 'PENDENTE' },
      { id: 45, name: 'Contato +55 31 7153-2395', phone: '553171532395', email: '', status: 'PENDENTE' },
      { id: 46, name: 'Contato +55 31 9935-7065', phone: '553199357065', email: '', status: 'PENDENTE' },
      { id: 47, name: 'Contato +55 31 9482-1210', phone: '553194821210', email: '', status: 'PENDENTE' },
      { id: 48, name: 'Contato +55 31 9950-3362', phone: '553199503362', email: '', status: 'PENDENTE' }
    ];

    let state = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {
      settings: {
        day: 'Quarta-feira, 18/11',
        time: '20:00',
        deadline: 'Terça-feira às 18:00',
        minPlayers: 12,
        publicKey: '',
        serviceId: '',
        templateId: ''
      },
      players: INITIAL_MEMBERS
    };

    let currentFilter = 'ALL';

    function save() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
    }

    function showToast(msg) {
      const t = document.getElementById('toast');
      t.textContent = msg;
      t.style.display = 'block';
      setTimeout(() => { t.style.display = 'none'; }, 4200);
    }

    function toggleAdmin() {
      document.getElementById('adminPanel').classList.toggle('hidden');
    }

    function saveSettings() {
      state.settings.day = document.getElementById('cfgDay').value;
      state.settings.time = document.getElementById('cfgTime').value;
      state.settings.deadline = document.getElementById('cfgDeadline').value;
      state.settings.minPlayers = parseInt(document.getElementById('cfgMin').value, 10) || 12;
      state.settings.publicKey = document.getElementById('cfgPublicKey').value.trim();
      state.settings.serviceId = document.getElementById('cfgServiceId').value.trim();
      state.settings.templateId = document.getElementById('cfgTemplateId').value.trim();
      save();
      renderAll();
    }

    function resetWeek() {
      state.players.forEach(p => p.status = 'PENDENTE');
      save();
      renderAll();
      showToast('Status zerados para a nova semana (os e-mails salvos foram mantidos)!');
    }

    function onSelectPlayer() {
      const id = parseInt(document.getElementById('playerSelect').value, 10);
      const player = state.players.find(p => p.id === id);
      document.getElementById('playerEmail').value = player && player.email ? player.email : '';
    }

    async function sendEmailNotification(player, voteText) {
      const { publicKey, serviceId, templateId, day, time, deadline } = state.settings;
      if (publicKey && serviceId && templateId) {
        try {
          emailjs.init({ publicKey });
          await emailjs.send(serviceId, templateId, {
            to_name: player.name,
            to_email: player.email,
            resposta: voteText,
            dia_jogo: day,
            horario_jogo: time,
            prazo_resposta: deadline
          });
          showToast(`✅ ${player.name}: "${voteText}" registrado e e-mail enviado para ${player.email}!`);
        } catch (e) {
          showToast(`⚠️ Resposta salva, mas houve erro no envio do e-mail. Confira as chaves do EmailJS.`);
        }
      } else {
        showToast(`✅ ${player.name}: "${voteText}" registrado! (Configure o EmailJS em ⚙️ para disparo real)`);
      }
    }

    async function confirmPresence(choice) {
      const id = parseInt(document.getElementById('playerSelect').value, 10);
      const email = document.getElementById('playerEmail').value.trim();

      if (!id) {
        showToast('⚠️ Selecione seu nome na lista primeiro.');
        return;
      }
      if (!email || !email.includes('@')) {
        showToast('⚠️ Digite um e-mail válido para receber a notificação.');
        return;
      }

      const player = state.players.find(p => p.id === id);
      player.email = email;
      player.status = choice === 'SIM' ? 'SIM' : 'NAO';

      save();
      renderAll();
      await sendEmailNotification(player, choice);
    }

    function setFilter(filter, btn) {
      currentFilter = filter;
      document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      renderList();
    }

    function copyWhatsAppList() {
      const simList = state.players.filter(p => p.status === 'SIM');
      const min = state.settings.minPlayers;
      const hasGame = simList.length >= min;

      const lines = simList.map((p, idx) => `${idx + 1}. ${p.name}`).join('\n');
      const msg =
        `⚽ *PELADA - ${state.settings.day.toUpperCase()} ÀS ${state.settings.time}*\n` +
        `⏰ Resposta final: ${state.settings.deadline}\n` +
        `📊 Status: ${hasGame ? '✅ VAI TER JOGO!' : `⏳ Faltam ${min - simList.length} jogador(es)`}\n\n` +
        `*Confirmados (${simList.length}/${min}):*\n` +
        (lines || 'Ninguém confirmado ainda.');

      navigator.clipboard.writeText(msg).then(() => {
        showToast('Lista copiada! Agora é só colar no grupo do WhatsApp.');
      });
    }

    function renderList() {
      const query = document.getElementById('searchInput').value.toLowerCase();
      const container = document.getElementById('playersContainer');
      container.innerHTML = '';

      const filtered = state.players.filter(p => {
        const matchTab = currentFilter === 'ALL' || p.status === currentFilter;
        const matchText = p.name.toLowerCase().includes(query) || (p.phone && p.phone.includes(query));
        return matchTab && matchText;
      });

      filtered.forEach(p => {
        let badgeClass = 'badge-wait';
        let badgeText = '⏳ Sem resposta';
        if (p.status === 'SIM') { badgeClass = 'badge-sim'; badgeText = '✅ SIM'; }
        if (p.status === 'NAO') { badgeClass = 'badge-nao'; badgeText = '❌ NÃO'; }

        const wppMsg = encodeURIComponent(`Fala ${p.name}! Confirma lá na pelada de ${state.settings.day} às ${state.settings.time} (prazo: ${state.settings.deadline}).`);
        const wppBtn = p.phone
          ? `<a href="https://wa.me/${p.phone}?text=${wppMsg}" target="_blank" class="btn-wpp">📲 Cobrar no Zap</a>`
          : '';

        const row = document.createElement('div');
        row.className = 'player-row';
        row.innerHTML = `
          <div>
            <strong>${p.name}</strong>
            <div style="font-size: 0.8rem; color: var(--muted);">
              ${p.email ? `✉️ ${p.email}` : 'E-mail ainda não informado'}
            </div>
          </div>
          <div style="display: flex; align-items: center; gap: 8px; flex-wrap: wrap;">
            ${wppBtn}
            <span class="badge ${badgeClass}">${badgeText}</span>
          </div>
        `;
        container.appendChild(row);
      });
    }

    function renderAll() {
      const s = state.settings;
      document.getElementById('cfgDay').value = s.day;
      document.getElementById('cfgTime').value = s.time;
      document.getElementById('cfgDeadline').value = s.deadline;
      document.getElementById('cfgMin').value = s.minPlayers;
      document.getElementById('cfgPublicKey').value = s.publicKey;
      document.getElementById('cfgServiceId').value = s.serviceId;
      document.getElementById('cfgTemplateId').value = s.templateId;

      document.getElementById('headerInfo').textContent =
        `Jogo: ${s.day} às ${s.time} • Verificação final: ${s.deadline}`;

      const select = document.getElementById('playerSelect');
      const currentVal = select.value;
      select.innerHTML = '<option value="">-- Selecione seu nome --</option>';
      state.players.forEach(p => {
        const opt = document.createElement('option');
        opt.value = p.id;
        opt.textContent = p.name;
        select.appendChild(opt);
      });
      select.value = currentVal;

      const simCount = state.players.filter(p => p.status === 'SIM').length;
      const naoCount = state.players.filter(p => p.status === 'NAO').length;
      const waitCount = state.players.filter(p => p.status === 'PENDENTE').length;

      document.getElementById('totalPlayers').textContent = state.players.length;
      document.getElementById('tabSim').textContent = simCount;
      document.getElementById('tabNao').textContent = naoCount;
      document.getElementById('tabWait').textContent = waitCount;
      document.getElementById('verdictScore').textContent = `${simCount} / ${s.minPlayers}`;

      const vBox = document.getElementById('verdictBox');
      const vTitle = document.getElementById('verdictTitle');
      const vSub = document.getElementById('verdictSub');

      vBox.className = 'verdict-box';
      if (simCount >= s.minPlayers) {
        vBox.classList.add('verdict-yes');
        vTitle.textContent = '✅ VAI TER PELADA! Quórum atingido.';
        vSub.textContent = `Jogo confirmado para ${s.day} às ${s.time}.`;
      } else {
        vBox.classList.add('verdict-wait');
        vTitle.textContent = `⏳ Faltam ${s.minPlayers - simCount} jogador(es) para confirmar`;
        vSub.textContent = `Verificação definitiva em: ${s.deadline}.`;
      }

      renderList();
    }

    renderAll();
  </script>
</body>
</html>
