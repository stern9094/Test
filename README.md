<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Windkarten</title>
  <style>
    :root {
      --bg: #0f172a;
      --card-bg: #1e293b;
      --card-border: #334155;
      --accent: #38bdf8;
      --text: #f1f5f9;
      --muted: #94a3b8;
      --arrow: #fbbf24;
      --special: #a78bfa;
      --success: #4ade80;
      --locked: #475569;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: linear-gradient(160deg, #0f172a 0%, #1e293b 55%, #0f172a 100%);
      color: var(--text);
      min-height: 100vh;
      padding: 1rem;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    h1 {
      font-size: 1.4rem;
      color: var(--accent);
      margin-bottom: 0.3rem;
    }

    /* SETUP */
    #setup {
      width: 100%;
      max-width: 360px;
      text-align: center;
      margin-top: 2rem;
    }

    #setup h2 {
      font-size: 1.1rem;
      color: var(--muted);
      margin-bottom: 1.5rem;
    }

    .player-select {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 0.6rem;
      margin-bottom: 1.8rem;
    }

    .player-btn {
      aspect-ratio: 1;
      background: var(--card-bg);
      border: 2px solid var(--card-border);
      border-radius: 14px;
      font-size: 1.6rem;
      font-weight: 700;
      color: var(--text);
      cursor: pointer;
      transition: all 0.15s;
    }

    .player-btn:hover, .player-btn.selected {
      border-color: var(--accent);
      background: #0c4a6e;
      color: var(--accent);
    }

    .start-btn {
      width: 100%;
      background: var(--accent);
      color: #0f172a;
      border: none;
      padding: 0.95rem;
      font-size: 1.1rem;
      font-weight: 700;
      border-radius: 12px;
      cursor: pointer;
    }

    .start-btn:disabled {
      opacity: 0.4;
      cursor: not-allowed;
    }

    /* GAME */
    #game {
      display: none;
      width: 100%;
      max-width: 420px;
    }

    .turn-banner {
      text-align: center;
      background: #0c4a6e;
      border: 2px solid var(--accent);
      border-radius: 12px;
      padding: 0.7rem 1rem;
      margin-bottom: 1rem;
      font-size: 1.15rem;
      font-weight: 700;
      color: var(--accent);
    }

    .turn-banner .hint {
      display: block;
      font-size: 0.75rem;
      font-weight: 400;
      color: var(--muted);
      margin-top: 0.25rem;
    }

    .info-line {
      text-align: center;
      font-size: 0.8rem;
      color: var(--muted);
      margin-bottom: 0.8rem;
      min-height: 1.2rem;
    }

    .info-line.warn { color: #fbbf24; }
    .info-line.ok { color: var(--success); }

    .section-label {
      font-size: 0.72rem;
      color: var(--muted);
      text-transform: uppercase;
      letter-spacing: 0.06em;
      text-align: center;
      margin: 0.7rem 0 0.4rem;
    }

    .deck-row {
      display: flex;
      justify-content: center;
      gap: 1rem;
      align-items: center;
    }

    .deck {
      width: 90px;
      height: 118px;
      background: linear-gradient(145deg, #334155, #1e293b);
      border: 3px solid var(--card-border);
      border-radius: 12px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      box-shadow: 0 6px 16px rgba(0,0,0,0.4);
    }

    .deck-count {
      font-size: 1.7rem;
      font-weight: 700;
      color: var(--accent);
    }

    .deck-sub {
      font-size: 0.65rem;
      color: var(--muted);
    }

    .slots {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 0.4rem;
    }

    .slot {
      aspect-ratio: 0.7;
      background: var(--card-bg);
      border: 2px solid var(--card-border);
      border-radius: 9px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 0.25rem;
      position: relative;
      overflow: hidden;
      transition: transform 0.12s, border-color 0.15s, opacity 0.15s;
    }

    .slot.filled {
      border-color: var(--accent);
      cursor: pointer;
    }

    .slot.filled:active {
      transform: scale(0.95);
    }

    .slot.special-border {
      border-color: var(--special);
    }

    .slot.empty {
      opacity: 0.3;
      border-style: dashed;
    }

    .slot.locked {
      opacity: 0.4;
      cursor: not-allowed !important;
      border-color: var(--locked);
    }

    .slot-id {
      position: absolute;
      top: 2px;
      left: 3px;
      font-size: 0.55rem;
      color: var(--muted);
      font-weight: 600;
    }

    .slot-arrows {
      font-size: 0.9rem;
      line-height: 1.15;
      color: var(--arrow);
      text-align: center;
      word-break: break-all;
    }

    .slot-special {
      font-size: 1.5rem;
      line-height: 1;
    }

    .slot-name {
      font-size: 0.55rem;
      color: var(--special);
      font-weight: 600;
      text-align: center;
      margin-top: 0.1rem;
      line-height: 1.15;
    }

    .controls {
      display: flex;
      flex-direction: column;
      gap: 0.55rem;
      margin-top: 1.3rem;
    }

    button.action {
      background: var(--accent);
      color: #0f172a;
      border: none;
      padding: 0.85rem;
      font-size: 1rem;
      font-weight: 700;
      border-radius: 12px;
      cursor: pointer;
      width: 100%;
    }

    button.action.secondary {
      background: transparent;
      border: 2px solid var(--card-border);
      color: var(--text);
    }

    button.action:disabled {
      opacity: 0.35;
      cursor: not-allowed;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.8);
      z-index: 50;
      align-items: center;
      justify-content: center;
      padding: 1rem;
    }

    .modal.show { display: flex; }

    .modal-box {
      background: var(--card-bg);
      border: 3px solid var(--accent);
      border-radius: 16px;
      padding: 1.3rem;
      width: 100%;
      max-width: 300px;
      text-align: center;
    }

    .modal-box.special { border-color: var(--special); }

    .modal-arrows {
      display: flex;
      flex-wrap: wrap;
      gap: 0.35rem;
      justify-content: center;
      margin: 0.9rem 0;
    }

    .modal-arrow {
      width: 44px;
      height: 44px;
      background: #0f172a;
      border: 2px solid #475569;
      border-radius: 9px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.5rem;
      color: var(--arrow);
    }

    .modal-icon { font-size: 3rem; margin: 0.4rem 0; }
    .modal-name { font-size: 1.25rem; font-weight: 700; color: var(--special); }
    .modal-desc { font-size: 0.85rem; color: var(--muted); margin: 0.4rem 0 1rem; }

    .modal-actions {
      display: flex;
      gap: 0.5rem;
    }

    .modal-actions button {
      flex: 1;
      padding: 0.75rem;
      border-radius: 10px;
      border: none;
      font-weight: 700;
      font-size: 0.95rem;
      cursor: pointer;
    }

    .modal-actions .yes {
      background: var(--accent);
      color: #0f172a;
    }

    .modal-actions .no {
      background: transparent;
      border: 2px solid var(--card-border);
      color: var(--text);
    }

    /* Pass overlay */
    .pass-overlay {
      display: none;
      position: fixed;
      inset: 0;
      background: #0f172a;
      z-index: 40;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 1.5rem;
      text-align: center;
    }

    .pass-overlay.show { display: flex; }

    .pass-overlay h2 {
      font-size: 1.8rem;
      color: var(--accent);
      margin-bottom: 0.5rem;
    }

    .pass-overlay p {
      color: var(--muted);
      margin-bottom: 2rem;
      font-size: 1rem;
    }

    .pass-overlay button {
      background: var(--accent);
      color: #0f172a;
      border: none;
      padding: 1rem 2.2rem;
      font-size: 1.15rem;
      font-weight: 700;
      border-radius: 14px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Windkarten</h1>

  <!-- SETUP -->
  <div id="setup">
    <h2>Wie viele Spieler?</h2>
    <div class="player-select">
      <button class="player-btn" data-n="1">1</button>
      <button class="player-btn" data-n="2">2</button>
      <button class="player-btn" data-n="3">3</button>
      <button class="player-btn" data-n="4">4</button>
    </div>
    <button class="start-btn" id="btnStart" disabled>Spiel starten</button>
  </div>

  <!-- GAME -->
  <div id="game">
    <div class="turn-banner" id="turnBanner">
      Spieler 1 ist dran
      <span class="hint">Nur dieser Spieler schaut auf das Display</span>
    </div>

    <div class="info-line" id="infoLine"></div>

    <div class="section-label">Gemeinsamer Stapel</div>
    <div class="deck-row">
      <div class="deck">
        <div class="deck-count" id="deckCount">0</div>
        <div class="deck-sub">Karten</div>
      </div>
    </div>

    <div class="section-label">Deine 5 Ablageflächen</div>
    <div class="slots" id="slots"></div>

    <div class="controls">
      <button class="action" id="btnDraw" disabled>1 Karte ziehen</button>
      <button class="action" id="btnDone">Fertig – nächster Spieler</button>
      <button class="action secondary" id="btnNewGame">Neues Spiel</button>
    </div>
  </div>

  <!-- PASS OVERLAY -->
  <div class="pass-overlay" id="passOverlay">
    <h2 id="passTitle">Spieler 2 ist dran</h2>
    <p>Bitte das Gerät weitergeben.<br>Nur der nächste Spieler soll auf den Bildschirm schauen.</p>
    <button id="btnPassConfirm">Ich bin Spieler <span id="passNum">2</span></button>
  </div>

  <!-- MODAL -->
  <div class="modal" id="modal">
    <div class="modal-box" id="modalBox">
      <div id="modalBody"></div>
      <div class="modal-actions">
        <button class="no" id="modalNo">Abbrechen</button>
        <button class="yes" id="modalYes">Ausspielen</button>
      </div>
    </div>
  </div>

  <script>
    const movementCards = [
      { id: 1,  arrows: ['↓'],                 type: 'Gerade' },
      { id: 2,  arrows: ['↓','↓'],             type: 'Gerade' },
      { id: 3,  arrows: ['↓','↓','↓'],         type: 'Gerade' },
      { id: 4,  arrows: ['↓','↓','↓','↓'],     type: 'Gerade' },
      { id: 5,  arrows: ['↑'],                 type: 'Gerade' },
      { id: 6,  arrows: ['↑','↑'],             type: 'Gerade' },
      { id: 7,  arrows: ['↑','↑','↑'],         type: 'Gerade' },
      { id: 8,  arrows: ['→','→'],             type: 'Gerade' },
      { id: 9,  arrows: ['←','←'],             type: 'Gerade' },
      { id: 10, arrows: ['↓','→','↓'],         type: 'Zickzack' },
      { id: 11, arrows: ['↓','←','↓'],         type: 'Zickzack' },
      { id: 12, arrows: ['↑','→','↑'],         type: 'Zickzack' },
      { id: 13, arrows: ['↑','←','↑'],         type: 'Zickzack' },
      { id: 14, arrows: ['→','↓','→'],         type: 'Zickzack' },
      { id: 15, arrows: ['←','↓','←'],         type: 'Zickzack' },
      { id: 16, arrows: ['↓','→','↓','→'],     type: 'Zickzack' },
      { id: 17, arrows: ['↓','←','↓','←'],     type: 'Zickzack' },
      { id: 18, arrows: ['↑','→','↑','→'],     type: 'Zickzack' },
      { id: 19, arrows: ['↑','←','↑','←'],     type: 'Zickzack' },
      { id: 20, arrows: ['↓','↓','→','↑'],     type: 'Mehrfach' },
      { id: 21, arrows: ['↓','↓','←','↑'],     type: 'Mehrfach' },
      { id: 22, arrows: ['↑','→','↓','→'],     type: 'Mehrfach' },
      { id: 23, arrows: ['↑','←','↓','←'],     type: 'Mehrfach' },
      { id: 24, arrows: ['→','↓','↓','←'],     type: 'Mehrfach' },
      { id: 25, arrows: ['←','↓','↓','→'],     type: 'Mehrfach' },
      { id: 26, arrows: ['↓','→','↓','←','↓'], type: 'Mehrfach' },
      { id: 27, arrows: ['↓','←','↓','→','↓'], type: 'Mehrfach' },
      { id: 28, arrows: ['→','↓','←','↓'],     type: 'Mehrfach' },
      { id: 29, arrows: ['←','↓','→','↓'],     type: 'Mehrfach' },
    ];

    const specialCards = [
      { id: 'S1', special: true, name: 'Kartentausch', icon: '🔄', desc: 'Tausche eine Karte mit einem Mitspieler' },
      { id: 'S2', special: true, name: 'Kartentausch', icon: '🔄', desc: 'Tausche eine Karte mit einem Mitspieler' },
      { id: 'S3', special: true, name: 'Weiße Wolke', icon: '☁️', desc: 'Schützt vor Wind für 1 Runde' },
      { id: 'S4', special: true, name: 'Weiße Wolke', icon: '☁️', desc: 'Schützt vor Wind für 1 Runde' },
      { id: 'S5', special: true, name: 'Weiße Wolke', icon: '☁️', desc: 'Schützt vor Wind für 1 Runde' },
      { id: 'S6', special: true, name: 'Graue Wolke', icon: '🌫️', desc: 'Bewege dich 2 Felder nach oben' },
      { id: 'S7', special: true, name: 'Graue Wolke', icon: '🌫️', desc: 'Bewege dich 2 Felder nach oben' },
      { id: 'S8', special: true, name: 'Graue Wolke', icon: '🌫️', desc: 'Bewege dich 2 Felder nach oben' },
      { id: 'S9',  special: true, name: 'Wirbelwind', icon: '🌀', desc: 'Alle Spieler drehen sich um 90°' },
      { id: 'S10', special: true, name: 'Wirbelwind', icon: '🌀', desc: 'Alle Spieler drehen sich um 90°' },
      { id: 'S11', special: true, name: 'Wirbelwind', icon: '🌀', desc: 'Alle Spieler drehen sich um 90°' },
      { id: 'S12', special: true, name: 'Wirbelwind', icon: '🌀', desc: 'Alle Spieler drehen sich um 90°' },
    ];

    const ALL_CARDS = movementCards.concat(specialCards);

    // State
    let playerCount = 0;
    let currentPlayer = 0;
    let deck = [];
    let playerSlots = [];
    let cardPlayedThisTurn = false;
    let drewThisTurn = false;
    let selectedSlot = null;

    // DOM
    const setupEl = document.getElementById('setup');
    const gameEl = document.getElementById('game');
    const btnStart = document.getElementById('btnStart');
    const turnBanner = document.getElementById('turnBanner');
    const infoLine = document.getElementById('infoLine');
    const deckCountEl = document.getElementById('deckCount');
    const slotsEl = document.getElementById('slots');
    const btnDraw = document.getElementById('btnDraw');
    const btnDone = document.getElementById('btnDone');
    const btnNewGame = document.getElementById('btnNewGame');
    const passOverlay = document.getElementById('passOverlay');
    const passTitle = document.getElementById('passTitle');
    const passNum = document.getElementById('passNum');
    const btnPassConfirm = document.getElementById('btnPassConfirm');
    const modal = document.getElementById('modal');
    const modalBox = document.getElementById('modalBox');
    const modalBody = document.getElementById('modalBody');
    const modalYes = document.getElementById('modalYes');
    const modalNo = document.getElementById('modalNo');

    // Setup
    document.querySelectorAll('.player-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        document.querySelectorAll('.player-btn').forEach(b => b.classList.remove('selected'));
        btn.classList.add('selected');
        playerCount = parseInt(btn.dataset.n, 10);
        btnStart.disabled = false;
      });
    });

    btnStart.addEventListener('click', startGame);

    function shuffle(arr) {
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    }

    function startGame() {
      deck = shuffle(ALL_CARDS.map(c => Object.assign({}, c)));
      playerSlots = [];
      for (let p = 0; p < playerCount; p++) {
        const slots = [null, null, null, null, null];
        for (let i = 0; i < 5; i++) {
          if (deck.length) slots[i] = deck.pop();
        }
        playerSlots.push(slots);
      }
      currentPlayer = 0;
      cardPlayedThisTurn = false;
      drewThisTurn = false;
      setupEl.style.display = 'none';
      gameEl.style.display = 'block';
      render();
    }

    function countMovementCards(slots) {
      return slots.filter(c => c && !c.special).length;
    }

    function countFilled(slots) {
      return slots.filter(c => c !== null).length;
    }

    function hasEmptySlot(slots) {
      return slots.some(c => c === null);
    }

    function render() {
      const slots = playerSlots[currentPlayer];
      const movCount = countMovementCards(slots);
      const filled = countFilled(slots);

      turnBanner.innerHTML = 'Spieler ' + (currentPlayer + 1) + ' ist dran' +
        '<span class="hint">Nur dieser Spieler schaut auf das Display</span>';

      deckCountEl.textContent = deck.length;

      if (cardPlayedThisTurn) {
        infoLine.textContent = 'Karte ausgespielt – weitere Karten sind blockiert';
        infoLine.className = 'info-line warn';
      } else if (movCount === 0 && filled > 0) {
        infoLine.textContent = 'Keine Bewegungskarten mehr – du darfst 1 Karte ziehen';
        infoLine.className = 'info-line ok';
      } else if (filled === 0) {
        infoLine.textContent = 'Hand leer – wird mit 5 neuen Karten gefüllt';
        infoLine.className = 'info-line ok';
      } else {
        infoLine.textContent = 'Du darfst 1 Karte ausspielen';
        infoLine.className = 'info-line';
      }

      const canDraw = !cardPlayedThisTurn && !drewThisTurn && movCount === 0 && hasEmptySlot(slots) && deck.length > 0;
      btnDraw.disabled = !canDraw;

      slotsEl.innerHTML = '';
      slots.forEach((card, idx) => {
        const el = document.createElement('div');
        let cls = 'slot';
        if (!card) {
          cls += ' empty';
        } else {
          cls += ' filled';
          if (card.special) cls += ' special-border';
          if (cardPlayedThisTurn) cls += ' locked';
        }
        el.className = cls;

        if (card) {
          el.innerHTML = '<div class="slot-id">' + card.id + '</div>';
          if (card.special) {
            el.innerHTML += '<div class="slot-special">' + card.icon + '</div>' +
                            '<div class="slot-name">' + card.name + '</div>';
          } else {
            el.innerHTML += '<div class="slot-arrows">' + card.arrows.join('') + '</div>';
          }

          if (!cardPlayedThisTurn) {
            el.addEventListener('click', () => openModal(idx, card));
          }
        }
        slotsEl.appendChild(el);
      });

      if (filled === 0 && deck.length > 0) {
        setTimeout(() => refillHand(), 400);
      }
    }

    function refillHand() {
      const slots = playerSlots[currentPlayer];
      for (let i = 0; i < 5; i++) {
        if (deck.length) {
          slots[i] = deck.pop();
        }
      }
      cardPlayedThisTurn = false;
      drewThisTurn = false;
      render();
    }

    btnDraw.addEventListener('click', () => {
      const slots = playerSlots[currentPlayer];
      if (drewThisTurn || cardPlayedThisTurn) return;
      if (countMovementCards(slots) > 0) return;
      if (!hasEmptySlot(slots) || deck.length === 0) return;

      for (let i = 0; i < 5; i++) {
        if (slots[i] === null) {
          slots[i] = deck.pop();
          break;
        }
      }
      drewThisTurn = true;
      render();
    });

    function openModal(idx, card) {
      if (cardPlayedThisTurn) return;
      selectedSlot = idx;
      modalBox.className = 'modal-box' + (card.special ? ' special' : '');

      if (card.special) {
        modalBody.innerHTML =
          '<div class="modal-icon">' + card.icon + '</div>' +
          '<div class="modal-name">' + card.name + '</div>' +
          '<div class="modal-desc">' + card.desc + '</div>' +
          '<div style="font-size:0.8rem;color:var(--muted)">Karte ' + card.id + '</div>';
      } else {
        const boxes = card.arrows.map(a => '<div class="modal-arrow">' + a + '</div>').join('');
        modalBody.innerHTML =
          '<div style="font-size:0.85rem;color:var(--muted);margin-bottom:0.2rem">Karte ' + card.id + ' · ' + card.type + '</div>' +
          '<div class="modal-arrows">' + boxes + '</div>';
      }
      modal.classList.add('show');
    }

    function closeModal() {
      modal.classList.remove('show');
      selectedSlot = null;
    }

    modalNo.addEventListener('click', closeModal);
    modal.addEventListener('click', e => { if (e.target === modal) closeModal(); });

    modalYes.addEventListener('click', () => {
      if (selectedSlot === null || cardPlayedThisTurn) return;
      const card = playerSlots[currentPlayer][selectedSlot];
      playerSlots[currentPlayer][selectedSlot] = null;
      deck.unshift(card);
      cardPlayedThisTurn = true;
      closeModal();
      render();
    });

    btnDone.addEventListener('click', () => {
      if (playerCount === 1) {
        cardPlayedThisTurn = false;
        drewThisTurn = false;
        render();
        return;
      }
      const next = (currentPlayer + 1) % playerCount;
      passTitle.textContent = 'Spieler ' + (next + 1) + ' ist dran';
      passNum.textContent = (next + 1);
      passOverlay.classList.add('show');
    });

    btnPassConfirm.addEventListener('click', () => {
      currentPlayer = (currentPlayer + 1) % playerCount;
      cardPlayedThisTurn = false;
      drewThisTurn = false;
      passOverlay.classList.remove('show');
      render();
    });

    btnNewGame.addEventListener('click', () => {
      gameEl.style.display = 'none';
      setupEl.style.display = 'block';
      playerCount = 0;
      btnStart.disabled = true;
      document.querySelectorAll('.player-btn').forEach(b => b.classList.remove('selected'));
    });
  </script>
</body>
</html>
