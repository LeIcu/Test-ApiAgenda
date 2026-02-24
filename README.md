<!DOCTYPE html>
<html lang="ro">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ApiAgenda</title>
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js"></script>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      :root {
        --honey: #f5a623;
        --honey-dark: #d4881a;
        --honey-light: #fdf3e3;
        --brown: #3e2a0f;
        --brown-light: #6b4c2a;
        --white: #ffffff;
        --gray: #f0f0f0;
        --gray-dark: #888;
        --danger: #e74c3c;
        --success: #27ae60;
        --card-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
      }

      body {
        font-family: "Segoe UI", sans-serif;
        background: var(--honey-light);
        color: var(--brown);
        min-height: 100vh;
      }

      header {
        background: linear-gradient(135deg, var(--brown), var(--brown-light));
        color: white;
        padding: 0 2rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 64px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
        position: sticky;
        top: 0;
        z-index: 100;
      }

      .logo {
        display: flex;
        align-items: center;
        gap: 0.5rem;
        font-size: 1.4rem;
        font-weight: 700;
        letter-spacing: 1px;
      }

      .logo span {
        color: var(--honey);
      }

      .header-right {
        display: flex;
        align-items: center;
        gap: 1rem;
      }

      header nav {
        display: flex;
        gap: 0.6rem;
      }

      header nav button {
        background: transparent;
        border: 2px solid rgba(255, 255, 255, 0.3);
        color: white;
        padding: 0.4rem 1rem;
        border-radius: 20px;
        cursor: pointer;
        font-size: 0.9rem;
        transition: all 0.2s;
      }

      header nav button:hover,
      header nav button.active {
        background: var(--honey);
        border-color: var(--honey);
        color: var(--brown);
        font-weight: 600;
      }

      .lang-switcher {
        display: flex;
        gap: 0.3rem;
        background: rgba(255, 255, 255, 0.1);
        border-radius: 20px;
        padding: 0.25rem;
      }

      .lang-btn {
        background: transparent;
        border: none;
        color: rgba(255, 255, 255, 0.7);
        padding: 0.25rem 0.6rem;
        border-radius: 16px;
        cursor: pointer;
        font-size: 0.8rem;
        font-weight: 600;
        transition: all 0.2s;
      }

      .lang-btn.active {
        background: var(--honey);
        color: var(--brown);
      }

      main {
        max-width: 1200px;
        margin: 0 auto;
        padding: 2rem 1rem;
      }

      .view {
        display: none;
      }

      .view.active {
        display: block;
      }

      .stats-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 1.2rem;
        margin-bottom: 2rem;
      }

      .stat-card {
        background: white;
        border-radius: 16px;
        padding: 1.5rem;
        box-shadow: var(--card-shadow);
        display: flex;
        align-items: center;
        gap: 1rem;
        border-left: 5px solid var(--honey);
      }

      .stat-icon {
        font-size: 2rem;
      }

      .stat-info h3 {
        font-size: 1.8rem;
        font-weight: 700;
        color: var(--brown);
      }

      .stat-info p {
        font-size: 0.85rem;
        color: var(--gray-dark);
      }

      .section-title {
        font-size: 1.2rem;
        font-weight: 700;
        color: var(--brown);
        margin-bottom: 1rem;
        display: flex;
        align-items: center;
        gap: 0.5rem;
      }

      .hives-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 1.5rem;
        flex-wrap: wrap;
        gap: 1rem;
      }

      .search-bar input {
        padding: 0.5rem 1rem;
        border: 2px solid #ddd;
        border-radius: 20px;
        font-size: 0.9rem;
        outline: none;
        transition: border-color 0.2s;
        width: 220px;
      }

      .search-bar input:focus {
        border-color: var(--honey);
      }

      .btn {
        padding: 0.6rem 1.4rem;
        border: none;
        border-radius: 20px;
        cursor: pointer;
        font-size: 0.9rem;
        font-weight: 600;
        transition: all 0.2s;
      }

      .btn-primary {
        background: var(--honey);
        color: var(--brown);
      }

      .btn-primary:hover {
        background: var(--honey-dark);
        transform: translateY(-1px);
      }

      .btn-danger {
        background: var(--danger);
        color: white;
      }

      .btn-danger:hover {
        background: #c0392b;
      }

      .btn-outline {
        background: transparent;
        border: 2px solid var(--honey);
        color: var(--brown);
      }

      .btn-outline:hover {
        background: var(--honey);
      }

      .btn-white {
        background: transparent;
        border: 2px solid rgba(255, 255, 255, 0.5);
        color: white;
      }

      .btn-white:hover {
        background: rgba(255, 255, 255, 0.15);
      }

      .hives-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
        gap: 1.5rem;
      }

      .hive-card {
        background: white;
        border-radius: 16px;
        box-shadow: var(--card-shadow);
        overflow: hidden;
        transition: transform 0.2s, box-shadow 0.2s;
        cursor: pointer;
      }

      .hive-card:hover {
        transform: translateY(-4px);
        box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
      }

      .hive-card-header {
        background: linear-gradient(135deg, var(--honey), var(--honey-dark));
        padding: 1.2rem;
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
      }

      .hive-name {
        font-size: 1.1rem;
        font-weight: 700;
        color: var(--brown);
      }

      .hive-id {
        font-size: 0.75rem;
        color: var(--brown-light);
        margin-top: 2px;
      }

      .hive-status {
        padding: 0.25rem 0.7rem;
        border-radius: 20px;
        font-size: 0.75rem;
        font-weight: 600;
      }

      .status-healthy {
        background: var(--success);
        color: white;
      }

      .status-attention {
        background: #f39c12;
        color: white;
      }

      .status-critical {
        background: var(--danger);
        color: white;
      }

      .hive-card-body {
        padding: 1.2rem;
      }

      .hive-stats {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 0.8rem;
        margin-bottom: 1rem;
      }

      .hive-stat {
        text-align: center;
        background: var(--honey-light);
        border-radius: 10px;
        padding: 0.6rem;
      }

      .hive-stat-value {
        font-size: 1.2rem;
        font-weight: 700;
        color: var(--brown);
      }

      .hive-stat-label {
        font-size: 0.72rem;
        color: var(--gray-dark);
        margin-top: 2px;
      }

      .hive-card-footer {
        padding: 0.8rem 1.2rem;
        border-top: 1px solid #f0f0f0;
        display: flex;
        justify-content: space-between;
        align-items: center;
      }

      .hive-date {
        font-size: 0.78rem;
        color: var(--gray-dark);
      }

      .hive-actions {
        display: flex;
        gap: 0.4rem;
      }

      .icon-btn {
        background: none;
        border: none;
        cursor: pointer;
        font-size: 1rem;
        padding: 0.3rem;
        border-radius: 8px;
        transition: background 0.2s;
      }

      .icon-btn:hover {
        background: var(--gray);
      }

      .modal-overlay {
        display: none;
        position: fixed;
        inset: 0;
        background: rgba(0, 0, 0, 0.5);
        z-index: 200;
        align-items: center;
        justify-content: center;
        padding: 1rem;
      }

      .modal-overlay.open {
        display: flex;
      }

      .modal {
        background: white;
        border-radius: 20px;
        width: 100%;
        max-width: 560px;
        max-height: 90vh;
        overflow-y: auto;
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
      }

      .modal-header {
        padding: 1.5rem;
        border-bottom: 1px solid #f0f0f0;
        display: flex;
        justify-content: space-between;
        align-items: center;
        position: sticky;
        top: 0;
        background: white;
        border-radius: 20px 20px 0 0;
      }

      .modal-header h2 {
        font-size: 1.2rem;
        color: var(--brown);
      }

      .modal-close {
        background: none;
        border: none;
        font-size: 1.5rem;
        cursor: pointer;
        color: var(--gray-dark);
        line-height: 1;
      }

      .modal-body {
        padding: 1.5rem;
      }

      .form-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 1rem;
      }

      .form-group {
        display: flex;
        flex-direction: column;
        gap: 0.4rem;
      }

      .form-group.full {
        grid-column: 1 / -1;
      }

      .form-group label {
        font-size: 0.85rem;
        font-weight: 600;
        color: var(--brown-light);
      }

      .form-group input,
      .form-group select,
      .form-group textarea {
        padding: 0.6rem 0.9rem;
        border: 2px solid #e0e0e0;
        border-radius: 10px;
        font-size: 0.9rem;
        outline: none;
        transition: border-color 0.2s;
        font-family: inherit;
      }

      .form-group input:focus,
      .form-group select:focus,
      .form-group textarea:focus {
        border-color: var(--honey);
      }

      .form-group textarea {
        resize: vertical;
        min-height: 80px;
      }

      .modal-footer {
        padding: 1rem 1.5rem;
        border-top: 1px solid #f0f0f0;
        display: flex;
        justify-content: flex-end;
        gap: 0.8rem;
      }

      .detail-header {
        background: linear-gradient(135deg, var(--brown), var(--brown-light));
        color: white;
        border-radius: 16px;
        padding: 2rem;
        margin-bottom: 1.5rem;
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        flex-wrap: wrap;
        gap: 1rem;
      }

      .detail-header h1 {
        font-size: 1.8rem;
      }

      .detail-header p {
        opacity: 0.8;
        margin-top: 0.3rem;
      }

      .detail-grid {
        display: grid;
        grid-template-columns: 2fr 1fr;
        gap: 1.5rem;
      }

      @media (max-width: 700px) {
        .detail-grid {
          grid-template-columns: 1fr;
        }

        .form-grid {
          grid-template-columns: 1fr;
        }

        header nav button span.nav-label {
          display: none;
        }
      }

      .detail-card {
        background: white;
        border-radius: 16px;
        padding: 1.5rem;
        box-shadow: var(--card-shadow);
        margin-bottom: 1.5rem;
      }

      .detail-stats {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        gap: 1rem;
        margin-top: 1rem;
      }

      .detail-stat {
        text-align: center;
        background: var(--honey-light);
        border-radius: 12px;
        padding: 1rem;
        border: 2px solid transparent;
        transition: border-color 0.2s;
      }

      .detail-stat:hover {
        border-color: var(--honey);
      }

      .detail-stat-value {
        font-size: 1.6rem;
        font-weight: 700;
        color: var(--brown);
      }

      .detail-stat-label {
        font-size: 0.8rem;
        color: var(--gray-dark);
        margin-top: 4px;
      }

      .log-entry {
        display: flex;
        gap: 1rem;
        padding: 0.9rem 0;
        border-bottom: 1px solid #f5f5f5;
        align-items: flex-start;
      }

      .log-entry:last-child {
        border-bottom: none;
      }

      .log-icon {
        font-size: 1.2rem;
        width: 36px;
        height: 36px;
        background: var(--honey-light);
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0;
      }

      .log-content {
        flex: 1;
      }

      .log-action {
        font-weight: 600;
        font-size: 0.9rem;
        color: var(--brown);
      }

      .log-note {
        font-size: 0.82rem;
        color: var(--gray-dark);
        margin-top: 2px;
      }

      .log-date {
        font-size: 0.75rem;
        color: var(--gray-dark);
        white-space: nowrap;
      }

      /* ── PRINT QR LABEL ─────────────────────────────────────────────── */
      .qr-modal-inner {
        max-width: 420px;
      }

      .qr-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.8rem;
        padding: 1rem 0;
      }

      .qr-label-card {
        border: 3px dashed var(--honey);
        border-radius: 16px;
        padding: 1.5rem;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.6rem;
        background: white;
        width: 100%;
        max-width: 280px;
        position: relative;
      }

      .qr-label-card .qr-logo {
        font-size: 1rem;
        font-weight: 700;
        color: var(--brown);
        letter-spacing: 1px;
      }

      .qr-label-card .qr-logo span {
        color: var(--honey-dark);
      }

      .qr-label-card canvas {
        border-radius: 8px;
        border: 4px solid white;
        box-shadow: var(--card-shadow);
      }

      .qr-label-card .qr-hive-title {
        font-size: 1.1rem;
        font-weight: 700;
        color: var(--brown);
        text-align: center;
      }

      .qr-label-card .qr-hive-id {
        font-size: 0.75rem;
        color: var(--gray-dark);
        font-family: monospace;
      }

      .qr-label-card .qr-scan-hint {
        font-size: 0.72rem;
        color: var(--gray-dark);
        text-align: center;
      }

      .qr-label-card .qr-cut-line {
        position: absolute;
        top: -10px;
        left: 50%;
        transform: translateX(-50%);
        font-size: 0.65rem;
        color: var(--gray-dark);
        background: var(--honey-light);
        padding: 0 0.4rem;
      }

      .qr-actions {
        display: flex;
        gap: 0.8rem;
        flex-wrap: wrap;
        justify-content: center;
        margin-top: 0.5rem;
      }

      /* ── PRINT STYLES ───────────────────────────────────────────────── */
      @media print {
        body * {
          visibility: hidden;
        }

        #print-area,
        #print-area * {
          visibility: visible;
        }

        #print-area {
          position: fixed;
          inset: 0;
          display: flex;
          align-items: center;
          justify-content: center;
          background: white;
        }

        .print-label {
          border: 3px solid #333;
          border-radius: 12px;
          padding: 20px;
          display: flex;
          flex-direction: column;
          align-items: center;
          gap: 8px;
          width: 260px;
          font-family: "Segoe UI", sans-serif;
        }

        .print-label .p-logo {
          font-size: 14px;
          font-weight: 700;
          color: #3e2a0f;
          letter-spacing: 1px;
        }

        .print-label .p-name {
          font-size: 16px;
          font-weight: 700;
          color: #3e2a0f;
          text-align: center;
        }

        .print-label .p-id {
          font-size: 10px;
          color: #888;
          font-family: monospace;
        }

        .print-label .p-hint {
          font-size: 9px;
          color: #888;
          text-align: center;
        }
      }

      #print-area {
        display: none;
      }

      .empty-state {
        text-align: center;
        padding: 4rem 2rem;
        color: var(--gray-dark);
      }

      .empty-state .empty-icon {
        font-size: 4rem;
        margin-bottom: 1rem;
      }

      .empty-state h3 {
        font-size: 1.2rem;
        color: var(--brown);
        margin-bottom: 0.5rem;
      }

      .toast {
        position: fixed;
        bottom: 2rem;
        right: 2rem;
        background: var(--brown);
        color: white;
        padding: 0.8rem 1.5rem;
        border-radius: 12px;
        font-size: 0.9rem;
        z-index: 999;
        transform: translateY(100px);
        opacity: 0;
        transition: all 0.3s;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
      }

      .toast.show {
        transform: translateY(0);
        opacity: 1;
      }

      .toast.success {
        background: var(--success);
      }

      .toast.error {
        background: var(--danger);
      }

      .back-btn {
        display: flex;
        align-items: center;
        gap: 0.5rem;
        background: none;
        border: none;
        cursor: pointer;
        font-size: 0.95rem;
        color: var(--brown-light);
        font-weight: 600;
        margin-bottom: 1.2rem;
        padding: 0;
        transition: color 0.2s;
      }

      .back-btn:hover {
        color: var(--brown);
      }

      .inspection-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
        gap: 0.8rem;
        margin-bottom: 1rem;
      }

      .inspection-item {
        background: var(--honey-light);
        border-radius: 10px;
        padding: 0.8rem;
        text-align: center;
      }

      .inspection-item label {
        display: block;
        font-size: 0.75rem;
        color: var(--gray-dark);
        margin-bottom: 0.3rem;
      }

      .inspection-item input {
        width: 100%;
        padding: 0.4rem;
        border: 2px solid #ddd;
        border-radius: 8px;
        text-align: center;
        font-size: 1rem;
        font-weight: 600;
        color: var(--brown);
        outline: none;
      }

      .inspection-item input:focus {
        border-color: var(--honey);
      }

      ::-webkit-scrollbar {
        width: 6px;
      }

      ::-webkit-scrollbar-track {
        background: var(--honey-light);
      }

      ::-webkit-scrollbar-thumb {
        background: var(--honey);
        border-radius: 3px;
      }
    </style>
  </head>
  <body>
    <header>
      <div class="logo">🍯 <span>Api</span>Agenda</div>
      <div class="header-right">
        <nav>
          <button class="active" onclick="showView('dashboard')">
            📊 <span class="nav-label" data-t="nav_dashboard">Panou</span>
          </button>
          <button onclick="showView('hives')">
            🐝 <span class="nav-label" data-t="nav_hives">Stupi</span>
          </button>
        </nav>
        <div class="lang-switcher">
          <button class="lang-btn active" onclick="setLang('ro')">RO</button>
          <button class="lang-btn" onclick="setLang('en')">EN</button>
        </div>
      </div>
    </header>

    <main>
      <!-- DASHBOARD -->
      <div class="view active" id="view-dashboard">
        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-icon">🏠</div>
            <div class="stat-info">
              <h3 id="stat-total">0</h3>
              <p data-t="stat_total">Total Stupi</p>
            </div>
          </div>
          <div class="stat-card">
            <div class="stat-icon">✅</div>
            <div class="stat-info">
              <h3 id="stat-healthy">0</h3>
              <p data-t="stat_healthy">Stupi Sănătoși</p>
            </div>
          </div>
          <div class="stat-card">
            <div class="stat-icon">🍯</div>
            <div class="stat-info">
              <h3 id="stat-honey">0 kg</h3>
              <p data-t="stat_honey">Miere Totală</p>
            </div>
          </div>
          <div class="stat-card">
            <div class="stat-icon">⚠️</div>
            <div class="stat-info">
              <h3 id="stat-attention">0</h3>
              <p data-t="stat_attention">Necesită Atenție</p>
            </div>
          </div>
        </div>
        <div class="section-title">
          🕐 <span data-t="recent_activity">Activitate Recentă</span>
        </div>
        <div class="detail-card">
          <div id="recent-log"></div>
        </div>
      </div>

      <!-- HIVES -->
      <div class="view" id="view-hives">
        <div class="hives-header">
          <div class="section-title" style="margin: 0">
            🐝 <span data-t="my_hives">Stupii Mei</span>
          </div>
          <div style="display: flex; gap: 0.8rem; align-items: center">
            <div class="search-bar">
              <input
                type="text"
                id="search-input"
                data-placeholder="search_placeholder"
                placeholder="🔍 Caută stupi..."
                oninput="renderHives()"
              />
            </div>
            <button class="btn btn-primary" onclick="openAddModal()">
              + <span data-t="add_hive">Adaugă Stup</span>
            </button>
          </div>
        </div>
        <div class="hives-grid" id="hives-grid"></div>
      </div>

      <!-- DETAIL -->
      <div class="view" id="view-detail">
        <button class="back-btn" onclick="showView('hives')">
          ← <span data-t="back_hives">Înapoi la Stupi</span>
        </button>
        <div class="detail-header">
          <div>
            <h1 id="detail-name"></h1>
            <p id="detail-location"></p>
            <p id="detail-queen" style="margin-top: 0.3rem"></p>
          </div>
          <div style="display: flex; gap: 0.6rem; flex-wrap: wrap">
            <button class="btn btn-primary" onclick="openQRModal()">
              📱 <span data-t="qr_code">Cod QR</span>
            </button>
            <button class="btn btn-white" onclick="openInspectionModal()">
              + <span data-t="inspection">Inspecție</span>
            </button>
            <button
              class="btn btn-white"
              onclick="openEditModal(currentHiveId)"
            >
              ✏️ <span data-t="edit">Editează</span>
            </button>
          </div>
        </div>
        <div class="detail-grid">
          <div>
            <div class="detail-card">
              <div class="section-title">
                📊 <span data-t="current_stats">Statistici Curente</span>
              </div>
              <div class="detail-stats" id="detail-stats"></div>
            </div>
            <div class="detail-card">
              <div class="section-title">
                📋 <span data-t="inspection_log">Jurnal Inspecții</span>
              </div>
              <div id="detail-log"></div>
            </div>
          </div>
          <div>
            <div class="detail-card">
              <div class="section-title">
                ℹ️ <span data-t="info">Informații</span>
              </div>
              <div id="detail-info"></div>
            </div>
            <div class="detail-card" style="text-align: center">
              <div class="section-title" style="justify-content: center">
                📱 <span data-t="qr_code">Cod QR</span>
              </div>
              <div id="detail-qr-mini" style="margin-top: 0.5rem"></div>
              <button
                class="btn btn-outline"
                style="margin-top: 0.8rem; font-size: 0.8rem"
                onclick="openQRModal()"
              >
                <span data-t="print_qr">Printează / Descarcă</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- ADD/EDIT HIVE MODAL -->
    <div class="modal-overlay" id="hive-modal">
      <div class="modal">
        <div class="modal-header">
          <h2 id="hive-modal-title" data-t="add_hive_title">
            Adaugă Stup Nou
          </h2>
          <button class="modal-close" onclick="closeHiveModal()">×</button>
        </div>
        <div class="modal-body">
          <div class="form-grid">
            <div class="form-group">
              <label data-t="f_name">Nume Stup *</label>
              <input
                type="text"
                id="f-name"
                data-placeholder="f_name_ph"
                placeholder="ex: Stupul Alpha"
              />
            </div>
            <div class="form-group">
              <label data-t="f_location">Locație</label>
              <input
                type="text"
                id="f-location"
                data-placeholder="f_loc_ph"
                placeholder="ex: Câmpul de Nord"
              />
            </div>
            <div class="form-group">
              <label data-t="f_status">Status</label>
              <select id="f-status">
                <option value="healthy" data-t="status_healthy">
                  ✅ Sănătos
                </option>
                <option value="attention" data-t="status_attention">
                  ⚠️ Necesită Atenție
                </option>
                <option value="critical" data-t="status_critical">
                  🚨 Critic
                </option>
              </select>
            </div>
            <div class="form-group">
              <label data-t="f_queen_age">Vârsta Mătcii (ani)</label>
              <input
                type="number"
                id="f-queen-age"
                placeholder="ex: 1"
                min="0"
              />
            </div>
            <div class="form-group">
              <label data-t="f_queen_type">Rasa Mătcii</label>
              <input
                type="text"
                id="f-queen-type"
                data-placeholder="f_queen_ph"
                placeholder="ex: Carniolan"
              />
            </div>
            <div class="form-group">
              <label data-t="f_frames">Total Rame</label>
              <input
                type="number"
                id="f-frames"
                placeholder="ex: 10"
                min="0"
              />
            </div>
            <div class="form-group">
              <label data-t="f_honey_frames">Rame cu Miere</label>
              <input
                type="number"
                id="f-honey-frames"
                placeholder="ex: 4"
                min="0"
              />
            </div>
            <div class="form-group">
              <label data-t="f_brood_frames">Rame cu Puiet</label>
              <input
                type="number"
                id="f-brood-frames"
                placeholder="ex: 6"
                min="0"
              />
            </div>
            <div class="form-group">
              <label data-t="f_honey">Miere Produsă Total (kg)</label>
              <input
                type="number"
                id="f-honey"
                placeholder="ex: 12.5"
                min="0"
                step="0.1"
              />
            </div>
            <div class="form-group">
              <label data-t="f_population">Populație Estimată</label>
              <select id="f-population">
                <option value="Scăzută" data-t="pop_low">Scăzută</option>
                <option value="Medie" data-t="pop_medium" selected>
                  Medie
                </option>
                <option value="Ridicată" data-t="pop_high">Ridicată</option>
                <option value="Foarte Ridicată" data-t="pop_very_high">
                  Foarte Ridicată
                </option>
              </select>
            </div>
            <div class="form-group full">
              <label data-t="f_notes">Observații</label>
              <textarea
                id="f-notes"
                data-placeholder="f_notes_ph"
                placeholder="Orice observații..."
              ></textarea>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button class="btn btn-outline" onclick="closeHiveModal()">
            <span data-t="cancel">Anulează</span>
          </button>
          <button class="btn btn-primary" onclick="saveHive()">
            <span data-t="save">Salvează</span>
          </button>
        </div>
      </div>
    </div>

    <!-- INSPECTION MODAL -->
    <div class="modal-overlay" id="inspection-modal">
      <div class="modal">
        <div class="modal-header">
          <h2 data-t="new_inspection">📋 Inspecție Nouă</h2>
          <button class="modal-close" onclick="closeInspectionModal()">
            ×
          </button>
        </div>
        <div class="modal-body">
          <div class="inspection-grid">
            <div class="inspection-item">
              <label data-t="i_frames">🖼️ Total Rame</label>
              <input type="number" id="i-frames" min="0" placeholder="10" />
            </div>
            <div class="inspection-item">
              <label data-t="i_honey_frames">🍯 Rame Miere</label>
              <input
                type="number"
                id="i-honey-frames"
                min="0"
                placeholder="4"
              />
            </div>
            <div class="inspection-item">
              <label data-t="i_brood_frames">🐣 Rame Puiet</label>
              <input
                type="number"
                id="i-brood-frames"
                min="0"
                placeholder="6"
              />
            </div>
            <div class="inspection-item">
              <label data-t="i_honey_kg">🍯 Miere (kg)</label>
              <input
                type="number"
                id="i-honey"
                min="0"
                step="0.1"
                placeholder="0"
              />
            </div>
          </div>
          <div class="form-grid" style="margin-top: 1rem">
            <div class="form-group">
              <label data-t="i_queen">Matcă Văzută?</label>
              <select id="i-queen">
                <option value="Da" data-t="yes">Da ✅</option>
                <option value="Nu" data-t="no">Nu ❌</option>
                <option value="Nesigur" data-t="unsure">Nesigur 🤔</option>
              </select>
            </div>
            <div class="form-group">
              <label data-t="i_temper">Temperament</label>
              <select id="i-temper">
                <option value="Calm" data-t="calm">Calm 😊</option>
                <option value="Moderat" data-t="moderate">Moderat 😐</option>
                <option value="Agresiv" data-t="aggressive">
                  Agresiv 😠
                </option>
              </select>
            </div>
            <div class="form-group">
              <label data-t="i_status">Actualizează Status</label>
              <select id="i-status">
                <option value="healthy" data-t="status_healthy">
                  ✅ Sănătos
                </option>
                <option value="attention" data-t="status_attention">
                  ⚠️ Necesită Atenție
                </option>
                <option value="critical" data-t="status_critical">
                  🚨 Critic
                </option>
              </select>
            </div>
            <div class="form-group">
              <label data-t="i_varroa">Nivel Varroa</label>
              <select id="i-varroa">
                <option value="Absent" data-t="none">Absent</option>
                <option value="Scăzut" data-t="low">Scăzut</option>
                <option value="Mediu" data-t="medium">Mediu</option>
                <option value="Ridicat" data-t="high">Ridicat</option>
              </select>
            </div>
            <div class="form-group full">
              <label data-t="f_notes">Observații</label>
              <textarea
                id="i-notes"
                data-placeholder="i_notes_ph"
                placeholder="Observații din această inspecție..."
              ></textarea>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button class="btn btn-outline" onclick="closeInspectionModal()">
            <span data-t="cancel">Anulează</span>
          </button>
          <button class="btn btn-primary" onclick="saveInspection()">
            <span data-t="save_inspection">Salvează Inspecția</span>
          </button>
        </div>
      </div>
    </div>

    <!-- QR MODAL -->
    <div class="modal-overlay" id="qr-modal">
      <div class="modal qr-modal-inner">
        <div class="modal-header">
          <h2>📱 <span data-t="qr_title">Cod QR Stup</span></h2>
          <button class="modal-close" onclick="closeQRModal()">×</button>
        </div>
        <div class="modal-body">
          <div class="qr-container">
            <!-- Printable label card -->
            <div class="qr-label-card" id="qr-label-card">
              <div class="qr-cut-line" data-t="cut_here">✂ taie aici</div>
              <div class="qr-logo">🍯 <span>Api</span>Agenda</div>
              <canvas id="qr-canvas"></canvas>
              <div class="qr-hive-title" id="qr-hive-name-display"></div>
              <div class="qr-hive-id" id="qr-hive-id-display"></div>
              <div class="qr-scan-hint" data-t="scan_hint">
                Scanează pentru a vedea detaliile stupului
              </div>
            </div>
            <div class="qr-actions">
              <button class="btn btn-primary" onclick="printQR()">
                🖨️ <span data-t="print">Printează</span>
              </button>
              <button class="btn btn-outline" onclick="downloadQR()">
                ⬇️ <span data-t="download">Descarcă PNG</span>
              </button>
            </div>
            <p
              style="
                font-size: 0.78rem;
                color: var(--gray-dark);
                text-align: center;
              "
              data-t="qr_hint"
            >
              Printează și lipește eticheta direct pe stupul fizic.
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- HIDDEN PRINT AREA -->
    <div id="print-area">
      <div class="print-label" id="print-label-inner"></div>
    </div>

    <div class="toast" id="toast"></div>

    <script>
      // ── TRANSLATIONS ────────────────────────────────────────────────────
      const T = {
        ro: {
          nav_dashboard: "Panou",
          nav_hives: "Stupi",
          stat_total: "Total Stupi",
          stat_healthy: "Stupi Sănătoși",
          stat_honey: "Miere Totală",
          stat_attention: "Necesită Atenție",
          recent_activity: "Activitate Recentă",
          my_hives: "Stupii Mei",
          search_placeholder: "🔍 Caută stupi...",
          add_hive: "Adaugă Stup",
          add_hive_title: "Adaugă Stup Nou",
          edit_hive_title: "Editează Stupul",
          back_hives: "Înapoi la Stupi",
          qr_code: "Cod QR",
          qr_title: "Cod QR Stup",
          inspection: "Inspecție",
          edit: "Editează",
          current_stats: "Statistici Curente",
          inspection_log: "Jurnal Inspecții",
          info: "Informații",
          print_qr: "Printează / Descarcă",
          f_name: "Nume Stup *",
          f_name_ph: "ex: Stupul Alpha",
          f_location: "Locație",
          f_loc_ph: "ex: Câmpul de Nord",
          f_status: "Status",
          f_queen_age: "Vârsta Mătcii (ani)",
          f_queen_type: "Rasa Mătcii",
          f_queen_ph: "ex: Carniolan",
          f_frames: "Total Rame",
          f_honey_frames: "Rame cu Miere",
          f_brood_frames: "Rame cu Puiet",
          f_honey: "Miere Produsă Total (kg)",
          f_population: "Populație Estimată",
          f_notes: "Observații",
          f_notes_ph: "Orice observații...",
          cancel: "Anulează",
          save: "Salvează",
          new_inspection: "📋 Inspecție Nouă",
          i_frames: "🖼️ Total Rame",
          i_honey_frames: "🍯 Rame Miere",
          i_brood_frames: "🐣 Rame Puiet",
          i_honey_kg: "🍯 Miere (kg)",
          i_queen: "Matcă Văzută?",
          i_temper: "Temperament",
          i_status: "Actualizează Status",
          i_varroa: "Nivel Varroa",
          i_notes_ph: "Observații din această inspecție...",
          save_inspection: "Salvează Inspecția",
          status_healthy: "✅ Sănătos",
          status_attention: "⚠️ Necesită Atenție",
          status_critical: "🚨 Critic",
          status_label_healthy: "Sănătos",
          status_label_attention: "Atenție",
          status_label_critical: "Critic",
          pop_low: "Scăzută",
          pop_medium: "Medie",
          pop_high: "Ridicată",
          pop_very_high: "Foarte Ridicată",
          yes: "Da ✅",
          no: "Nu ❌",
          unsure: "Nesigur 🤔",
          calm: "Calm 😊",
          moderate: "Moderat 😐",
          aggressive: "Agresiv 😠",
          none: "Absent",
          low: "Scăzut",
          medium: "Mediu",
          high: "Ridicat",
          cut_here: "✂ taie aici",
          scan_hint: "Scanează pentru a vedea detaliile stupului",
          print: "Printează",
          download: "Descarcă PNG",
          qr_hint: "Printează și lipește eticheta direct pe stupul fizic.",
          no_location: "Fără locație",
          no_location_set: "Locație nesetată",
          unknown: "Necunoscut",
          queen_suffix: "matcă",
          yr_old: "ani",
          never: "Niciodată",
          no_inspections: "Nicio inspecție înregistrată.",
          no_activity: "Nicio activitate. Adaugă un stup!",
          no_hives: "Niciun stup adăugat",
          no_search: "Niciun stup nu corespunde căutării",
          no_search_sub: "Încearcă alt termen de căutare",
          no_hives_sub: "Apasă '+ Adaugă Stup' pentru a începe",
          delete_confirm: "Ștergi acest stup? Acțiunea este ireversibilă.",
          hive_added: "Stup adăugat!",
          hive_updated: "Stup actualizat!",
          hive_deleted: "Stup șters",
          inspection_saved: "Inspecție salvată!",
          name_required: "Introdu numele stupului",
          log_created: "Stup creat",
          log_updated: "Stup actualizat",
          log_inspection: "Inspecție înregistrată",
          id_label: "ID",
          created_label: "Creat",
          last_inspected: "Ultima Inspecție",
          notes_label: "Observații",
          stat_frames: "🖼️ Total Rame",
          stat_honey_frames: "🍯 Rame Miere",
          stat_brood_frames: "🐣 Rame Puiet",
          stat_total_honey: "🍯 Miere Totală",
          stat_population: "🐝 Populație",
          stat_status: "💚 Status",
        },
        en: {
          nav_dashboard: "Dashboard",
          nav_hives: "Hives",
          stat_total: "Total Hives",
          stat_healthy: "Healthy Hives",
          stat_honey: "Total Honey",
          stat_attention: "Need Attention",
          recent_activity: "Recent Activity",
          my_hives: "My Hives",
          search_placeholder: "🔍 Search hives...",
          add_hive: "Add Hive",
          add_hive_title: "Add New Hive",
          edit_hive_title: "Edit Hive",
          back_hives: "Back to Hives",
          qr_code: "QR Code",
          qr_title: "Hive QR Code",
          inspection: "Inspection",
          edit: "Edit",
          current_stats: "Current Stats",
          inspection_log: "Inspection Log",
          info: "Information",
          print_qr: "Print / Download",
          f_name: "Hive Name *",
          f_name_ph: "e.g. Alpha Hive",
          f_location: "Location",
          f_loc_ph: "e.g. North Field",
          f_status: "Status",
          f_queen_age: "Queen Age (years)",
          f_queen_type: "Queen Type",
          f_queen_ph: "e.g. Carniolan",
          f_frames: "Total Frames",
          f_honey_frames: "Honey Frames",
          f_brood_frames: "Brood Frames",
          f_honey: "Total Honey Produced (kg)",
          f_population: "Est. Population",
          f_notes: "Notes",
          f_notes_ph: "Any observations...",
          cancel: "Cancel",
          save: "Save",
          new_inspection: "📋 New Inspection",
          i_frames: "🖼️ Total Frames",
          i_honey_frames: "🍯 Honey Frames",
          i_brood_frames: "🐣 Brood Frames",
          i_honey_kg: "🍯 Honey (kg)",
          i_queen: "Queen Seen?",
          i_temper: "Temper",
          i_status: "Update Status",
          i_varroa: "Varroa Level",
          i_notes_ph: "Observations from this inspection...",
          save_inspection: "Save Inspection",
          status_healthy: "✅ Healthy",
          status_attention: "⚠️ Needs Attention",
          status_critical: "🚨 Critical",
          status_label_healthy: "Healthy",
          status_label_attention: "Attention",
          status_label_critical: "Critical",
          pop_low: "Low",
          pop_medium: "Medium",
          pop_high: "High",
          pop_very_high: "Very High",
          yes: "Yes ✅",
          no: "No ❌",
          unsure: "Unsure 🤔",
          calm: "Calm 😊",
          moderate: "Moderate 😐",
          aggressive: "Aggressive 😠",
          none: "None",
          low: "Low",
          medium: "Medium",
          high: "High",
          cut_here: "✂ cut here",
          scan_hint: "Scan to view this hive's details",
          print: "Print",
          download: "Download PNG",
          qr_hint: "Print and stick the label directly on the physical hive.",
          no_location: "No location",
          no_location_set: "No location set",
          unknown: "Unknown",
          queen_suffix: "queen",
          yr_old: "yr old",
          never: "Never",
          no_inspections: "No inspections recorded yet.",
          no_activity: "No activity yet. Add a hive!",
          no_hives: "No hives yet",
          no_search: "No hives match your search",
          no_search_sub: "Try a different search term",
          no_hives_sub: "Click '+ Add Hive' to get started",
          delete_confirm: "Delete this hive? This cannot be undone.",
          hive_added: "Hive added!",
          hive_updated: "Hive updated!",
          hive_deleted: "Hive deleted",
          inspection_saved: "Inspection saved!",
          name_required: "Please enter a hive name",
          log_created: "Hive created",
          log_updated: "Hive updated",
          log_inspection: "Inspection recorded",
          id_label: "ID",
          created_label: "Created",
          last_inspected: "Last Inspected",
          notes_label: "Notes",
          stat_frames: "🖼️ Total Frames",
          stat_honey_frames: "🍯 Honey Frames",
          stat_brood_frames: "🐣 Brood Frames",
          stat_total_honey: "🍯 Total Honey",
          stat_population: "🐝 Population",
          stat_status: "💚 Status",
        },
      };

      let lang = localStorage.getItem("apiagenda-lang") || "ro";
      let hives = JSON.parse(
        localStorage.getItem("apiagenda-hives") || "[]"
      );
      let currentHiveId = null;
      let editingHiveId = null;

      function t(key) {
        return T[lang][key] || T["ro"][key] || key;
      }

      function setLang(l) {
        lang = l;
        localStorage.setItem("apiagenda-lang", l);
        document.querySelectorAll(".lang-btn").forEach((b) => {
          b.classList.toggle("active", b.textContent.trim().toLowerCase() === l);
        });
        applyTranslations();
        // re-render current view
        const active = document.querySelector(".view.active");
        if (active.id === "view-dashboard") renderDashboard();
        else if (active.id === "view-hives") renderHives();
        else if (active.id === "view-detail" && currentHiveId)
          openDetail(currentHiveId);
      }

      function applyTranslations() {
        document.querySelectorAll("[data-t]").forEach((el) => {
          el.textContent = t(el.dataset.t);
        });
        document.querySelectorAll("[data-placeholder]").forEach((el) => {
          el.placeholder = t(el.dataset.placeholder);
        });
      }

      // ── UTILS ──────────────────────────────────────────────────────────
      function save() {
        localStorage.setItem("apiagenda-hives", JSON.stringify(hives));
      }

      function uid() {
        return (
          "HIVE-" + Math.random().toString(36).substr(2, 6).toUpperCase()
        );
      }

      function fmt(date) {
        return new Date(date).toLocaleDateString(
          lang === "ro" ? "ro-RO" : "en-GB",
          { day: "2-digit", month: "short", year: "numeric" }
        );
      }

      function showToast(msg, type = "") {
        const el = document.getElementById("toast");
        el.textContent = msg;
        el.className = "toast " + type + " show";
        setTimeout(() => (el.className = "toast " + type), 2800);
      }

      function statusLabel(s) {
        return t(
          s === "healthy"
            ? "status_label_healthy"
            : s === "attention"
            ? "status_label_attention"
            : "status_label_critical"
        );
      }

      // ── VIEWS ──────────────────────────────────────────────────────────
      function showView(name) {
        document
          .querySelectorAll(".view")
          .forEach((v) => v.classList.remove("active"));
        document.getElementById("view-" + name).classList.add("active");
        document
          .querySelectorAll("header nav button")
          .forEach((b) => b.classList.remove("active"));
        if (name === "dashboard") {
          document
            .querySelectorAll("header nav button")[0]
            .classList.add("active");
          renderDashboard();
        } else if (name === "hives") {
          document
            .querySelectorAll("header nav button")[1]
            .classList.add("active");
          renderHives();
        }
      }

      // ── DASHBOARD ──────────────────────────────────────────────────────
      function renderDashboard() {
        document.getElementById("stat-total").textContent = hives.length;
        document.getElementById("stat-healthy").textContent = hives.filter(
          (h) => h.status === "healthy"
        ).length;
        document.getElementById("stat-attention").textContent = hives.filter(
          (h) => h.status !== "healthy"
        ).length;
        const totalHoney = hives.reduce(
          (s, h) => s + (parseFloat(h.honey) || 0),
          0
        );
        document.getElementById("stat-honey").textContent =
          totalHoney.toFixed(1) + " kg";

        const allLogs = [];
        hives.forEach((h) => {
          (h.log || []).forEach((l) =>
            allLogs.push({ ...l, hiveName: h.name })
          );
        });
        allLogs.sort((a, b) => new Date(b.date) - new Date(a.date));
        const recent = allLogs.slice(0, 6);
        const el = document.getElementById("recent-log");
        if (!recent.length) {
          el.innerHTML = `<div class="empty-state" style="padding:2rem">
            <div class="empty-icon">📋</div>
            <p>${t("no_activity")}</p>
          </div>`;
        } else {
          el.innerHTML = recent
            .map(
              (l) => `
            <div class="log-entry">
              <div class="log-icon">${l.icon || "📋"}</div>
              <div class="log-content">
                <div class="log-action">${l.hiveName} — ${l.action}</div>
                <div class="log-note">${l.note || ""}</div>
              </div>
              <div class="log-date">${fmt(l.date)}</div>
            </div>`
            )
            .join("");
        }
        applyTranslations();
      }

      // ── HIVES ──────────────────────────────────────────────────────────
      function renderHives() {
        const q = (
          document.getElementById("search-input").value || ""
        ).toLowerCase();
        const filtered = hives.filter(
          (h) =>
            h.name.toLowerCase().includes(q) ||
            (h.location || "").toLowerCase().includes(q)
        );
        const grid = document.getElementById("hives-grid");
        if (!filtered.length) {
          grid.innerHTML = `<div class="empty-state" style="grid-column:1/-1">
            <div class="empty-icon">🐝</div>
            <h3>${hives.length ? t("no_search") : t("no_hives")}</h3>
            <p>${hives.length ? t("no_search_sub") : t("no_hives_sub")}</p>
          </div>`;
          return;
        }
        grid.innerHTML = filtered
          .map(
            (h) => `
          <div class="hive-card" onclick="openDetail('${h.id}')">
            <div class="hive-card-header">
              <div>
                <div class="hive-name">🏠 ${h.name}</div>
                <div class="hive-id">${h.id}</div>
              </div>
              <span class="hive-status status-${h.status}">${statusLabel(h.status)}</span>
            </div>
            <div class="hive-card-body">
              <div class="hive-stats">
                <div class="hive-stat">
                  <div class="hive-stat-value">${h.frames || 0}</div>
                  <div class="hive-stat-label">🖼️ ${t("f_frames")}</div>
                </div>
                <div class="hive-stat">
                  <div class="hive-stat-value">${h.honey || 0} kg</div>
                  <div class="hive-stat-label">🍯 ${lang === "ro" ? "Miere" : "Honey"}</div>
                </div>
                <div class="hive-stat">
                  <div class="hive-stat-value">${h.honeyFrames || 0}</div>
                  <div class="hive-stat-label">🍯 ${lang === "ro" ? "R. Miere" : "Honey Fr."}</div>
                </div>
                <div class="hive-stat">
                  <div class="hive-stat-value">${h.broodFrames || 0}</div>
                  <div class="hive-stat-label">🐣 ${lang === "ro" ? "R. Puiet" : "Brood Fr."}</div>
                </div>
              </div>
            </div>
            <div class="hive-card-footer">
              <span class="hive-date">📍 ${h.location || t("no_location")}</span>
              <div class="hive-actions" onclick="event.stopPropagation()">
                <button class="icon-btn" title="${t("edit")}" onclick="openEditModal('${h.id}')">✏️</button>
                <button class="icon-btn" title="${t("qr_code")}" onclick="openQRForHive('${h.id}')">📱</button>
                <button class="icon-btn" title="Delete" onclick="deleteHive('${h.id}')">🗑️</button>
              </div>
            </div>
          </div>`
          )
          .join("");
      }

      // ── ADD / EDIT ──────────────────────────────────────────────────────
      function openAddModal() {
        editingHiveId =qnull;
        document.getElementById("hive-modal-title").dataset.t = "add_hive_title";
        document.getElementById("hive-modal-title").textContent = t("add_hive_title");
        ["name","location","queen-age","queen-type","frames","honey-frames","brood-frames","honey","notes"].forEach(id => {
          const el = document.getElementById("f-" + id);
          if (el) el.value = "";
        });
        document.getElementById("f-status").value = "healthy";
        document.getElementById("f-population").value = "Medie";
        document.getElementById("hive-modal").classList.add("open");
      }

      function openEditModal(id) {
        const h = hives.find(x => x.id === id);
        if (!h) return;
        editingHiveId = id;
        document.getElementById("hive-modal-title").dataset.t = "edit_hive_title";
        document.getElementById("hive-modal-title").textContent = t("edit_hive_title");
        document.getElementById("f-name").value = h.name || "";
        document.getElementById("f-location").value = h.location || "";
        document.getElementById("f-status").value = h.status || "healthy";
        document.getElementById("f-queen-age").value = h.queenAge || "";
        document.getElementById("f-queen-type").value = h.queenType || "";
        document.getElementById("f-frames").value = h.frames || "";
        document.getElementById("f-honey-frames").value = h.honeyFrames || "";
        document.getElementById("f-brood-frames").value = h.broodFrames || "";
        document.getElementById("f-honey").value = h.honey || "";
        document.getElementById("f-population").value = h.population || "Medie";
        document.getElementById("f-notes").value = h.notes || "";
        document.getElementById("hive-modal").classList.add("open");
      }

      function closeHiveModal() {
        document.getElementById("hive-modal").classList.remove("open");
        editingHiveId = null;
      }

      function saveHive() {
        const name = document.getElementById("f-name").value.trim();
        if (!name) { showToast(t("name_required"), "error"); return; }

        const data = {
          name,
          location: document.getElementById("f-location").value.trim(),
          status: document.getElementById("f-status").value,
          queenAge: document.getElementById("f-queen-age").value,
          queenType: document.getElementById("f-queen-type").value.trim(),
          frames: document.getElementById("f-frames").value,
          honeyFrames: document.getElementById("f-honey-frames").value,
          broodFrames: document.getElementById("f-brood-frames").value,
          honey: document.getElementById("f-honey").value,
          population: document.getElementById("f-population").value,
          notes: document.getElementById("f-notes").value.trim(),
        };

        if (editingHiveId) {
          const idx = hives.findIndex(x => x.id === editingHiveId);
          hives[idx] = { ...hives[idx], ...data };
          hives[idx].log = hives[idx].log || [];
          hives[idx].log.push({
            date: new Date().toISOString(),
            action: t("log_updated"),
            note: "",
            icon: "✏️"
          });
          showToast(t("hive_updated"), "success");
        } else {
          const newHive = {
            id: uid(),
            createdAt: new Date().toISOString(),
            log: [{
              date: new Date().toISOString(),
              action: t("log_created"),
              note: "",
              icon: "🏠"
            }],
            ...data
          };
          hives.unshift(newHive);
          showToast(t("hive_added"), "success");
        }

        save();
        closeHiveModal();
        renderHives();
        renderDashboard();
      }

      // ── DELETE ─────────────────────────────────────────────────────────
      function deleteHive(id) {
        if (!confirm(t("delete_confirm"))) return;
        hives = hives.filter(h => h.id !== id);
        save();
        renderHives();
        renderDashboard();
        showToast(t("hive_deleted"));
      }

      // ── DETAIL ─────────────────────────────────────────────────────────
      function openDetail(id) {
        currentHiveId = id;
        const h = hives.find(x => x.id === id);
        if (!h) return;

        document.getElementById("detail-name").textContent = "🏠 " + h.name;
        document.getElementById("detail-location").textContent =
          "📍 " + (h.location || t("no_location_set"));
        document.getElementById("detail-queen").textContent = h.queenType
          ? `👑 ${h.queenType}${h.queenAge ? " · " + h.queenAge + " " + t("yr_old") : ""}`
          : "";

        // Stats
        document.getElementById("detail-stats").innerHTML = [
          [t("stat_frames"), h.frames || 0],
          [t("stat_honey_frames"), h.honeyFrames || 0],
          [t("stat_brood_frames"), h.broodFrames || 0],
          [t("stat_total_honey"), (h.honey || 0) + " kg"],
          [t("stat_population"), h.population || t("unknown")],
          [t("stat_status"), statusLabel(h.status)],
        ].map(([label, val]) => `
          <div class="detail-stat">
            <div class="detail-stat-value">${val}</div>
            <div class="detail-stat-label">${label}</div>
          </div>
        `).join("");

        // Info
        document.getElementById("detail-info").innerHTML = `
          <div style="display:flex;flex-direction:column;gap:0.7rem;font-size:0.88rem;">
            <div><strong>${t("id_label")}:</strong> <code style="font-size:0.8rem;background:#f5f5f5;padding:2px 6px;border-radius:4px">${h.id}</code></div>
            <div><strong>${t("created_label")}:</strong> ${fmt(h.createdAt)}</div>
            <div><strong>${t("last_inspected")}:</strong> ${
              h.log && h.log.filter(l => l.action === t("log_inspection")).length
                ? fmt(h.log.filter(l => l.action === t("log_inspection")).slice(-1)[0].date)
                : t("never")
            }</div>
            ${h.notes ? `<div><strong>${t("notes_label")}:</strong> ${h.notes}</div>` : ""}
          </div>
        `;

        // Log
        const logs = (h.log || []).slice().reverse();
        document.getElementById("detail-log").innerHTML = logs.length
          ? logs.map(l => `
            <div class="log-entry">
              <div class="log-icon">${l.icon || "📋"}</div>
              <div class="log-content">
                <div class="log-action">${l.action}</div>
                <div class="log-note">${l.note || ""}</div>
              </div>
              <div class="log-date">${fmt(l.date)}</div>
            </div>
          `).join("")
          : `<p style="color:var(--gray-dark);font-size:0.85rem">${t("no_inspections")}</p>`;

        // Mini QR
        const miniContainer = document.getElementById("detail-qr-mini");
        miniContainer.innerHTML = "";
        const miniCanvas = document.createElement("canvas");
        miniContainer.appendChild(miniCanvas);
        QRCode.toCanvas(miniCanvas, `apiagenda://hive/${h.id}`, {
          width: 120, margin: 1,
          color: { dark: "#3e2a0f", light: "#ffffff" }
        });

        showView("detail");
        document.querySelector("#view-detail").classList.add("active");
        document.querySelectorAll(".view").forEach(v => {
          if (v.id !== "view-detail") v.classList.remove("active");
        });
        document.querySelectorAll("header nav button").forEach(b => b.classList.remove("active"));
        applyTranslations();
      }

      // ── INSPECTION ─────────────────────────────────────────────────────
      function openInspectionModal() {
        ["i-frames","i-honey-frames","i-brood-frames","i-honey","i-notes"].forEach(id => {
          document.getElementById(id).value = "";
        });
        document.getElementById("i-queen").value = "Da";
        document.getElementById("i-temper").value = "Calm";
        document.getElementById("i-status").value = "healthy";
        document.getElementById("i-varroa").value = "Absent";
        document.getElementById("inspection-modal").classList.add("open");
      }

      function closeInspectionModal() {
        document.getElementById("inspection-modal").classList.remove("open");
      }

      function saveInspection() {
        const h = hives.find(x => x.id === currentHiveId);
        if (!h) return;

        const frames = document.getElementById("i-frames").value;
        const honeyFrames = document.getElementById("i-honey-frames").value;
        const broodFrames = document.getElementById("i-brood-frames").value;
        const honey = document.getElementById("i-honey").value;
        const queen = document.getElementById("i-queen").value;
        const temper = document.getElementById("i-temper").value;
        const status = document.getElementById("i-status").value;
        const varroa = document.getElementById("i-varroa").value;
        const notes = document.getElementById("i-notes").value.trim();

        if (frames) h.frames = frames;
        if (honeyFrames) h.honeyFrames = honeyFrames;
        if (broodFrames) h.broodFrames = broodFrames;
        if (honey) h.honey = (parseFloat(h.honey || 0) + parseFloat(honey)).toFixed(1);
        h.status = status;

        const noteParts = [];
        noteParts.push(`👑 ${queen}`);
        noteParts.push(`😊 ${temper}`);
        noteParts.push(`🔬 Varroa: ${varroa}`);
        if (notes) noteParts.push(notes);

        h.log = h.log || [];
        h.log.push({
          date: new Date().toISOString(),
          action: t("log_inspection"),
          note: noteParts.join(" · "),
          icon: "🔍"
        });

        save();
        closeInspectionModal();
        openDetail(currentHiveId);
        showToast(t("inspection_saved"), "success");
      }

      // ── QR ─────────────────────────────────────────────────────────────
      function openQRModal() {
        const h = hives.find(x => x.id === currentHiveId);
        if (!h) return;
        renderQRModal(h);
        document.getElementById("qr-modal").classList.add("open");
      }

      function openQRForHive(id) {
        currentHiveId = id;
        const h = hives.find(x => x.id === id);
        if (!h) return;
        renderQRModal(h);
        document.getElementById("qr-modal").classList.add("open");
      }

      function renderQRModal(h) {
        document.getElementById("qr-hive-name-display").textContent = h.name;
        document.getElementById("qr-hive-id-display").textContent = h.id;
        const canvas = document.getElementById("qr-canvas");
        QRCode.toCanvas(canvas, `apiagenda://hive/${h.id}`, {
          width: 180, margin: 2,
          color: { dark: "#3e2a0f", light: "#ffffff" }
        });
        applyTranslations();
      }

      function closeQRModal() {
        document.getElementById("qr-modal").classList.remove("open");
      }

      function printQR() {
        const h = hives.find(x => x.id === currentHiveId);
        if (!h) return;
        const printCanvas = document.createElement("canvas");
        QRCode.toCanvas(printCanvas, `apiagenda://hive/${h.id}`, {
          width: 200, margin: 2,
          color: { dark: "#000000", light: "#ffffff" }
        }, () => {
          const inner = document.getElementById("print-label-inner");
          inner.innerHTML = `
            <div class="p-logo">🍯 ApiAgenda</div>
            <img src="${printCanvas.toDataURL()}" width="200" height="200" style="border-radius:8px"/>
            <div class="p-name">${h.name}</div>
            <div class="p-id">${h.id}</div>
            <div class="p-hint">${t("scan_hint")}</div>
          `;
          document.getElementById("print-area").style.display = "flex";
          window.print();
          document.getElementById("print-area").style.display = "none";
        });
      }

      function downloadQR() {
        const h = hives.find(x => x.id === currentHiveId);
        if (!h) return;
        const dlCanvas = document.createElement("canvas");
        const size = 400;
        QRCode.toCanvas(dlCanvas, `apiagenda://hive/${h.id}`, {
          width: size, margin: 2,
          color: { dark: "#3e2a0f", light: "#ffffff" }
        }, () => {
          // Draw a nicer labeled image
          const out = document.createElement("canvas");
          out.width = size;
          out.height = size + 80;
          const ctx = out.getContext("2d");
          ctx.fillStyle = "#ffffff";
          ctx.fillRect(0, 0, out.width, out.height);
          ctx.drawImage(dlCanvas, 0, 0);
          ctx.fillStyle = "#3e2a0f";
          ctx.font = "bold 20px Segoe UI, sans-serif";
          ctx.textAlign = "center";
          ctx.fillText(h.name, size / 2, size + 28);
          ctx.font = "13px monospace";
          ctx.fillStyle = "#888";
          ctx.fillText(h.id, size / 2, size + 50);
          ctx.font = "11px Segoe UI, sans-serif";
          ctx.fillText("🍯 ApiAgenda", size / 2, size + 70);

          const a = document.createElement("a");
          a.download = `qr-${h.id}.png`;
          a.href = out.toDataURL("image/png");
          a.click();
        });
      }

      // ── INIT ───────────────────────────────────────────────────────────
      applyTranslations();
      renderDashboard();
      renderHives();
    </script>
  </body>
</html>
