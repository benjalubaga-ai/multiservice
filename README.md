<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multiservice - Dashboard</title>
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons+Outlined" rel="stylesheet">
    <style>
        :root {
            --primary: #1a73e8;
            --primary-bg: #e8f0fe;
            --sidebar-bg: #0b1a30;
            --bg: #f8f9fa;
            --card-bg: #ffffff;
            --text-main: #1f2937;
            --text-sub: #6b7280;
            --border: #e5e7eb;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg);
            color: var(--text-main);
            display: flex;
            height: 100vh;
            overflow: hidden;
        }

        /* BARRE LATÉRALE (SIDEBAR) */
        .sidebar {
            width: 260px;
            background-color: var(--sidebar-bg);
            color: white;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px 0;
        }

        .logo-section {
            padding: 0 24px 30px 24px;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.2rem;
            font-weight: bold;
            letter-spacing: 1px;
        }

        .logo-icon {
            color: var(--primary);
            font-size: 28px;
        }

        .nav-tabs {
            list-style: none;
            flex-grow: 1;
        }

        .nav-item {
            padding: 12px 24px;
            display: flex;
            align-items: center;
            gap: 15px;
            color: #94a3b8;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 0.95rem;
        }

        .nav-item:hover {
            background-color: rgba(255, 255, 255, 0.05);
            color: white;
        }

        .nav-item.active {
            background-color: var(--primary);
            color: white;
            border-radius: 0 25px 25px 0;
            margin-right: 15px;
        }

        .user-profiles {
            padding: 20px 15px 0 15px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .user-pill {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 8px;
            border-radius: 8px;
            margin-bottom: 8px;
            cursor: pointer;
        }

        .user-pill:hover {
            background-color: rgba(255, 255, 255, 0.05);
        }

        .user-avatar {
            width: 35px;
            height: 35px;
            border-radius: 50%;
            background-color: #cbd5e1;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: var(--sidebar-bg);
            font-size: 0.85rem;
        }

        .user-info th {
            font-weight: normal;
        }
        
        .user-name {
            font-size: 0.85rem;
            font-weight: 600;
        }

        .user-role {
            font-size: 0.75rem;
            color: #94a3b8;
        }

        /* CONTENU PRINCIPAL */
        .main-content {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
        }

        /* BARRE DE RECHERCHE / EN-TÊTE */
        .top-header {
            background-color: white;
            padding: 15px 30px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid var(--border);
        }

        .page-title {
            font-size: 1.4rem;
            font-weight: bold;
        }

        .search-bar {
            display: flex;
            align-items: center;
            background-color: #f1f5f9;
            padding: 8px 16px;
            border-radius: 8px;
            width: 400px;
            gap: 10px;
        }

        .search-bar input {
            border: none;
            background: transparent;
            outline: none;
            width: 100%;
            font-size: 0.9rem;
        }

        .header-actions {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .icon-btn {
            position: relative;
            cursor: pointer;
            color: var(--text-sub);
        }

        .badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background-color: var(--danger);
            color: white;
            font-size: 0.65rem;
            padding: 2px 5px;
            border-radius: 10px;
        }

        .btn-primary {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 10px 16px;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: 500;
            font-size: 0.9rem;
        }

        /* ZONE DU TABLEAU DE BORD */
        .dashboard-view {
            padding: 30px;
            display: flex;
            flex-direction: column;
            gap: 24px;
        }

        .welcome-text h2 {
            font-size: 1.6rem;
            margin-bottom: 4px;
        }

        .welcome-text p {
            color: var(--text-sub);
            font-size: 0.95rem;
        }

        /* GRILLE DES CARTES STATS */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
        }

        .stat-card {
            background-color: white;
            padding: 20px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            gap: 16px;
            border: 1px solid var(--border);
        }

        .stat-icon {
            width: 48px;
            height: 48px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .stat-card[data-type="total"] .stat-icon { background-color: #e8f0fe; color: var(--primary); }
        .stat-card[data-type="done"] .stat-icon { background-color: #e6f4ea; color: var(--success); }
        .stat-card[data-type="progress"] .stat-icon { background-color: #fef3c7; color: var(--warning); }
        .stat-card[data-type="delay"] .stat-icon { background-color: #fce8e6; color: var(--danger); }

        .stat-info .value {
            font-size: 1.5rem;
            font-weight: bold;
        }

        .stat-info .label {
            font-size: 0.85rem;
            color: var(--text-sub);
            margin-bottom: 4px;
        }

        .stat-trend {
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            gap: 4px;
        }
        .trend-up { color: var(--success); }
        .trend-down { color: var(--danger); }

        /* GRILLE SECONDAIRE (GRAPHES & LISTES) */
        .content-grid {
            display: grid;
            grid-template-columns: 1.4fr 1fr;
            gap: 24px;
        }

        .card {
            background-color: white;
            border-radius: 12px;
            padding: 24px;
            border: 1px solid var(--border);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .card-title {
            font-size: 1.1rem;
            font-weight: 600;
        }

        .card-link {
            font-size: 0.85rem;
            color: var(--primary);
            text-decoration: none;
            cursor: pointer;
        }

        /* APERÇU DÉPARTEMENTS */
        .dept-content {
            display: flex;
            align-items: center;
            gap: 30px;
        }

        .donut-chart-placeholder {
            width: 160px;
            height: 160px;
            border-radius: 50%;
            background: conic-gradient(
                var(--primary) 0% 45%, 
                var(--success) 45% 70%, 
                var(--warning) 70% 85%, 
                #a855f7 85% 95%, 
                #06b6d4 95% 100%
            );
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .donut-chart-placeholder::before {
            content: "";
            position: absolute;
            width: 110px;
            height: 110px;
            background-color: white;
            border-radius: 50%;
        }

        .donut-text {
            position: relative;
            z-index: 1;
            text-align: center;
        }

        .donut-num { font-size: 1.6rem; font-weight: bold; }
        .donut-lbl { font-size: 0.75rem; color: var(--text-sub); }

        .dept-list {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .dept-item {
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .dept-info {
            display: flex;
            justify-content: space-between;
            font-size: 0.85rem;
        }

        .dept-name {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .dept-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
        }

        .progress-bar {
            height: 6px;
            background-color: #f1f5f9;
            border-radius: 10px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            border-radius: 10px;
        }

        /* LISTE DES SERVICES */
        .service-list {
            display: flex;
            flex-direction: column;
            gap: 14px;
        }

        .service-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 10px;
            border-radius: 8px;
            border: 1px solid transparent;
            transition: all 0.2s;
            cursor: pointer;
        }

        .service-item:hover {
            border-color: var(--border);
            background-color: #f8f9fa;
        }

        .service-left {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .checkbox-mock {
            width: 18px;
            height: 18px;
            border: 2px solid #cbd5e1;
            border-radius: 50%;
        }

        .service-details .title {
            font-size: 0.9rem;
            font-weight: 500;
        }

        .service-details .tag {
            font-size: 0.75rem;
            color: var(--text-sub);
        }

        .service-right {
            display: flex;
            align-items: center;
            gap: 15px;
            font-size: 0.8rem;
        }

        .service-date { color: var(--text-sub); }

        .badge-priority {
            padding: 4px 8px;
            border-radius: 4px;
            font-weight: 600;
            font-size: 0.75rem;
        }
        .priority-haute { background-color: #fce8e6; color: var(--danger); }
        .priority-moyenne { background-color: #fef3c7; color: var(--warning); }
        .priority-basse { background-color: #e6f4ea; color: var(--success); }

        /* CALENDRIER MINI */
        .calendar-section {
            grid-column: 1 / 2;
        }

        .cal-grid {
            display: grid;
            grid-template-columns: 60px repeat(5, 1fr);
            border: 1px solid var(--border);
            border-radius: 8px;
            overflow: hidden;
        }

        .cal-header-cell {
            background-color: #f8f9fa;
            padding: 10px;
            text-align: center;
            font-size: 0.8rem;
            font-weight: 600;
            border-bottom: 1px solid var(--border);
            border-right: 1px solid var(--border);
        }

        .cal-header-cell.active-day .day-num {
            background-color: var(--primary);
            color: white;
            width: 24px;
            height: 24px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
        }

        .cal-row {
            display: contents;
        }

        .cal-cell {
            padding: 15px 8px;
            border-bottom: 1px solid var(--border);
            border-right: 1px solid var(--border);
            min-height: 60px;
            font-size: 0.75rem;
            position: relative;
        }

        .time-cell {
            color: var(--text-sub);
            text-align: center;
            font-weight: 500;
            background-color: #f8f9fa;
        }

        .event {
            padding: 6px;
            border-radius: 4px;
            font-size: 0.75rem;
            font-weight: 500;
        }
        .ev-technique { background-color: #e6f4ea; color: var(--success); }
        .ev-project { background-color: #e8f0fe; color: var(--primary); }
        .ev-rh { background-color: #fef3c7; color: var(--warning); }

        /* ÉQUIPE */
        .team-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .member-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 6px 0;
        }

        .member-left {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .member-status {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 0.8rem;
            color: var(--text-sub);
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
        }
        .status-online { background-color: var(--success); }
        .status-offline { background-color: var(--warning); }

        /* SÉLECTION DES ONGLETS VIDÉS */
        .view-section {
            display: none;
        }
        .view-section.active-view {
            display: block;
        }
        .empty-view {
            padding: 50px;
            text-align: center;
            color: var(--text-sub);
        }
    </style>
</head>
<body>

    <div class="sidebar">
        <div>
            <div class="logo-section">
                <span class="material-icons-outlined logo-icon">blur_on</span>
                <span>MULTISERVICE</span>
            </div>
            <ul class="nav-tabs">
                <li class="nav-item active" onclick="switchTab(this, 'accueil')">
                    <span class="material-icons-outlined">home</span> Accueil
                </li>
                <li class="nav-item" onclick="switchTab(this, 'service')">
                    <span class="material-icons-outlined">assignment_turned_in</span> Service
                </li>
                <li class="nav-item" onclick="switchTab(this, 'departements')">
                    <span class="material-icons-outlined">business</span> Départements
                </li>
                <li class="nav-item" onclick="switchTab(this, 'calendrier')">
                    <span class="material-icons-outlined">calendar_today</span> Calendrier
                </li>
                <li class="nav-item" onclick="switchTab(this, 'equipe')">
                    <span class="material-icons-outlined">people</span> Équipe
                </li>
                <li class="nav-item" onclick="switchTab(this, 'documents')">
                    <span class="material-icons-outlined">description</span> Documents
                </li>
                <li class="nav-item" onclick="switchTab(this, 'messages')">
                    <span class="material-icons-outlined">chat</span> Messages
                </li>
                <li class="nav-item" onclick="switchTab(this, 'rapports')">
                    <span class="material-icons-outlined">insert_chart</span> Rapports
                </li>
                <li class="nav-item" onclick="switchTab(this, 'parametres')">
                    <span class="material-icons-outlined">settings</span> Paramètres
                </li>
            </ul>
        </div>

        <div class="user-profiles">
            <div class="user-pill">
                <div class="user-avatar" id="avatar-main">IJ</div>
                <div class="user-info">
                    <div class="user-name" id="name-main">Ilunga jonathan</div>
                    <div class="user-role" id="role-main">Chef de projet</div>
                </div>
            </div>
            <div class="user-pill">
                <div class="user-avatar" id="avatar-sub">KM</div>
                <div class="user-info">
                    <div class="user-name" id="name-sub">Kemal malonga</div>
                    <div class="user-role" id="role-sub">Adjoint du Chef</div>
                </div>
            </div>
        </div>
    </div>

    <div class="main-content">
        <div class="top-header">
            <div class="page-title" id="current-tab-title">Accueil</div>
            <div class="search-bar">
                <span class="material-icons-outlined" style="color: var(--text-sub)">search</span>
                <input type="text" placeholder="Rechercher...">
            </div>
            <div class="header-actions">
                <div class="icon-btn">
                    <span class="material-icons-outlined">notifications</span>
                    <span class="badge">3</span>
                </div>
                <div class="icon-btn">
                    <span class="material-icons-outlined">mail</span>
                </div>
                <button class="btn-primary">
                    <span class="material-icons-outlined">add</span> Nouvelle tâche
                </button>
            </div>
        </div>

        <div id="view-accueil" class="view-section active-view dashboard-view">
            
            <div class="welcome-text">
                <h2>Bonjour, <span id="welcome-name">Ilunga jonathan</span> ! 👋</h2>
                <p>Voici ce qui se passe aujourd'hui.</p>
            </div>

            <div class="stats-grid">
                <div class="stat-card" data-type="total">
                    <div class="stat-icon"><span class="material-icons-outlined">format_list_bulleted</span></div>
                    <div class="stat-info">
                        <div class="value" id="stat-totaux">24</div>
                        <div class="label">Services totaux</div>
                        <div class="stat-trend trend-up"><span class="material-icons-outlined" style="font-size:14px">arrow_upward</span> 12% depuis hier</div>
                    </div>
                </div>
                <div class="stat-card" data-type="done">
                    <div class="stat-icon"><span class="material-icons-outlined">check_circle</span></div>
                    <div class="stat-info">
                        <div class="value" id="stat-termines">12</div>
                        <div class="label">Services terminés</div>
                        <div class="stat-trend trend-up"><span class="material-icons-outlined" style="font-size:14px">arrow_upward</span> 8% depuis hier</div>
                    </div>
                </div>
                <div class="stat-card" data-type="progress">
                    <div class="stat-icon"><span class="material-icons-outlined">schedule</span></div>
                    <div class="stat-info">
                        <div class="value" id="stat-encours">8</div>
                        <div class="label">En cours</div>
                        <div class="stat-trend trend-down"><span class="material-icons-outlined" style="font-size:14px">arrow_downward</span> 4% depuis hier</div>
                    </div>
                </div>
                <div class="stat-card" data-type="delay">
                    <div class="stat-icon"><span class="material-icons-outlined">flag</span></div>
                    <div class="stat-info">
                        <div class="value" id="stat-retard">4</div>
                        <div class="label">En retard</div>
                        <div class="stat-trend trend-down"><span class="material-icons-outlined" style="font-size:14px">arrow_downward</span> 2% depuis hier</div>
                    </div>
                </div>
            </div>

            <div class="content-grid">
                
                <div class="card">
                    <div class="card-header">
                        <div class="card-title">Aperçu des départements</div>
                        <select style="border:1px solid var(--border); padding:4px 8px; border-radius:6px; font-size:0.85rem; outline:none;">
                            <option>Tous les départements</option>
                        </select>
                    </div>
                    <div class="dept-content">
                        <div class="donut-chart-placeholder">
                            <div class="donut-text">
                                <div class="donut-num" id="global-progress">68%</div>
                                <div class="donut-lbl">Avancement global</div>
                            </div>
                        </div>
                        <div class="dept-list">
                            <div class="dept-item">
                                <div class="dept-info">
                                    <div class="dept-name"><span class="dept-dot" style="background-color: var(--primary)"></span>C.P.B</div>
                                    <div class="weight-bold" id="p-cpb">75%</div>
                                </div>
                                <div class="progress-bar"><div class="progress-fill" style="background-color: var(--primary); width: 75%"></div></div>
                            </div>
                            <div class="dept-item">
                                <div class="dept-info">
                                    <div class="dept-name"><span class="dept-dot" style="background-color: var(--success)"></span>Technique</div>
                                    <div class="weight-bold" id="p-tech">60%</div>
                                </div>
                                <div class="progress-bar"><div class="progress-fill" style="background-color: var(--success); width: 60%"></div></div>
                            </div>
                            <div class="dept-item">
                                <div class="dept-info">
                                    <div class="dept-name"><span class="dept-dot" style="background-color: var(--warning)"></span>Ressources Humaines</div>
                                    <div class="weight-bold" id="p-rh">40%</div>
                                </div>
                                <div class="progress-bar"><div class="progress-fill" style="background-color: var(--warning); width: 40%"></div></div>
                            </div>
                            <div class="dept-item">
                                <div class="dept-info">
                                    <div class="dept-name"><span class="dept-dot" style="background-color: #a855f7"></span>Finance</div>
                                    <div class="weight-bold" id="p-fin">30%</div>
                                </div>
                                <div class="progress-bar"><div class="progress-fill" style="background-color: #a855f7; width: 30%"></div></div>
                            </div>
                            <div class="dept-item">
                                <div class="dept-info">
                                    <div class="dept-name"><span class="dept-dot" style="background-color: #06b6d4"></span>Construction</div>
                                    <div class="weight-bold" id="p-const">20%</div>
                                </div>
                                <div class="progress-bar"><div class="progress-fill" style="background-color: #06b6d4; width: 20%"></div></div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <div class="card-header">
                        <div class="card-title">Services récents</div>
                        <div class="card-link">Voir tout</div>
                    </div>
                    <div class="service-list">
                        <div class="service-item">
                            <div class="service-left">
                                <div class="checkbox-mock"></div>
                                <div class="service-details">
                                    <div class="title" id="t1-title">Créer la maquette de la page d'accueil</div>
                                    <div class="tag">C.P.B</div>
                                </div>
                            </div>
                            <div class="service-right">
                                <div class="service-date">25 mai</div>
                                <span class="badge-priority priority-haute">Haute</span>
                            </div>
                        </div>
                        <div class="service-item" style="background-color: #f1f5f9; border-color: var(--border);">
                            <div class="service-left">
                                <div class="checkbox-mock" style="background-color: var(--primary); border-color: var(--primary)"></div>
                                <div class="service-details">
                                    <div class="title" id="t2-title">Intégrer l'API de paiement</div>
                                    <div class="tag">C.P.B</div>
                                </div>
                            </div>
                            <div class="service-right">
                                <div class="service-date">27 mai</div>
                                <span class="badge-priority priority-moyenne">Moyenne</span>
                            </div>
                        </div>
                        <div class="service-item">
                            <div class="service-left">
                                <div class="checkbox-mock"></div>
                                <div class="service-details">
                                    <div class="title" id="t3-title">Rédiger le contenu SEO</div>
                                    <div class="tag">Technique</div>
                                </div>
                            </div>
                            <div class="service-right">
                                <div class="service-date">28 mai</div>
                                <span class="badge-priority priority-moyenne">Moyenne</span>
                            </div>
                        </div>
                        <div class="service-item">
                            <div class="service-left">
                                <div class="checkbox-mock"></div>
                                <div class="service-details">
                                    <div class="title" id="t4-title">Préparer la présentation client</div>
                                    <div class="tag">Technique</div>
                                </div>
                            </div>
                            <div class="service-right">
                                <div class="service-date">30 mai</div>
                                <span class="badge-priority priority-basse">Basse</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="card calendar-section">
                    <div class="card-header">
                        <div class="card-title">Calendrier</div>
                        <div class="card-link">Voir le calendrier</div>
                    </div>
                    <div class="cal-grid">
                        <div class="cal-header-cell"></div>
                        <div class="cal-header-cell">Lun 20 mai</div>
                        <div class="cal-header-cell active-day"><div class="day-num">21</div><div style="font-size:10px;color:var(--text-sub)">mai</div></div>
                        <div class="cal-header-cell">Mer 22 mai</div>
                        <div class="cal-header-cell">Jeu 23 mai</div>
                        <div class="cal-header-cell">Ven 24 mai</div>

                        <div class="cal-row">
                            <div class="cal-cell time-cell">09:00</div>
                            <div class="cal-cell"><div class="event ev-technique">Réunion d'équipe<br><span style="font-size:10px">09:00 - 10:00</span></div></div>
                            <div class="cal-cell"></div>
                            <div class="cal-cell"><div class="event ev-project">Appel client<br><span style="font-size:10px">09:00 - 10:00</span></div></div>
                            <div class="cal-cell"></div>
                            <div class="cal-cell"></div>
                        </div>

                        <div class="cal-row">
                            <div class="cal-cell time-cell">10:00</div>
                            <div class="cal-cell"></div>
                            <div class="cal-cell"><div class="event ev-project">Point projet<br><span style="font-size:10px">09:30 - 10:30</span></div></div>
                            <div class="cal-cell"></div>
                            <div class="cal-cell"><div class="event ev-rh">Revue design<br><span style="font-size:10px">10:00 - 11:00</span></div></div>
                            <div class="cal-cell"></div>
                        </div>

                        <div class="cal-row">
                            <div class="cal-cell time-cell">11:00</div>
                            <div class="cal-cell"></div>
                            <div class="cal-cell"><div class="event ev-danger" style="background-color:#fee2e2; color:var(--danger)">Rédaction contenu<br><span style="font-size:10px">11:00 - 12:00</span></div></div>
                            <div class="cal-cell"></div>
                            <div class="cal-cell"></div>
                            <div class="cal-cell"><div class="event ev-technique">Présentation<br><span style="font-size:10px">11:00 - 12:00</span></div></div>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <div class="card-header">
                        <div class="card-title">Membres de l'équipe</div>
                        <div class="card-link">Voir tout</div>
                    </div>
                    <div class="team-list">
                        <div class="member-item">
                            <div class="member-left">
                                <div class="user-avatar" style="background-color:#fed7aa">ED</div>
                                <div class="user-info">
                                    <div class="user-name" id="m1-name">Emma Dupont</div>
                                    <div class="user-role">Designer UI/UX</div>
                                </div>
                            </div>
                            <div class="member-status"><span class="status-dot status-online"></span>En ligne</div>
                        </div>
                        <div class="member-item">
                            <div class="member-left">
                                <div class="user-avatar" style="background-color:#bbf7d0">LB</div>
                                <div class="user-info">
                                    <div class="user-name" id="m2-name">Lucas Bernard</div>
                                    <div class="user-role">Développeur Frontend</div>
                                </div>
                            </div>
                            <div class="member-status"><span class="status-dot status-online"></span>En ligne</div>
                        </div>
                        <div class="member-item">
                            <div class="member-left">
                                <div class="user-avatar" style="background-color:#fef08a">CP</div>
                                <div class="user-info">
                                    <div class="user-name" id="m3-name">Chloé Petit</div>
                                    <div class="user-role">Chargée de marketing</div>
                                </div>
                            </div>
                            <div class="member-status"><span class="status-dot status-offline"></span>Absent</div>
                        </div>
                        <div class="member-item">
                            <div class="member-left">
                                <div class="user-avatar" style="background-color:#e9d5ff">AL</div>
                                <div class="user-info">
                                    <div class="user-name" id="m4-name">Antoine Lefèvre</div>
                                    <div class="user-role">Développeur Backend</div>
                                </div>
                            </div>
                            <div class="member-status"><span class="status-dot status-online"></span>En ligne</div>
                        </div>
                    </div>
                </div>

            </div>
        </div>

        <div id="view-service" class="view-section empty-view"><h2>Page Service</h2><p>Contenu de l'onglet Service.</p></div>
        <div id="view-departements" class="view-section empty-view"><h2>Page Départements</h2><p>Gestion des départements.</p></div>
        <div id="view-calendrier" class="view-section empty-view"><h2>Page Calendrier</h2><p>Vue calendrier complète.</p></div>
        <div id="view-equipe" class="view-section empty-view"><h2>Page Équipe</h2><p>Liste complète des effectifs.</p></div>
        <div id="view-documents" class="view-section empty-view"><h2>Page Documents</h2><p>Gestionnaire de fichiers.</p></div>
        <div id="view-messages" class="view-section empty-view"><h2>Page Messages</h2><p>Boîte de réception et messagerie.</p></div>
        <div id="view-rapports" class="view-section empty-view"><h2>Page Rapports</h2><p>Analyses statistiques détaillées.</p></div>
        <div id="view-parametres" class="view-section empty-view"><h2>Page Paramètres</h2><p>Configuration de l'application.</p></div>

    </div>

    <script>
        // Système d'onglets de navigation cliquables
        function switchTab(element, viewId) {
            // Désactiver l'ancien onglet actif
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            // Masquer toutes les sections
            document.querySelectorAll('.view-section').forEach(view => view.classList.remove('active-view'));
            
            // Activer le nouvel onglet visuellement
            element.classList.add('active');
            // Afficher le contenu lié
            document.getElementById('view-' + viewId).classList.add('active-view');
            // Mettre à jour le titre du haut
            document.getElementById('current-tab-title').innerText = element.innerText.trim();
        }

        // Script d'édition en direct sur simple clic
        // Cliquez sur un texte de l'interface pour le modifier instantanément !
        const editableElements = [
            'name-main', 'role-main', 'name-sub', 'role-sub', 'welcome-name',
            'stat-totaux', 'stat-termines', 'stat-encours', 'stat-retard',
            'global-progress', 'p-cpb', 'p-tech', 'p-rh', 'p-fin', 'p-const',
            't1-title', 't2-title', 't3-title', 't4-title',
            'm1-name', 'm2-name', 'm3-name', 'm4-name'
        ];

        editableElements.forEach(id => {
            const el = document.getElementById(id);
            if(el) {
                el.style.cursor = "pointer";
                el.title = "Cliquez pour modifier";
                el.addEventListener('click', function() {
                    const currentText = this.innerText;
                    const newValue = prompt("Modifier la valeur :", currentText);
                    if(newValue !== null && newValue.trim() !== "") {
                        this.innerText = newValue;
                        
                        // Si on modifie le nom dans le profil en bas, met à jour le message de bienvenue en haut
                        if(id === 'name-main') {
                            document.getElementById('welcome-name').innerText = newValue;
                            document.getElementById('avatar-main').innerText = newValue.split(' ').map(n => n[0]).join('').toUpperCase().substring(0,2);
                        }
                    }
                });
            }
        });
    </script>
</body>
</html>
