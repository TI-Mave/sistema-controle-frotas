<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Controle de Frota</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons+Outlined" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }

        :root {
            --primary: #E65100;
            --primary-light: #FF6D00;
            --primary-lighter: #FF9100;
            --primary-lightest: #FFF3E0;
            --primary-dark: #BF360C;
            --accent: #FF6D00;
            --bg: #FFF8F0;
            --bg-card: #FFFFFF;
            --text: #3E2723;
            --text-light: #795548;
            --text-muted: #A1887F;
            --border: #FFCCBC;
            --border-light: #FFE0B2;
            --success: #2E7D32;
            --danger: #C62828;
            --shadow: 0 2px 12px rgba(230, 81, 0, 0.08);
            --shadow-hover: 0 4px 20px rgba(230, 81, 0, 0.15);
            --radius: 12px;
            --radius-sm: 8px;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg);
            color: var(--text);
            min-height: 100vh;
        }

        /* ===== HEADER ===== */
        header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            color: white;
            padding: 0;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 4px 20px rgba(230, 81, 0, 0.3);
        }

        .header-top {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 16px 24px;
        }

        .header-top h1 {
            font-size: 1.5rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .header-top h1 .material-icons-outlined {
            font-size: 28px;
        }

        .header-stats {
            display: flex;
            gap: 20px;
            font-size: 0.85rem;
            opacity: 0.9;
        }

        .header-stats span {
            display: flex;
            align-items: center;
            gap: 4px;
        }

        /* ===== NAVIGATION ===== */
        nav {
            display: flex;
            padding: 0 24px;
            gap: 4px;
        }

        nav button {
            background: transparent;
            border: none;
            color: rgba(255,255,255,0.7);
            padding: 12px 20px;
            font-family: inherit;
            font-size: 0.9rem;
            font-weight: 500;
            cursor: pointer;
            border-bottom: 3px solid transparent;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        nav button:hover {
            color: white;
            background: rgba(255,255,255,0.1);
        }

        nav button.active {
            color: white;
            border-bottom-color: white;
        }

        /* ===== MAIN CONTENT ===== */
        main {
            max-width: 1200px;
            margin: 0 auto;
            padding: 24px;
        }

        .page { display: none; }
        .page.active { display: block; }

        /* ===== CARDS ===== */
        .card {
            background: var(--bg-card);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            border: 1px solid var(--border-light);
            padding: 24px;
            margin-bottom: 20px;
            transition: box-shadow 0.2s;
        }

        .card:hover {
            box-shadow: var(--shadow-hover);
        }

        .card-title {
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--primary);
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* ===== FORMS ===== */
        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 16px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        .form-group.full-width {
            grid-column: 1 / -1;
        }

        .form-group label {
            font-size: 0.8rem;
            font-weight: 600;
            color: var(--text-light);
            margin-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .form-group input,
        .form-group select,
        .form-group textarea {
            padding: 10px 14px;
            border: 2px solid var(--border-light);
            border-radius: var(--radius-sm);
            font-family: inherit;
            font-size: 0.95rem;
            color: var(--text);
            transition: border-color 0.2s, box-shadow 0.2s;
            background: white;
        }

        .form-group input:focus,
        .form-group select:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: var(--primary-lighter);
            box-shadow: 0 0 0 3px rgba(255, 145, 0, 0.15);
        }

        .form-group textarea {
            resize: vertical;
            min-height: 70px;
        }

        .form-group input::placeholder,
        .form-group textarea::placeholder {
            color: var(--text-muted);
        }

        /* ===== BUTTONS ===== */
        .btn {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            padding: 10px 20px;
            border: none;
            border-radius: var(--radius-sm);
            font-family: inherit;
            font-size: 0.9rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            color: white;
        }

        .btn-primary:hover {
            box-shadow: 0 4px 15px rgba(230, 81, 0, 0.4);
            transform: translateY(-1px);
        }

        .btn-secondary {
            background: var(--primary-lightest);
            color: var(--primary);
        }

        .btn-secondary:hover {
            background: var(--border);
        }

        .btn-danger {
            background: #FFEBEE;
            color: var(--danger);
        }

        .btn-danger:hover {
            background: #FFCDD2;
        }

        .btn-sm {
            padding: 6px 12px;
            font-size: 0.8rem;
        }

        .btn-icon {
            width: 36px;
            height: 36px;
            padding: 0;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
        }

        .form-actions {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            justify-content: flex-end;
        }

        /* ===== TABLES ===== */
        .table-container {
            overflow-x: auto;
            margin-top: 16px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        thead th {
            background: var(--primary-lightest);
            color: var(--primary);
            font-size: 0.8rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            padding: 12px 14px;
            text-align: left;
            border-bottom: 2px solid var(--border);
            white-space: nowrap;
        }

        tbody td {
            padding: 12px 14px;
            border-bottom: 1px solid var(--border-light);
            font-size: 0.9rem;
            vertical-align: middle;
        }

        tbody tr:hover {
            background: var(--primary-lightest);
        }

        .badge {
            display: inline-block;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: 600;
        }

        .badge-fuel { background: #FFF3E0; color: #E65100; }
        .badge-oil { background: #FBE9E7; color: #BF360C; }
        .badge-maintenance { background: #EFEBE9; color: #4E342E; }
        .badge-revision { background: #FFF8E1; color: #F57F17; }
        .badge-other { background: #F3E5F5; color: #6A1B9A; }

        .actions-cell {
            display: flex;
            gap: 4px;
        }

        /* ===== EMPTY STATE ===== */
        .empty-state {
            text-align: center;
            padding: 48px 24px;
            color: var(--text-muted);
        }

        .empty-state .material-icons-outlined {
            font-size: 64px;
            color: var(--border);
            margin-bottom: 12px;
        }

        .empty-state p {
            font-size: 1rem;
            margin-bottom: 4px;
        }

        .empty-state small {
            font-size: 0.85rem;
        }

        /* ===== SEARCH & FILTER BAR ===== */
        .toolbar {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
            flex-wrap: wrap;
            align-items: center;
        }

        .toolbar .search-box {
            flex: 1;
            min-width: 200px;
            position: relative;
        }

        .toolbar .search-box input {
            width: 100%;
            padding: 10px 14px 10px 40px;
            border: 2px solid var(--border-light);
            border-radius: var(--radius-sm);
            font-family: inherit;
            font-size: 0.9rem;
            color: var(--text);
            background: white;
        }

        .toolbar .search-box input:focus {
            outline: none;
            border-color: var(--primary-lighter);
            box-shadow: 0 0 0 3px rgba(255, 145, 0, 0.15);
        }

        .toolbar .search-box .material-icons-outlined {
            position: absolute;
            left: 12px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-muted);
            font-size: 20px;
        }

        .toolbar select {
            padding: 10px 14px;
            border: 2px solid var(--border-light);
            border-radius: var(--radius-sm);
            font-family: inherit;
            font-size: 0.9rem;
            color: var(--text);
            background: white;
            cursor: pointer;
        }

        /* ===== DASHBOARD ===== */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 16px;
            margin-bottom: 24px;
        }

        .stat-card {
            background: var(--bg-card);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            border: 1px solid var(--border-light);
            padding: 20px;
            display: flex;
            align-items: center;
            gap: 16px;
        }

        .stat-icon {
            width: 52px;
            height: 52px;
            border-radius: var(--radius);
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
        }

        .stat-icon .material-icons-outlined { font-size: 26px; color: white; }

        .stat-icon.orange { background: linear-gradient(135deg, #E65100, #FF6D00); }
        .stat-icon.amber { background: linear-gradient(135deg, #FF8F00, #FFB300); }
        .stat-icon.deep-orange { background: linear-gradient(135deg, #BF360C, #E64A19); }
        .stat-icon.brown { background: linear-gradient(135deg, #4E342E, #795548); }

        .stat-info h3 {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--text);
        }

        .stat-info p {
            font-size: 0.8rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        /* ===== MODAL ===== */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(62, 39, 35, 0.5);
            z-index: 200;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .modal-overlay.active {
            display: flex;
        }

        .modal {
            background: white;
            border-radius: var(--radius);
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            width: 100%;
            max-width: 600px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .modal-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 20px 24px;
            border-bottom: 1px solid var(--border-light);
        }

        .modal-header h2 {
            font-size: 1.2rem;
            color: var(--primary);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .modal-body { padding: 24px; }
        .modal-footer {
            padding: 16px 24px;
            border-top: 1px solid var(--border-light);
            display: flex;
            justify-content: flex-end;
            gap: 10px;
        }

        /* ===== FILE UPLOAD ===== */
        .file-upload-area {
            border: 2px dashed var(--border);
            border-radius: var(--radius-sm);
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s;
            color: var(--text-muted);
        }

        .file-upload-area:hover {
            border-color: var(--primary-lighter);
            background: var(--primary-lightest);
            color: var(--primary);
        }

        .file-upload-area .material-icons-outlined {
            font-size: 36px;
            margin-bottom: 8px;
        }

        .file-preview {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 10px;
        }

        .file-preview-item {
            position: relative;
            width: 80px;
            height: 80px;
            border-radius: var(--radius-sm);
            overflow: hidden;
            border: 1px solid var(--border-light);
        }

        .file-preview-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .file-preview-item .remove-file {
            position: absolute;
            top: 2px;
            right: 2px;
            background: rgba(198, 40, 40, 0.8);
            color: white;
            border: none;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            font-size: 14px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* ===== DYNAMIC FIELDS ===== */
        .dynamic-fields-container {
            border: 1px solid var(--border-light);
            border-radius: var(--radius-sm);
            padding: 16px;
            margin-top: 10px;
            background: var(--primary-lightest);
        }

        .dynamic-field-row {
            display: flex;
            gap: 10px;
            margin-bottom: 8px;
            align-items: flex-end;
        }

        .dynamic-field-row .form-group { flex: 1; margin-bottom: 0; }

        /* ===== TOAST ===== */
        .toast-container {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 300;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .toast {
            padding: 12px 20px;
            border-radius: var(--radius-sm);
            color: white;
            font-size: 0.9rem;
            font-weight: 500;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            animation: slideIn 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .toast.success { background: var(--success); }
        .toast.error { background: var(--danger); }
        .toast.info { background: var(--primary); }

        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        /* ===== CONFIRM DIALOG ===== */
        .confirm-dialog {
            text-align: center;
            padding: 20px;
        }

        .confirm-dialog .material-icons-outlined {
            font-size: 48px;
            color: var(--danger);
            margin-bottom: 12px;
        }

        .confirm-dialog p {
            font-size: 1rem;
            margin-bottom: 20px;
            color: var(--text-light);
        }

        .confirm-dialog .btn-group {
            display: flex;
            gap: 10px;
            justify-content: center;
        }

        /* ===== VEHICLE DETAIL ===== */
        .vehicle-detail-header {
            display: flex;
            align-items: center;
            gap: 16px;
            margin-bottom: 24px;
        }

        .vehicle-avatar {
            width: 64px;
            height: 64px;
            border-radius: var(--radius);
            background: linear-gradient(135deg, var(--primary), var(--primary-light));
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
        }

        .vehicle-avatar .material-icons-outlined {
            font-size: 32px;
            color: white;
        }

        .vehicle-detail-info h2 {
            font-size: 1.3rem;
            font-weight: 700;
        }

        .vehicle-detail-info p {
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        /* ===== RESPONSIVE ===== */
        @media (max-width: 768px) {
            .header-top { flex-direction: column; gap: 8px; text-align: center; }
            .header-stats { justify-content: center; }
            nav { justify-content: center; overflow-x: auto; }
            nav button { padding: 10px 14px; font-size: 0.8rem; }
            main { padding: 16px; }
            .form-grid { grid-template-columns: 1fr; }
            .toolbar { flex-direction: column; }
            .dashboard-grid { grid-template-columns: 1fr 1fr; }
            .modal { max-height: 95vh; margin: 10px; }
        }

        @media (max-width: 480px) {
            .dashboard-grid { grid-template-columns: 1fr; }
            nav button span.nav-text { display: none; }
        }

        /* ===== LOADING ===== */
        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid var(--border-light);
            border-top-color: var(--primary);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }

        @keyframes spin { to { transform: rotate(360deg); } }

        .obs-text {
            font-size: 0.8rem;
            color: var(--text-muted);
            font-style: italic;
            max-width: 200px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .km-display {
            font-weight: 600;
            color: var(--primary);
        }

        .plate-display {
            font-family: monospace;
            font-weight: 700;
            background: var(--primary-lightest);
            padding: 2px 8px;
            border-radius: 4px;
            letter-spacing: 1px;
        }

        /* ===== CATEGORY REPORT ===== */
        .cat-report-period {
            font-size: 0.9rem;
            color: var(--text-light);
            padding: 10px 14px;
            background: var(--primary-lightest);
            border-radius: var(--radius-sm);
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            gap: 8px;
            flex-wrap: wrap;
        }

        .cat-report-bar-wrap {
            display: flex;
            align-items: center;
            gap: 8px;
            min-width: 140px;
        }

        .cat-report-bar-value {
            min-width: 52px;
            font-weight: 700;
            color: var(--primary);
        }

        .cat-report-bar-container {
            flex: 1;
            height: 8px;
            background: var(--border-light);
            border-radius: 4px;
            overflow: hidden;
        }

        .cat-report-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--primary) 0%, var(--primary-light) 100%);
            border-radius: 4px;
            transition: width 0.4s ease;
        }

        /* ===== PRINT ===== */
        @media print {
            body.printing-category {
                background: white;
            }

            body.printing-category header,
            body.printing-category nav,
            body.printing-category .toolbar,
            body.printing-category .toast-container,
            body.printing-category .modal-overlay,
            body.printing-category .no-print,
            body.printing-category .page:not(#page-reports),
            body.printing-category #page-reports > .card:not(.print-target) {
                display: none !important;
            }

            body.printing-category main {
                max-width: 100%;
                padding: 0;
            }

            body.printing-category .card.print-target {
                box-shadow: none;
                border: none;
                padding: 0;
                margin: 0;
            }

            body.printing-category .card.print-target .form-grid {
                display: none;
            }

            body.printing-category table {
                font-size: 0.85rem;
            }

            body.printing-category thead th {
                background: #FFE0B2 !important;
                color: #BF360C !important;
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }

            body.printing-category .badge,
            body.printing-category .cat-report-period,
            body.printing-category .cat-report-bar,
            body.printing-category .cat-report-bar-container {
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }

            @page {
                margin: 1.5cm;
            }
        }
    </style>
</head>
<body>

    <!-- HEADER -->
    <header>
        <div class="header-top">
            <h1>
                <span class="material-icons-outlined">local_shipping</span>
                Controle de Frota
            </h1>
            <div class="header-stats">
                <span><span class="material-icons-outlined" style="font-size:16px">directions_car</span> <strong id="totalVehicles">0</strong> veículos</span>
                <span><span class="material-icons-outlined" style="font-size:16px">receipt_long</span> <strong id="totalExpenses">0</strong> gastos</span>
            </div>
        </div>
        <nav>
            <button class="active" onclick="showPage('dashboard')">
                <span class="material-icons-outlined" style="font-size:18px">dashboard</span>
                <span class="nav-text">Painel</span>
            </button>
            <button onclick="showPage('vehicles')">
                <span class="material-icons-outlined" style="font-size:18px">directions_car</span>
                <span class="nav-text">Veículos</span>
            </button>
            <button onclick="showPage('expenses')">
                <span class="material-icons-outlined" style="font-size:18px">payments</span>
                <span class="nav-text">Gastos</span>
            </button>
            <button onclick="showPage('reports')">
                <span class="material-icons-outlined" style="font-size:18px">bar_chart</span>
                <span class="nav-text">Relatórios</span>
            </button>
        </nav>
    </header>

    <!-- TOAST CONTAINER -->
    <div class="toast-container" id="toastContainer"></div>

    <!-- MAIN CONTENT -->
    <main>

        <!-- ============ DASHBOARD ============ -->
        <div class="page active" id="page-dashboard">
            <div class="dashboard-grid">
                <div class="stat-card">
                    <div class="stat-icon orange">
                        <span class="material-icons-outlined">directions_car</span>
                    </div>
                    <div class="stat-info">
                        <h3 id="statVehicles">0</h3>
                        <p>Veículos Cadastrados</p>
                    </div>
                </div>
                <div class="stat-card">
                    <div class="stat-icon amber">
                        <span class="material-icons-outlined">local_gas_station</span>
                    </div>
                    <div class="stat-info">
                        <h3 id="statFuel">R$ 0,00</h3>
                        <p>Total Abastecimentos</p>
                    </div>
                </div>
                <div class="stat-card">
                    <div class="stat-icon deep-orange">
                        <span class="material-icons-outlined">build</span>
                    </div>
                    <div class="stat-info">
                        <h3 id="statMaintenance">R$ 0,00</h3>
                        <p>Total Manutenções</p>
                    </div>
                </div>
                <div class="stat-card">
                    <div class="stat-icon brown">
                        <span class="material-icons-outlined">account_balance_wallet</span>
                    </div>
                    <div class="stat-info">
                        <h3 id="statTotal">R$ 0,00</h3>
                        <p>Gasto Total</p>
                    </div>
                </div>
            </div>

            <div class="card">
                <div class="card-title">
                    <span class="material-icons-outlined">history</span>
                    Últimos Gastos Registrados
                </div>
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>Data</th>
                                <th>Veículo</th>
                                <th>Tipo</th>
                                <th>Valor</th>
                                <th>Observação</th>
                            </tr>
                        </thead>
                        <tbody id="recentExpensesBody"></tbody>
                    </table>
                    <div class="empty-state" id="emptyDashboard">
                        <span class="material-icons-outlined">inbox</span>
                        <p>Nenhum gasto registrado ainda</p>
                        <small>Comece cadastrando veículos e registrando gastos</small>
                    </div>
                </div>
            </div>
        </div>

        <!-- ============ VEHICLES ============ -->
        <div class="page" id="page-vehicles">
            <div class="card">
                <div class="card-title">
                    <span class="material-icons-outlined">add_circle</span>
                    <span id="vehicleFormTitle">Cadastrar Veículo</span>
                </div>
                <form id="vehicleForm" onsubmit="saveVehicle(event)">
                    <input type="hidden" id="vehicleId">
                    <div class="form-grid">
                        <div class="form-group">
                            <label>Nome / Modelo *</label>
                            <input type="text" id="vehicleName" placeholder="Ex: Fiat Strada" required>
                        </div>
                        <div class="form-group">
                            <label>Placa *</label>
                            <input type="text" id="vehiclePlate" placeholder="Ex: ABC-1D23" required maxlength="8">
                        </div>
                        <div class="form-group">
                            <label>Ano</label>
                            <input type="number" id="vehicleYear" placeholder="Ex: 2023" min="1950" max="2030">
                        </div>
                        <div class="form-group">
                            <label>KM Inicial</label>
                            <input type="number" id="vehicleKm" placeholder="Ex: 45000" min="0" step="1">
                        </div>
                        <div class="form-group full-width">
                            <label>Observação</label>
                            <textarea id="vehicleObs" placeholder="Informações adicionais sobre o veículo..."></textarea>
                        </div>
                    </div>
                    <div class="form-actions">
                        <button type="button" class="btn btn-secondary" onclick="resetVehicleForm()">
                            <span class="material-icons-outlined" style="font-size:18px">close</span> Cancelar
                        </button>
                        <button type="submit" class="btn btn-primary">
                            <span class="material-icons-outlined" style="font-size:18px">save</span> Salvar Veículo
                        </button>
                    </div>
                </form>
            </div>

            <div class="toolbar">
                <div class="search-box">
                    <span class="material-icons-outlined">search</span>
                    <input type="text" placeholder="Buscar veículo por nome ou placa..." id="searchVehicle" oninput="renderVehicles()">
                </div>
            </div>

            <div class="card" style="padding: 0; overflow: hidden;">
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>Veículo</th>
                                <th>Placa</th>
                                <th>KM Atual</th>
                                <th>KM Rodados</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="vehiclesBody"></tbody>
                    </table>
                    <div class="empty-state" id="emptyVehicles">
                        <span class="material-icons-outlined">directions_car</span>
                        <p>Nenhum veículo cadastrado</p>
                        <small>Use o formulário acima para cadastrar</small>
                    </div>
                </div>
            </div>
        </div>

        <!-- ============ EXPENSES ============ -->
        <div class="page" id="page-expenses">
            <div class="card">
                <div class="card-title">
                    <span class="material-icons-outlined">add_circle</span>
                    <span id="expenseFormTitle">Registrar Gasto</span>
                </div>
                <form id="expenseForm" onsubmit="saveExpense(event)">
                    <input type="hidden" id="expenseId">
                    <div class="form-grid">
                        <div class="form-group">
                            <label>Veículo *</label>
                            <select id="expenseVehicle" required>
                                <option value="">Selecione o veículo</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>Tipo de Gasto *</label>
                            <select id="expenseType" required onchange="onExpenseTypeChange()">
                                <option value="">Selecione o tipo</option>
                                <option value="abastecimento">Abastecimento</option>
                                <option value="oleo">Óleo</option>
                                <option value="manutencao">Manutenção</option>
                                <option value="revisao">Revisão</option>
                                <option value="abastecimento_oleo">Abastecimento + Óleo</option>
                                <option value="balde_oleo">Balde de Óleo</option>
                                <option value="troca_oleo_filtro">Troca de Óleo e Filtro</option>
                                <option value="outro">Outro (personalizado)</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>Data *</label>
                            <input type="date" id="expenseDate" required>
                        </div>
                        <div class="form-group">
                            <label>Valor (R$) *</label>
                            <input type="text" id="expenseValue" placeholder="0,00" required oninput="maskMoney(this); calcLiters()">
                        </div>
                    </div>

                    <!-- Campos específicos por tipo -->
                    <div id="fuelFields" style="display:none; margin-top: 16px;">
                        <div class="form-grid">
                            <div class="form-group">
                                <label>Preço por Litro (R$)</label>
                                <input type="text" id="fuelPricePerLiter" placeholder="0,00" oninput="maskMoney(this); calcLiters()">
                            </div>
                            <div class="form-group">
                                <label>Litros <small style="font-weight:400;text-transform:none;color:var(--text-muted)">(calculado automaticamente)</small></label>
                                <input type="text" id="fuelLiters" placeholder="--" readonly style="background:#FFF3E0; font-weight:700; color:var(--primary); cursor:default;">
                            </div>
                            <div class="form-group">
                                <label>Combustível</label>
                                <select id="fuelType">
                                    <option value="">Selecione</option>
                                    <option value="gasolina">Gasolina</option>
                                    <option value="etanol">Etanol</option>
                                    <option value="diesel">Diesel</option>
                                    <option value="gnv">GNV</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="form-grid" style="margin-top: 16px;">
                        <div class="form-group full-width">
                            <label>Observação</label>
                            <textarea id="expenseObs" placeholder="Detalhes adicionais sobre este gasto..."></textarea>
                        </div>
                    </div>

                    <!-- Upload de Comprovante -->
                    <div style="margin-top: 16px;">
                        <label style="font-size: 0.8rem; font-weight: 600; color: var(--text-light); text-transform: uppercase; letter-spacing: 0.5px; display: block; margin-bottom: 6px;">Comprovante / Foto</label>
                        <div class="file-upload-area" onclick="document.getElementById('expenseFile').click()">
                            <span class="material-icons-outlined">cloud_upload</span>
                            <p>Clique para anexar comprovante</p>
                            <small>JPG, PNG ou PDF (máx. 5MB)</small>
                        </div>
                        <input type="file" id="expenseFile" accept="image/*,.pdf" style="display:none" onchange="previewFile(this)" multiple>
                        <div class="file-preview" id="filePreview"></div>
                    </div>

                    <div class="form-actions">
                        <button type="button" class="btn btn-secondary" onclick="resetExpenseForm()">
                            <span class="material-icons-outlined" style="font-size:18px">close</span> Cancelar
                        </button>
                        <button type="submit" class="btn btn-primary">
                            <span class="material-icons-outlined" style="font-size:18px">save</span> Salvar Gasto
                        </button>
                    </div>
                </form>
            </div>

            <div class="toolbar">
                <div class="search-box">
                    <span class="material-icons-outlined">search</span>
                    <input type="text" placeholder="Buscar gastos..." id="searchExpense" oninput="renderExpenses()">
                </div>
                <select id="filterVehicle" onchange="renderExpenses()">
                    <option value="">Todos os veículos</option>
                </select>
                <select id="filterType" onchange="renderExpenses()">
                    <option value="">Todos os tipos</option>
                    <option value="abastecimento">Abastecimento</option>
                    <option value="oleo">Óleo</option>
                    <option value="manutencao">Manutenção</option>
                    <option value="revisao">Revisão</option>
                    <option value="abastecimento_oleo">Abastecimento + Óleo</option>
                    <option value="balde_oleo">Balde de Óleo</option>
                    <option value="troca_oleo_filtro">Troca Óleo e Filtro</option>
                    <option value="outro">Outro</option>
                </select>
            </div>

            <div class="card" style="padding: 0; overflow: hidden;">
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>Data</th>
                                <th>Veículo</th>
                                <th>Tipo</th>
                                <th>Valor</th>
                                <th>KM</th>
                                <th>Observação</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="expensesBody"></tbody>
                    </table>
                    <div class="empty-state" id="emptyExpenses">
                        <span class="material-icons-outlined">receipt_long</span>
                        <p>Nenhum gasto registrado</p>
                        <small>Use o formulário acima para registrar</small>
                    </div>
                </div>
            </div>
        </div>

        <!-- ============ REPORTS ============ -->
        <div class="page" id="page-reports">
            <div class="card">
                <div class="card-title">
                    <span class="material-icons-outlined">bar_chart</span>
                    Relatório por Veículo
                </div>
                <div class="form-grid" style="margin-bottom: 20px;">
                    <div class="form-group">
                        <label>Veículo</label>
                        <select id="reportVehicle" onchange="generateReport()">
                            <option value="">Todos os veículos</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Período - Início</label>
                        <input type="date" id="reportDateStart" onchange="generateReport()">
                    </div>
                    <div class="form-group">
                        <label>Período - Fim</label>
                        <input type="date" id="reportDateEnd" onchange="generateReport()">
                    </div>
                </div>

                <div id="reportContent">
                    <div class="dashboard-grid" id="reportStats"></div>
                    <div class="table-container">
                        <table>
                            <thead>
                                <tr>
                                    <th>Tipo</th>
                                    <th>Quantidade</th>
                                    <th>Total (R$)</th>
                                </tr>
                            </thead>
                            <tbody id="reportBody"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Relatório de Gastos por Categoria -->
            <div class="card print-target">
                <div class="card-title">
                    <span class="material-icons-outlined">pie_chart</span>
                    Relatório de Gastos por Categoria
                </div>
                <div class="form-grid" style="margin-bottom: 16px;">
                    <div class="form-group">
                        <label>Data Inicial *</label>
                        <input type="date" id="catReportStart">
                    </div>
                    <div class="form-group">
                        <label>Data Final *</label>
                        <input type="date" id="catReportEnd">
                    </div>
                    <div class="form-group">
                        <label>Tipo de Gasto</label>
                        <select id="catReportType">
                            <option value="">Todos os tipos</option>
                            <option value="abastecimento">Abastecimento</option>
                            <option value="oleo">Óleo</option>
                            <option value="manutencao">Manutenção</option>
                            <option value="revisao">Revisão</option>
                            <option value="abastecimento_oleo">Abastecimento + Óleo</option>
                            <option value="balde_oleo">Balde de Óleo</option>
                            <option value="troca_oleo_filtro">Troca Óleo e Filtro</option>
                            <option value="outro">Outro</option>
                        </select>
                    </div>
                </div>
                <div class="form-actions no-print" style="margin-top: 0; margin-bottom: 16px; justify-content: flex-start;">
                    <button type="button" class="btn btn-primary" onclick="renderCategoryReport()">
                        <span class="material-icons-outlined" style="font-size:18px">search</span> Gerar Relatório
                    </button>
                    <button type="button" class="btn btn-secondary" onclick="printCategoryReport()">
                        <span class="material-icons-outlined" style="font-size:18px">print</span> Imprimir / Salvar PDF
                    </button>
                </div>
                <div id="catReportContent">
                    <div class="empty-state">
                        <span class="material-icons-outlined">pie_chart</span>
                        <p>Defina o período e clique em "Gerar Relatório"</p>
                        <small>A data inicial e final são obrigatórias; o tipo é opcional</small>
                    </div>
                </div>
            </div>

            <!-- Registro de Quilometragem Mensal -->
            <div class="card">
                <div class="card-title">
                    <span class="material-icons-outlined">edit_note</span>
                    Registro de Quilometragem Mensal
                </div>
                <p style="font-size: 0.85rem; color: var(--text-light); margin-bottom: 16px;">
                    Registre a quilometragem de cada veículo no último dia de cada mês para acompanhar o deslocamento mensal.
                </p>
                <div class="form-grid" style="margin-bottom: 16px;">
                    <div class="form-group">
                        <label>Veículo</label>
                        <select id="kmReadingVehicle">
                            <option value="">Selecione o veículo</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Mês/Ano</label>
                        <input type="month" id="kmReadingMonth">
                    </div>
                    <div class="form-group">
                        <label>Quilometragem Atual (km)</label>
                        <input type="number" id="kmReadingValue" placeholder="Ex: 45230" min="0">
                    </div>
                </div>
                <div class="form-actions" style="margin-top: 0; margin-bottom: 20px;">
                    <button class="btn btn-primary" onclick="saveKmReading()">
                        <span class="material-icons-outlined" style="font-size:18px">save</span>
                        Salvar Leitura
                    </button>
                </div>
                <div id="kmReadingsTableContainer"></div>
            </div>

            <!-- Relatório de Quilometragem -->
            <div class="card">
                <div class="card-title">
                    <span class="material-icons-outlined">route</span>
                    Relatório de Quilometragem
                </div>
                <div class="form-grid" style="margin-bottom: 20px;">
                    <div class="form-group">
                        <label>Veículo</label>
                        <select id="kmReportVehicle" onchange="renderMileageReport()">
                            <option value="">Todos os veículos</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Período - Início (Mês/Ano)</label>
                        <input type="month" id="kmReportStart" onchange="renderMileageReport()">
                    </div>
                    <div class="form-group">
                        <label>Período - Fim (Mês/Ano)</label>
                        <input type="month" id="kmReportEnd" onchange="renderMileageReport()">
                    </div>
                </div>
                <div id="kmReportContent"></div>
            </div>
        </div>

    </main>

    <!-- ============ MODAL CONFIRM ============ -->
    <div class="modal-overlay" id="confirmModal">
        <div class="modal" style="max-width: 400px;">
            <div class="modal-body">
                <div class="confirm-dialog">
                    <span class="material-icons-outlined">warning</span>
                    <p id="confirmMessage">Tem certeza que deseja excluir?</p>
                    <div class="btn-group">
                        <button class="btn btn-secondary" onclick="closeConfirm()">Cancelar</button>
                        <button class="btn btn-danger" id="confirmBtn" onclick="confirmAction()">Excluir</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- ============ MODAL DETAIL ============ -->
    <div class="modal-overlay" id="detailModal">
        <div class="modal">
            <div class="modal-header">
                <h2><span class="material-icons-outlined">info</span> Detalhes do Gasto</h2>
                <button class="btn btn-icon btn-secondary" onclick="closeDetail()">
                    <span class="material-icons-outlined">close</span>
                </button>
            </div>
            <div class="modal-body" id="detailBody"></div>
        </div>
    </div>

    <!-- ============ FIREBASE SDK ============ -->
    <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-storage-compat.js"></script>

    <script>
    // ================================================================
    //  FIREBASE CONFIG - Substitua com suas credenciais
    // ================================================================
    const firebaseConfig = {
        apiKey: "AIzaSyAx8mWC07rTV39dJx_ZpBiAiLmwreu6FbE",
        authDomain: "controle-frota-mave.firebaseapp.com",
        projectId: "controle-frota-mave",
        storageBucket: "controle-frota-mave.firebasestorage.app",
        messagingSenderId: "331175200686",
        appId: "1:331175200686:web:7c16826d073866ccd82a3f"
    };

    // Detecta se o Firebase foi configurado com credenciais reais
    const firebaseConfigured = firebaseConfig.apiKey !== "SUA_API_KEY" && firebaseConfig.projectId !== "SEU_PROJETO";

    let db = null;
    let storage = null;

    if (firebaseConfigured) {
        firebase.initializeApp(firebaseConfig);
        db = firebase.firestore();
        storage = firebase.storage();
        console.log('Firebase inicializado com sucesso.');
    } else {
        console.warn('Firebase não configurado. Usando armazenamento local (localStorage).');
    }

    // ================================================================
    //  STATE
    // ================================================================
    let vehicles = [];
    let expenses = [];
    let kmReadings = [];
    let pendingFiles = [];
    let confirmCallback = null;

    // ================================================================
    //  INITIALIZATION
    // ================================================================
    document.addEventListener('DOMContentLoaded', () => {
        setDefaultDate();
        loadData();
    });

    function setDefaultDate() {
        const today = new Date().toISOString().split('T')[0];
        document.getElementById('expenseDate').value = today;
    }

    // ================================================================
    //  FIREBASE DATA OPERATIONS
    // ================================================================
    async function loadData() {
        if (firebaseConfigured) {
            try {
                const vSnap = await db.collection('vehicles').orderBy('createdAt', 'desc').get();
                vehicles = vSnap.docs.map(doc => ({ id: doc.id, ...doc.data() }));

                const eSnap = await db.collection('expenses').orderBy('date', 'desc').get();
                expenses = eSnap.docs.map(doc => ({ id: doc.id, ...doc.data() }));

                const kSnap = await db.collection('km_readings').orderBy('yearMonth', 'desc').get();
                kmReadings = kSnap.docs.map(doc => ({ id: doc.id, ...doc.data() }));
            } catch (e) {
                console.error('Erro ao carregar do Firebase:', e);
                alert('Erro ao carregar dados do Firebase: ' + e.message + '\nVerifique as regras do Firestore.');
                loadFromLocalStorage();
            }
        } else {
            loadFromLocalStorage();
        }

        renderAll();
    }

    // Fallback: LocalStorage (funciona sem Firebase configurado)
    function loadFromLocalStorage() {
        vehicles = JSON.parse(localStorage.getItem('fleet_vehicles') || '[]');
        expenses = JSON.parse(localStorage.getItem('fleet_expenses') || '[]');
        kmReadings = JSON.parse(localStorage.getItem('fleet_km_readings') || '[]');
    }

    function saveToLocalStorage() {
        localStorage.setItem('fleet_vehicles', JSON.stringify(vehicles));
        localStorage.setItem('fleet_expenses', JSON.stringify(expenses));
        localStorage.setItem('fleet_km_readings', JSON.stringify(kmReadings));
    }

    let storageAvailable = firebaseConfigured && !!storage;

    async function uploadFile(file) {
        if (!storageAvailable) return null;
        try {
            const fileName = `comprovantes/${Date.now()}_${file.name}`;
            const ref = storage.ref(fileName);
            const timeout = new Promise((_, reject) => setTimeout(() => reject(new Error('timeout')), 8000));
            await Promise.race([ref.put(file), timeout]);
            return await ref.getDownloadURL();
        } catch (e) {
            console.warn('Storage não disponível:', e.message);
            storageAvailable = false;
            return null;
        }
    }

    // ================================================================
    //  VEHICLES CRUD
    // ================================================================
    async function saveVehicle(e) {
        e.preventDefault();

        const id = document.getElementById('vehicleId').value;
        const vehicle = {
            name: document.getElementById('vehicleName').value.trim(),
            plate: document.getElementById('vehiclePlate').value.trim().toUpperCase(),
            year: document.getElementById('vehicleYear').value || '',
            initialKm: document.getElementById('vehicleKm').value || 0,
            obs: document.getElementById('vehicleObs').value.trim(),
            updatedAt: new Date().toISOString()
        };

        if (firebaseConfigured) {
            try {
                if (id) {
                    await db.collection('vehicles').doc(id).update(vehicle);
                    const idx = vehicles.findIndex(v => v.id === id);
                    if (idx >= 0) vehicles[idx] = { ...vehicles[idx], ...vehicle };
                    showToast('Veículo atualizado com sucesso!', 'success');
                } else {
                    vehicle.createdAt = new Date().toISOString();
                    const docRef = await db.collection('vehicles').add(vehicle);
                    vehicle.id = docRef.id;
                    vehicles.unshift(vehicle);
                    showToast('Veículo cadastrado com sucesso!', 'success');
                }
            } catch (err) {
                console.error('Erro Firebase:', err);
                showToast('Erro ao salvar no servidor. Tente novamente.', 'error');
                return;
            }
        } else {
            if (id) {
                const idx = vehicles.findIndex(v => v.id === id);
                if (idx >= 0) vehicles[idx] = { ...vehicles[idx], ...vehicle };
                showToast('Veículo atualizado com sucesso!', 'success');
            } else {
                vehicle.id = generateId();
                vehicle.createdAt = new Date().toISOString();
                vehicles.unshift(vehicle);
                showToast('Veículo cadastrado com sucesso!', 'success');
            }
            saveToLocalStorage();
        }

        resetVehicleForm();
        renderAll();
    }

    function editVehicle(id) {
        const v = vehicles.find(v => v.id === id);
        if (!v) return;

        document.getElementById('vehicleId').value = v.id;
        document.getElementById('vehicleName').value = v.name;
        document.getElementById('vehiclePlate').value = v.plate;
        document.getElementById('vehicleYear').value = v.year;
        document.getElementById('vehicleKm').value = v.initialKm;
        document.getElementById('vehicleObs').value = v.obs || '';
        document.getElementById('vehicleFormTitle').textContent = 'Editar Veículo';

        showPage('vehicles');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    async function deleteVehicle(id) {
        showConfirm('Tem certeza que deseja excluir este veículo? Todos os gastos associados serão mantidos.', async () => {
            if (firebaseConfigured) {
                try {
                    await db.collection('vehicles').doc(id).delete();
                } catch (err) {
                    console.warn('Erro ao excluir do Firebase:', err);
                }
            }
            vehicles = vehicles.filter(v => v.id !== id);
            saveToLocalStorage();
            renderAll();
            showToast('Veículo excluído!', 'info');
        });
    }

    function resetVehicleForm() {
        document.getElementById('vehicleForm').reset();
        document.getElementById('vehicleId').value = '';
        document.getElementById('vehicleFormTitle').textContent = 'Cadastrar Veículo';
    }

    // ================================================================
    //  EXPENSES CRUD
    // ================================================================
    async function saveExpense(e) {
        e.preventDefault();

        const id = document.getElementById('expenseId').value;
        const type = document.getElementById('expenseType').value;

        const expense = {
            vehicleId: document.getElementById('expenseVehicle').value,
            type: type,
            date: document.getElementById('expenseDate').value,
            value: parseMoney(document.getElementById('expenseValue').value),
            km: '',
            obs: document.getElementById('expenseObs').value.trim(),
            updatedAt: new Date().toISOString()
        };

        // Campos de abastecimento
        if (['abastecimento', 'abastecimento_oleo'].includes(type)) {
            expense.fuelLiters = parseFloat(document.getElementById('fuelLiters').value.replace(',', '.')) || '';
            expense.fuelPricePerLiter = parseMoney(document.getElementById('fuelPricePerLiter').value);
            expense.fuelType = document.getElementById('fuelType').value;
        }

        // Upload de arquivos
        if (pendingFiles.length > 0) {
            if (!storageAvailable) {
                showToast('Storage não configurado. Gasto será salvo sem o anexo.', 'info');
            } else {
                expense.files = [];
                for (const file of pendingFiles) {
                    const url = await uploadFile(file);
                    if (url) expense.files.push(url);
                }
                if (expense.files.length === 0) {
                    delete expense.files;
                    showToast('Não foi possível enviar o anexo. Gasto será salvo sem ele.', 'info');
                }
            }
        }

        if (firebaseConfigured) {
            try {
                if (id) {
                    await db.collection('expenses').doc(id).update(expense);
                    const idx = expenses.findIndex(ex => ex.id === id);
                    if (idx >= 0) expenses[idx] = { ...expenses[idx], ...expense };
                    showToast('Gasto atualizado com sucesso!', 'success');
                } else {
                    expense.createdAt = new Date().toISOString();
                    const docRef = await db.collection('expenses').add(expense);
                    expense.id = docRef.id;
                    expenses.unshift(expense);
                    showToast('Gasto registrado com sucesso!', 'success');
                }
            } catch (err) {
                console.error('Erro Firebase:', err);
                showToast('Erro ao salvar no servidor. Tente novamente.', 'error');
                return;
            }
        } else {
            if (id) {
                const idx = expenses.findIndex(ex => ex.id === id);
                if (idx >= 0) expenses[idx] = { ...expenses[idx], ...expense };
                showToast('Gasto atualizado com sucesso!', 'success');
            } else {
                expense.id = generateId();
                expense.createdAt = new Date().toISOString();
                expenses.unshift(expense);
                showToast('Gasto registrado com sucesso!', 'success');
            }
            saveToLocalStorage();
        }

        resetExpenseForm();
        renderAll();
    }

    function editExpense(id) {
        const ex = expenses.find(e => e.id === id);
        if (!ex) return;

        document.getElementById('expenseId').value = ex.id;
        document.getElementById('expenseVehicle').value = ex.vehicleId;
        document.getElementById('expenseType').value = ex.type;
        document.getElementById('expenseDate').value = ex.date;
        document.getElementById('expenseValue').value = formatMoney(ex.value);
        document.getElementById('expenseObs').value = ex.obs || '';

        onExpenseTypeChange();

        if (['abastecimento', 'abastecimento_oleo'].includes(ex.type)) {
            document.getElementById('fuelLiters').value = ex.fuelLiters ? parseFloat(ex.fuelLiters).toFixed(2).replace('.', ',') : '';
            document.getElementById('fuelPricePerLiter').value = ex.fuelPricePerLiter ? formatMoney(ex.fuelPricePerLiter) : '';
            document.getElementById('fuelType').value = ex.fuelType || '';
        }

        document.getElementById('expenseFormTitle').textContent = 'Editar Gasto';
        showPage('expenses');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    async function deleteExpense(id) {
        showConfirm('Tem certeza que deseja excluir este gasto?', async () => {
            if (firebaseConfigured) {
                try {
                    await db.collection('expenses').doc(id).delete();
                } catch (err) {
                    console.warn('Erro ao excluir do Firebase:', err);
                }
            }
            expenses = expenses.filter(ex => ex.id !== id);
            saveToLocalStorage();
            renderAll();
            showToast('Gasto excluído!', 'info');
        });
    }

    function resetExpenseForm() {
        document.getElementById('expenseForm').reset();
        document.getElementById('expenseId').value = '';
        document.getElementById('expenseFormTitle').textContent = 'Registrar Gasto';
        document.getElementById('fuelFields').style.display = 'none';
        document.getElementById('filePreview').innerHTML = '';
        pendingFiles = [];
        setDefaultDate();
    }

    // ================================================================
    //  EXPENSE TYPE FIELDS
    // ================================================================
    function onExpenseTypeChange() {
        const type = document.getElementById('expenseType').value;

        document.getElementById('fuelFields').style.display =
            ['abastecimento', 'abastecimento_oleo'].includes(type) ? 'block' : 'none';
    }

    // ================================================================
    //  AUTO CALC LITERS
    // ================================================================
    function calcLiters() {
        const type = document.getElementById('expenseType').value;
        if (!['abastecimento', 'abastecimento_oleo'].includes(type)) return;

        const totalValue = parseMoney(document.getElementById('expenseValue').value);
        const pricePerLiter = parseMoney(document.getElementById('fuelPricePerLiter').value);

        const litersInput = document.getElementById('fuelLiters');

        if (totalValue > 0 && pricePerLiter > 0) {
            const liters = totalValue / pricePerLiter;
            litersInput.value = liters.toFixed(2).replace('.', ',');
        } else {
            litersInput.value = '';
        }
    }

    // ================================================================
    //  FILE HANDLING
    // ================================================================
    function previewFile(input) {
        const preview = document.getElementById('filePreview');
        Array.from(input.files).forEach(file => {
            if (file.size > 5 * 1024 * 1024) {
                showToast('Arquivo muito grande (máx 5MB)', 'error');
                return;
            }
            pendingFiles.push(file);
            const div = document.createElement('div');
            div.className = 'file-preview-item';
            if (file.type.startsWith('image/')) {
                const img = document.createElement('img');
                img.src = URL.createObjectURL(file);
                div.appendChild(img);
            } else {
                div.innerHTML = '<span class="material-icons-outlined" style="font-size:36px;color:var(--primary);display:flex;align-items:center;justify-content:center;height:100%">description</span>';
            }
            const removeBtn = document.createElement('button');
            removeBtn.className = 'remove-file';
            removeBtn.innerHTML = '×';
            removeBtn.onclick = () => {
                const idx = pendingFiles.indexOf(file);
                if (idx >= 0) pendingFiles.splice(idx, 1);
                div.remove();
            };
            div.appendChild(removeBtn);
            preview.appendChild(div);
        });
        input.value = '';
    }

    // ================================================================
    //  RENDERING
    // ================================================================
    function renderAll() {
        renderVehicles();
        renderExpenses();
        renderDashboard();
        populateVehicleSelects();
        generateReport();
        renderKmReadings();
        renderMileageReport();
    }

    function renderVehicles() {
        const search = document.getElementById('searchVehicle').value.toLowerCase();
        const filtered = vehicles.filter(v =>
            v.name.toLowerCase().includes(search) ||
            v.plate.toLowerCase().includes(search)
        );

        const tbody = document.getElementById('vehiclesBody');
        const empty = document.getElementById('emptyVehicles');

        if (filtered.length === 0) {
            tbody.innerHTML = '';
            empty.style.display = 'block';
            return;
        }

        empty.style.display = 'none';
        tbody.innerHTML = filtered.map(v => {
            const initialKm = Number(v.initialKm) || 0;

            // KM atual: maior valor entre leituras mensais e KM dos gastos
            const latestReading = kmReadings
                .filter(r => r.vehicleId === v.id)
                .sort((a, b) => b.yearMonth.localeCompare(a.yearMonth))[0];
            const bestKmFromReadings = latestReading ? Number(latestReading.km) : 0;
            const bestKmFromExpenses = expenses
                .filter(e => e.vehicleId === v.id && e.km)
                .reduce((max, e) => Math.max(max, Number(e.km) || 0), 0);
            const currentKm = Math.max(bestKmFromReadings, bestKmFromExpenses);
            const kmRodados = currentKm > initialKm ? currentKm - initialKm : 0;

            return `
            <tr>
                <td><strong>${escapeHtml(v.name)}</strong></td>
                <td><span class="plate-display">${escapeHtml(v.plate)}</span></td>
                <td class="km-display">${currentKm > 0 ? currentKm.toLocaleString('pt-BR') + ' km' : '-'}</td>
                <td><strong style="color:${kmRodados > 0 ? 'var(--success)' : 'var(--text-muted)'}">${kmRodados > 0 ? kmRodados.toLocaleString('pt-BR') + ' km' : '-'}</strong></td>
                <td>
                    <div class="actions-cell">
                        <button class="btn btn-secondary btn-icon" onclick="editVehicle('${v.id}')" title="Editar">
                            <span class="material-icons-outlined" style="font-size:18px">edit</span>
                        </button>
                        <button class="btn btn-danger btn-icon" onclick="deleteVehicle('${v.id}')" title="Excluir">
                            <span class="material-icons-outlined" style="font-size:18px">delete</span>
                        </button>
                    </div>
                </td>
            </tr>
        `}).join('');
    }

    function renderExpenses() {
        const search = document.getElementById('searchExpense').value.toLowerCase();
        const filterV = document.getElementById('filterVehicle').value;
        const filterT = document.getElementById('filterType').value;

        let filtered = expenses;
        if (search) {
            filtered = filtered.filter(e => {
                const v = vehicles.find(v => v.id === e.vehicleId);
                const vName = v ? v.name.toLowerCase() : '';
                return vName.includes(search) || getTypeName(e).toLowerCase().includes(search) || (e.obs || '').toLowerCase().includes(search);
            });
        }
        if (filterV) filtered = filtered.filter(e => e.vehicleId === filterV);
        if (filterT) filtered = filtered.filter(e => e.type === filterT);

        const tbody = document.getElementById('expensesBody');
        const empty = document.getElementById('emptyExpenses');

        if (filtered.length === 0) {
            tbody.innerHTML = '';
            empty.style.display = 'block';
            return;
        }

        empty.style.display = 'none';
        tbody.innerHTML = filtered.map(e => {
            const v = vehicles.find(v => v.id === e.vehicleId);
            return `
                <tr>
                    <td>${formatDate(e.date)}</td>
                    <td>${v ? escapeHtml(v.name) : '<em>Removido</em>'}</td>
                    <td><span class="badge ${getTypeBadge(e.type)}">${getTypeName(e)}</span></td>
                    <td><strong>R$ ${formatMoney(e.value)}</strong></td>
                    <td>${e.km ? Number(e.km).toLocaleString('pt-BR') : '-'}</td>
                    <td><span class="obs-text" title="${escapeHtml(e.obs || '')}">${escapeHtml(e.obs || '-')}</span></td>
                    <td>
                        <div class="actions-cell">
                            <button class="btn btn-secondary btn-icon" onclick="showExpenseDetail('${e.id}')" title="Detalhes">
                                <span class="material-icons-outlined" style="font-size:18px">visibility</span>
                            </button>
                            <button class="btn btn-secondary btn-icon" onclick="editExpense('${e.id}')" title="Editar">
                                <span class="material-icons-outlined" style="font-size:18px">edit</span>
                            </button>
                            <button class="btn btn-danger btn-icon" onclick="deleteExpense('${e.id}')" title="Excluir">
                                <span class="material-icons-outlined" style="font-size:18px">delete</span>
                            </button>
                        </div>
                    </td>
                </tr>
            `;
        }).join('');
    }

    function renderDashboard() {
        // Stats
        const totalVehicles = vehicles.length;
        const fuelExpenses = expenses.filter(e => ['abastecimento', 'abastecimento_oleo'].includes(e.type));
        const maintenanceExpenses = expenses.filter(e => !['abastecimento'].includes(e.type));
        const totalFuel = fuelExpenses.reduce((s, e) => s + (parseFloat(e.value) || 0), 0);
        const totalMaint = expenses.filter(e => ['manutencao', 'revisao', 'troca_oleo_filtro', 'oleo', 'balde_oleo', 'outro'].includes(e.type)).reduce((s, e) => s + (parseFloat(e.value) || 0), 0);
        const totalAll = expenses.reduce((s, e) => s + (parseFloat(e.value) || 0), 0);

        document.getElementById('statVehicles').textContent = totalVehicles;
        document.getElementById('statFuel').textContent = 'R$ ' + formatMoney(totalFuel);
        document.getElementById('statMaintenance').textContent = 'R$ ' + formatMoney(totalMaint);
        document.getElementById('statTotal').textContent = 'R$ ' + formatMoney(totalAll);
        document.getElementById('totalVehicles').textContent = totalVehicles;
        document.getElementById('totalExpenses').textContent = expenses.length;

        // Recent expenses
        const recent = expenses.slice(0, 10);
        const tbody = document.getElementById('recentExpensesBody');
        const empty = document.getElementById('emptyDashboard');

        if (recent.length === 0) {
            tbody.innerHTML = '';
            empty.style.display = 'block';
            return;
        }

        empty.style.display = 'none';
        tbody.innerHTML = recent.map(e => {
            const v = vehicles.find(v => v.id === e.vehicleId);
            return `
                <tr>
                    <td>${formatDate(e.date)}</td>
                    <td>${v ? escapeHtml(v.name) : '-'}</td>
                    <td><span class="badge ${getTypeBadge(e.type)}">${getTypeName(e)}</span></td>
                    <td><strong>R$ ${formatMoney(e.value)}</strong></td>
                    <td><span class="obs-text">${escapeHtml(e.obs || '-')}</span></td>
                </tr>
            `;
        }).join('');
    }

    function populateVehicleSelects() {
        const options = vehicles.map(v => `<option value="${v.id}">${escapeHtml(v.name)} - ${escapeHtml(v.plate)}</option>`).join('');

        document.getElementById('expenseVehicle').innerHTML = '<option value="">Selecione o veículo</option>' + options;
        document.getElementById('filterVehicle').innerHTML = '<option value="">Todos os veículos</option>' + options;
        document.getElementById('reportVehicle').innerHTML = '<option value="">Todos os veículos</option>' + options;
        document.getElementById('kmReadingVehicle').innerHTML = '<option value="">Selecione o veículo</option>' + options;
        document.getElementById('kmReportVehicle').innerHTML = '<option value="">Todos os veículos</option>' + options;
    }

    // ================================================================
    //  REPORTS
    // ================================================================
    function generateReport() {
        const vehicleId = document.getElementById('reportVehicle').value;
        const dateStart = document.getElementById('reportDateStart').value;
        const dateEnd = document.getElementById('reportDateEnd').value;

        let filtered = [...expenses];
        if (vehicleId) filtered = filtered.filter(e => e.vehicleId === vehicleId);
        if (dateStart) filtered = filtered.filter(e => e.date >= dateStart);
        if (dateEnd) filtered = filtered.filter(e => e.date <= dateEnd);

        const types = {};
        filtered.forEach(e => {
            const typeName = getTypeName(e);
            if (!types[typeName]) types[typeName] = { count: 0, total: 0 };
            types[typeName].count++;
            types[typeName].total += parseFloat(e.value) || 0;
        });

        const grandTotal = filtered.reduce((s, e) => s + (parseFloat(e.value) || 0), 0);

        // Stats cards
        document.getElementById('reportStats').innerHTML = `
            <div class="stat-card">
                <div class="stat-icon orange"><span class="material-icons-outlined">receipt_long</span></div>
                <div class="stat-info">
                    <h3>${filtered.length}</h3>
                    <p>Total de Registros</p>
                </div>
            </div>
            <div class="stat-card">
                <div class="stat-icon amber"><span class="material-icons-outlined">account_balance_wallet</span></div>
                <div class="stat-info">
                    <h3>R$ ${formatMoney(grandTotal)}</h3>
                    <p>Valor Total</p>
                </div>
            </div>
        `;

        // Table
        const tbody = document.getElementById('reportBody');
        if (Object.keys(types).length === 0) {
            tbody.innerHTML = '<tr><td colspan="3" style="text-align:center;color:var(--text-muted);padding:20px;">Nenhum dado encontrado para o filtro selecionado</td></tr>';
            return;
        }

        tbody.innerHTML = Object.entries(types).map(([name, data]) => `
            <tr>
                <td><strong>${escapeHtml(name)}</strong></td>
                <td>${data.count}</td>
                <td><strong>R$ ${formatMoney(data.total)}</strong></td>
            </tr>
        `).join('') + `
            <tr style="background: var(--primary-lightest); font-weight: 700;">
                <td>TOTAL</td>
                <td>${filtered.length}</td>
                <td>R$ ${formatMoney(grandTotal)}</td>
            </tr>
        `;
    }

    // ================================================================
    //  CATEGORY REPORT (Relatório de Gastos por Categoria)
    // ================================================================
    function renderCategoryReport() {
        const container = document.getElementById('catReportContent');
        const start = document.getElementById('catReportStart').value;
        const end = document.getElementById('catReportEnd').value;
        const typeFilter = document.getElementById('catReportType').value;

        if (!start || !end) {
            showToast('Preencha a data inicial e a data final.', 'error');
            return;
        }
        if (end < start) {
            showToast('A data final não pode ser menor que a data inicial.', 'error');
            return;
        }

        const rows = expenses.filter(e =>
            e.date && e.date >= start && e.date <= end &&
            (!typeFilter || e.type === typeFilter)
        );

        const periodHtml = `
            <div class="cat-report-period">
                <span class="material-icons-outlined" style="font-size:18px">event</span>
                <span>Período analisado: <strong>${formatDate(start)} a ${formatDate(end)}</strong></span>
                ${typeFilter ? `<span>&nbsp;|&nbsp; Filtro: <strong>${escapeHtml(TYPE_NAMES[typeFilter] || typeFilter)}</strong></span>` : ''}
            </div>
        `;

        if (rows.length === 0) {
            container.innerHTML = periodHtml + `
                <div class="empty-state">
                    <span class="material-icons-outlined">search_off</span>
                    <p>Nenhum gasto encontrado no período</p>
                    <small>Tente ampliar o intervalo ou remover o filtro de tipo</small>
                </div>
            `;
            return;
        }

        const grandTotal = rows.reduce((s, e) => s + (parseFloat(e.value) || 0), 0);

        const grouped = rows.reduce((acc, e) => {
            const key = getTypeName(e);
            if (!acc[key]) acc[key] = { count: 0, total: 0, type: e.type };
            acc[key].count += 1;
            acc[key].total += parseFloat(e.value) || 0;
            return acc;
        }, {});

        const result = Object.entries(grouped).map(([name, g]) => ({
            name,
            count: g.count,
            total: g.total,
            avg: g.count > 0 ? g.total / g.count : 0,
            pct: grandTotal > 0 ? (g.total / grandTotal) * 100 : 0,
            badge: getTypeBadge(g.type)
        })).sort((a, b) => b.total - a.total);

        const bodyRows = result.map(r => `
            <tr>
                <td><span class="badge ${r.badge}">${escapeHtml(r.name)}</span></td>
                <td>${r.count}</td>
                <td><strong>R$ ${formatMoney(r.total)}</strong></td>
                <td>R$ ${formatMoney(r.avg)}</td>
                <td>
                    <div class="cat-report-bar-wrap">
                        <span class="cat-report-bar-value">${r.pct.toFixed(1).replace('.', ',')}%</span>
                        <div class="cat-report-bar-container">
                            <div class="cat-report-bar" style="width:${r.pct.toFixed(2)}%"></div>
                        </div>
                    </div>
                </td>
            </tr>
        `).join('');

        container.innerHTML = periodHtml + `
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Categoria</th>
                            <th>Qtd. Lançamentos</th>
                            <th>Total</th>
                            <th>Média / Lançamento</th>
                            <th>% do Total</th>
                        </tr>
                    </thead>
                    <tbody>${bodyRows}</tbody>
                    <tfoot>
                        <tr style="background: var(--primary-lightest); font-weight: 700;">
                            <td>TOTAL GERAL</td>
                            <td>${rows.length}</td>
                            <td>R$ ${formatMoney(grandTotal)}</td>
                            <td>R$ ${formatMoney(grandTotal / rows.length)}</td>
                            <td>100,0%</td>
                        </tr>
                    </tfoot>
                </table>
            </div>
        `;
    }

    function printCategoryReport() {
        const container = document.getElementById('catReportContent');
        if (!container.querySelector('table')) {
            showToast('Gere o relatório antes de imprimir.', 'error');
            return;
        }
        document.body.classList.add('printing-category');
        const cleanup = () => {
            document.body.classList.remove('printing-category');
            window.removeEventListener('afterprint', cleanup);
        };
        window.addEventListener('afterprint', cleanup);
        window.print();
        setTimeout(cleanup, 1500);
    }

    // ================================================================
    //  KM READINGS (Leituras de Quilometragem Mensal)
    // ================================================================
    async function saveKmReading() {
        const vehicleId = document.getElementById('kmReadingVehicle').value;
        const yearMonth = document.getElementById('kmReadingMonth').value;
        const km = parseFloat(document.getElementById('kmReadingValue').value);

        if (!vehicleId || !yearMonth || !km) {
            showToast('Preencha todos os campos (veículo, mês/ano e quilometragem).', 'error');
            return;
        }

        const existing = kmReadings.find(r => r.vehicleId === vehicleId && r.yearMonth === yearMonth);

        const reading = {
            vehicleId,
            yearMonth,
            km,
            updatedAt: new Date().toISOString()
        };

        if (firebaseConfigured) {
            try {
                if (existing) {
                    await db.collection('km_readings').doc(existing.id).update(reading);
                    const idx = kmReadings.findIndex(r => r.id === existing.id);
                    if (idx >= 0) kmReadings[idx] = { ...kmReadings[idx], ...reading };
                    showToast('Leitura atualizada com sucesso!', 'success');
                } else {
                    reading.createdAt = new Date().toISOString();
                    const docRef = await db.collection('km_readings').add(reading);
                    reading.id = docRef.id;
                    kmReadings.unshift(reading);
                    showToast('Leitura registrada com sucesso!', 'success');
                }
            } catch (err) {
                console.error('Erro Firebase:', err);
                showToast('Erro ao salvar no servidor.', 'error');
                return;
            }
        } else {
            if (existing) {
                const idx = kmReadings.findIndex(r => r.id === existing.id);
                if (idx >= 0) kmReadings[idx] = { ...kmReadings[idx], ...reading };
            } else {
                reading.id = generateId();
                reading.createdAt = new Date().toISOString();
                kmReadings.unshift(reading);
            }
            saveToLocalStorage();
        }

        document.getElementById('kmReadingValue').value = '';
        renderKmReadings();
        renderMileageReport();
    }

    async function deleteKmReading(id) {
        showConfirm('Tem certeza que deseja excluir esta leitura?', async () => {
            if (firebaseConfigured) {
                try {
                    await db.collection('km_readings').doc(id).delete();
                } catch (err) {
                    console.error('Erro Firebase:', err);
                    showToast('Erro ao excluir.', 'error');
                    return;
                }
            }
            kmReadings = kmReadings.filter(r => r.id !== id);
            if (!firebaseConfigured) saveToLocalStorage();
            showToast('Leitura excluída!', 'success');
            renderKmReadings();
            renderMileageReport();
        });
    }

    function renderKmReadings() {
        const container = document.getElementById('kmReadingsTableContainer');

        if (kmReadings.length === 0) {
            container.innerHTML = '<div class="empty-state"><span class="material-icons-outlined">edit_note</span><p>Nenhuma leitura registrada</p><small>Registre a quilometragem mensal dos veículos acima</small></div>';
            return;
        }

        const sorted = [...kmReadings].sort((a, b) => {
            if (b.yearMonth !== a.yearMonth) return b.yearMonth.localeCompare(a.yearMonth);
            const vA = vehicles.find(v => v.id === a.vehicleId);
            const vB = vehicles.find(v => v.id === b.vehicleId);
            return (vA?.name || '').localeCompare(vB?.name || '');
        });

        const rows = sorted.map(r => {
            const v = vehicles.find(v => v.id === r.vehicleId);
            const [year, month] = r.yearMonth.split('-');
            return `<tr>
                <td><strong>${v ? escapeHtml(v.name) : '-'}</strong></td>
                <td>${v ? escapeHtml(v.plate) : '-'}</td>
                <td>${month}/${year}</td>
                <td><strong>${Number(r.km).toLocaleString('pt-BR')} km</strong></td>
                <td>
                    <button class="btn btn-danger btn-sm btn-icon" onclick="deleteKmReading('${r.id}')" title="Excluir">
                        <span class="material-icons-outlined" style="font-size:16px">delete</span>
                    </button>
                </td>
            </tr>`;
        }).join('');

        container.innerHTML = `
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Veículo</th>
                            <th>Placa</th>
                            <th>Mês/Ano</th>
                            <th>Quilometragem</th>
                            <th>Ações</th>
                        </tr>
                    </thead>
                    <tbody>${rows}</tbody>
                </table>
            </div>
        `;
    }

    function formatYearMonth(ym) {
        const [y, m] = ym.split('-');
        return `${m}/${y}`;
    }

    function renderMileageReport() {
        const container = document.getElementById('kmReportContent');
        const vehicleId = document.getElementById('kmReportVehicle').value;
        const startYM = document.getElementById('kmReportStart').value;
        const endYM = document.getElementById('kmReportEnd').value;

        const targetVehicles = vehicleId ? vehicles.filter(v => v.id === vehicleId) : vehicles;

        if (targetVehicles.length === 0) {
            container.innerHTML = '<div class="empty-state"><span class="material-icons-outlined">route</span><p>Nenhum veículo cadastrado</p></div>';
            return;
        }

        let filteredReadings = [...kmReadings];
        if (startYM) filteredReadings = filteredReadings.filter(r => r.yearMonth >= startYM);
        if (endYM) filteredReadings = filteredReadings.filter(r => r.yearMonth <= endYM);

        let allRows = '';
        let hasData = false;

        targetVehicles.forEach(v => {
            const vReadings = filteredReadings
                .filter(r => r.vehicleId === v.id)
                .sort((a, b) => a.yearMonth.localeCompare(b.yearMonth));

            if (vReadings.length === 0) return;
            hasData = true;

            let monthRows = '';
            let totalKmDriven = 0;

            for (let i = 0; i < vReadings.length; i++) {
                const current = vReadings[i];
                let kmDriven = '-';
                let kmDrivenNum = 0;

                if (i === 0) {
                    // For the first reading in the range, try to find the previous month's reading
                    const prevMonthReadings = kmReadings
                        .filter(r => r.vehicleId === v.id && r.yearMonth < current.yearMonth)
                        .sort((a, b) => b.yearMonth.localeCompare(a.yearMonth));

                    if (prevMonthReadings.length > 0) {
                        kmDrivenNum = current.km - prevMonthReadings[0].km;
                        kmDriven = kmDrivenNum > 0 ? `${kmDrivenNum.toLocaleString('pt-BR')} km` : '-';
                        if (kmDrivenNum > 0) totalKmDriven += kmDrivenNum;
                    }
                } else {
                    kmDrivenNum = current.km - vReadings[i - 1].km;
                    kmDriven = kmDrivenNum > 0 ? `${kmDrivenNum.toLocaleString('pt-BR')} km` : '-';
                    if (kmDrivenNum > 0) totalKmDriven += kmDrivenNum;
                }

                // Calculate km/L for this month
                const [cYear, cMonth] = current.yearMonth.split('-');
                const monthStart = `${cYear}-${cMonth}-01`;
                const lastDay = new Date(parseInt(cYear), parseInt(cMonth), 0).getDate();
                const monthEnd = `${cYear}-${cMonth}-${String(lastDay).padStart(2, '0')}`;

                const monthFuelExps = expenses.filter(e =>
                    e.vehicleId === v.id &&
                    ['abastecimento', 'abastecimento_oleo'].includes(e.type) &&
                    e.date >= monthStart && e.date <= monthEnd &&
                    e.fuelLiters && parseFloat(e.fuelLiters) > 0
                );

                const totalLiters = monthFuelExps.reduce((sum, e) => sum + (parseFloat(e.fuelLiters) || 0), 0);
                let kmPerLiter = '-';
                if (kmDrivenNum > 0 && totalLiters > 0) {
                    kmPerLiter = (kmDrivenNum / totalLiters).toFixed(2).replace('.', ',') + ' km/L';
                }

                monthRows += `<tr>
                    <td>${formatYearMonth(current.yearMonth)}</td>
                    <td>${Number(current.km).toLocaleString('pt-BR')} km</td>
                    <td><strong>${kmDriven}</strong></td>
                    <td>${totalLiters > 0 ? totalLiters.toFixed(2).replace('.', ',') + ' L' : '-'}</td>
                    <td><strong>${kmPerLiter}</strong></td>
                </tr>`;
            }

            // Calculate total fuel and avg km/L across the period
            let periodStart = vReadings[0].yearMonth;
            let periodEnd = vReadings[vReadings.length - 1].yearMonth;
            const [sY, sM] = periodStart.split('-');
            const [eY, eM] = periodEnd.split('-');
            const periodStartDate = `${sY}-${sM}-01`;
            const lastDayEnd = new Date(parseInt(eY), parseInt(eM), 0).getDate();
            const periodEndDate = `${eY}-${eM}-${String(lastDayEnd).padStart(2, '0')}`;

            const periodFuelExps = expenses.filter(e =>
                e.vehicleId === v.id &&
                ['abastecimento', 'abastecimento_oleo'].includes(e.type) &&
                e.date >= periodStartDate && e.date <= periodEndDate &&
                e.fuelLiters && parseFloat(e.fuelLiters) > 0
            );
            const totalPeriodLiters = periodFuelExps.reduce((sum, e) => sum + (parseFloat(e.fuelLiters) || 0), 0);
            const avgKmL = totalKmDriven > 0 && totalPeriodLiters > 0
                ? (totalKmDriven / totalPeriodLiters).toFixed(2).replace('.', ',') + ' km/L'
                : '-';

            allRows += `
                <tr style="background: var(--primary-lightest);">
                    <td colspan="5" style="font-weight:700; color: var(--primary); padding: 12px 14px;">
                        ${escapeHtml(v.name)} - ${escapeHtml(v.plate)}
                    </td>
                </tr>
                ${monthRows}
                <tr style="background: #FFF3E0; font-weight: 600;">
                    <td>TOTAL</td>
                    <td></td>
                    <td>${totalKmDriven > 0 ? totalKmDriven.toLocaleString('pt-BR') + ' km' : '-'}</td>
                    <td>${totalPeriodLiters > 0 ? totalPeriodLiters.toFixed(2).replace('.', ',') + ' L' : '-'}</td>
                    <td>${avgKmL}</td>
                </tr>
            `;
        });

        if (!hasData) {
            container.innerHTML = '<div class="empty-state"><span class="material-icons-outlined">route</span><p>Nenhuma leitura encontrada</p><small>Registre a quilometragem mensal na seção acima</small></div>';
            return;
        }

        container.innerHTML = `
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Mês/Ano</th>
                            <th>KM Registrado</th>
                            <th>KM Rodado</th>
                            <th>Litros Abastecidos</th>
                            <th>Média (km/L)</th>
                        </tr>
                    </thead>
                    <tbody>${allRows}</tbody>
                </table>
            </div>
        `;
    }

    // ================================================================
    //  EXPENSE DETAIL MODAL
    // ================================================================
    function showExpenseDetail(id) {
        const e = expenses.find(ex => ex.id === id);
        if (!e) return;
        const v = vehicles.find(v => v.id === e.vehicleId);

        let html = `
            <div style="display: grid; gap: 12px;">
                <div><strong>Veículo:</strong> ${v ? escapeHtml(v.name) + ' (' + escapeHtml(v.plate) + ')' : 'Removido'}</div>
                <div><strong>Tipo:</strong> <span class="badge ${getTypeBadge(e.type)}">${getTypeName(e)}</span></div>
                <div><strong>Data:</strong> ${formatDate(e.date)}</div>
                <div><strong>Valor:</strong> <span style="font-size:1.2rem;font-weight:700;color:var(--primary)">R$ ${formatMoney(e.value)}</span></div>
                ${e.km ? `<div><strong>KM:</strong> ${Number(e.km).toLocaleString('pt-BR')}</div>` : ''}
        `;

        if (e.fuelLiters) html += `<div><strong>Litros:</strong> ${e.fuelLiters}</div>`;
        if (e.fuelPricePerLiter) html += `<div><strong>Preço/Litro:</strong> R$ ${formatMoney(e.fuelPricePerLiter)}</div>`;
        if (e.fuelType) html += `<div><strong>Combustível:</strong> ${e.fuelType}</div>`;
        if (e.maintenanceDesc) html += `<div><strong>Serviço:</strong> ${escapeHtml(e.maintenanceDesc)}</div>`;
        if (e.maintenanceProvider) html += `<div><strong>Oficina:</strong> ${escapeHtml(e.maintenanceProvider)}</div>`;
        if (e.customTypeName) html += `<div><strong>Tipo Personalizado:</strong> ${escapeHtml(e.customTypeName)}</div>`;
        if (e.customProvider) html += `<div><strong>Fornecedor:</strong> ${escapeHtml(e.customProvider)}</div>`;

        if (e.customFields && e.customFields.length > 0) {
            html += `<div><strong>Campos Adicionais:</strong></div>`;
            e.customFields.forEach(f => {
                html += `<div style="padding-left:16px;">• ${escapeHtml(f.name)}: ${escapeHtml(f.value)}</div>`;
            });
        }

        if (e.obs) html += `<div style="margin-top:8px;padding:12px;background:var(--primary-lightest);border-radius:8px;"><strong>Observação:</strong><br>${escapeHtml(e.obs)}</div>`;

        if (e.files && e.files.length > 0) {
            html += `<div style="margin-top:8px;"><strong>Comprovantes:</strong></div><div class="file-preview">`;
            e.files.forEach(url => {
                html += `<a href="${url}" target="_blank" class="file-preview-item"><img src="${url}" onerror="this.parentElement.innerHTML='<span class=\\'material-icons-outlined\\' style=\\'font-size:36px;color:var(--primary);display:flex;align-items:center;justify-content:center;height:100%\\'>description</span>'"></a>`;
            });
            html += `</div>`;
        }

        html += `</div>`;

        document.getElementById('detailBody').innerHTML = html;
        document.getElementById('detailModal').classList.add('active');
    }

    function closeDetail() {
        document.getElementById('detailModal').classList.remove('active');
    }

    // ================================================================
    //  NAVIGATION
    // ================================================================
    function showPage(page) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById('page-' + page).classList.add('active');
        document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
        event.target.closest('button')?.classList.add('active') ||
            document.querySelector(`nav button[onclick="showPage('${page}')"]`)?.classList.add('active');
    }

    // ================================================================
    //  CONFIRM DIALOG
    // ================================================================
    function showConfirm(message, callback) {
        document.getElementById('confirmMessage').textContent = message;
        confirmCallback = callback;
        document.getElementById('confirmModal').classList.add('active');
    }

    function closeConfirm() {
        document.getElementById('confirmModal').classList.remove('active');
        confirmCallback = null;
    }

    function confirmAction() {
        if (confirmCallback) confirmCallback();
        closeConfirm();
    }

    // ================================================================
    //  TOAST NOTIFICATIONS
    // ================================================================
    function showToast(message, type = 'info') {
        const container = document.getElementById('toastContainer');
        const toast = document.createElement('div');
        toast.className = `toast ${type}`;
        const icons = { success: 'check_circle', error: 'error', info: 'info' };
        toast.innerHTML = `<span class="material-icons-outlined" style="font-size:20px">${icons[type]}</span> ${message}`;
        container.appendChild(toast);
        setTimeout(() => {
            toast.style.opacity = '0';
            toast.style.transform = 'translateX(100%)';
            toast.style.transition = 'all 0.3s';
            setTimeout(() => toast.remove(), 300);
        }, 3000);
    }

    // ================================================================
    //  HELPERS
    // ================================================================
    function generateId() {
        return Date.now().toString(36) + Math.random().toString(36).substr(2, 9);
    }

    function escapeHtml(str) {
        const div = document.createElement('div');
        div.textContent = str;
        return div.innerHTML;
    }

    function formatDate(dateStr) {
        if (!dateStr) return '-';
        const [y, m, d] = dateStr.split('-');
        return `${d}/${m}/${y}`;
    }

    function formatMoney(value) {
        const num = parseFloat(value) || 0;
        return num.toFixed(2).replace('.', ',').replace(/\B(?=(\d{3})+(?!\d))/g, '.');
    }

    function parseMoney(str) {
        if (!str) return 0;
        return parseFloat(str.replace(/\./g, '').replace(',', '.')) || 0;
    }

    function maskMoney(input) {
        let v = input.value.replace(/\D/g, '');
        if (v.length === 0) { input.value = ''; return; }
        v = (parseInt(v) / 100).toFixed(2);
        input.value = v.replace('.', ',').replace(/\B(?=(\d{3})+(?!\d))/g, '.');
    }

    const TYPE_NAMES = {
        abastecimento: 'Abastecimento',
        oleo: 'Óleo',
        manutencao: 'Manutenção',
        revisao: 'Revisão',
        abastecimento_oleo: 'Abastecimento + Óleo',
        balde_oleo: 'Balde de Óleo',
        troca_oleo_filtro: 'Troca Óleo e Filtro',
        outro: 'Outro'
    };

    function getTypeName(expense) {
        if (expense.type === 'outro' && expense.customTypeName) return expense.customTypeName;
        return TYPE_NAMES[expense.type] || expense.type;
    }

    function getTypeBadge(type) {
        const badges = {
            abastecimento: 'badge-fuel',
            oleo: 'badge-oil',
            manutencao: 'badge-maintenance',
            revisao: 'badge-revision',
            abastecimento_oleo: 'badge-fuel',
            balde_oleo: 'badge-oil',
            troca_oleo_filtro: 'badge-oil',
            outro: 'badge-other'
        };
        return badges[type] || 'badge-other';
    }

    // Close modals on overlay click
    document.querySelectorAll('.modal-overlay').forEach(overlay => {
        overlay.addEventListener('click', (e) => {
            if (e.target === overlay) {
                overlay.classList.remove('active');
            }
        });
    });
    </script>
</body>
</html>
