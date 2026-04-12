<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | KURUCU PANELİ</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; }
        * { box-sizing: border-box; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'DM Sans', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        /* SOL MENÜ */
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; margin-bottom: 25px; border-bottom: 1px solid #222; padding-bottom: 15px; }
        .logo-text { color: var(--p); font-size: 22px; font-weight: 900; font-family: 'Space Grotesk'; text-transform: uppercase; margin: 0; }
        .kurucu-text { color: var(--gold); font-size: 13px; font-weight: bold; margin-top: 5px; display: block; }
        
        .nav-label { color: #444; font-size: 10px; font-weight: bold; text-transform: uppercase; margin: 15px 0 8px; letter-spacing: 1px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; transition: 0.3s; font-family: 'Space Grotesk'; font-size: 13px; }
        .nav-btn:hover { background: #1a1a1a; color: #fff; border-left: 4px solid var(--p); }
        .active-btn { background: var(--p) !important; color: white !important; box-shadow: 0 4px 15px rgba(204, 0, 0, 0.3); }

        /* ANA ALAN */
        .main { flex: 1; padding: 35px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #222; text-align: center; position: relative; transition: 0.3s; }
        .card:hover { border-color: var(--p); transform: translateY(-5px); }
        .card img { width: 100%; height: 160px; object-fit: cover; border-radius: 10px; margin-bottom: 15px; }
        
        /* FORM ELEMENTLERİ */
        input, select, textarea { width: 100%; padding: 12px; background: #050505; border: 1px solid #333; color: white; border-radius: 8px; margin-bottom: 10px; outline: none; }
        input:focus { border-color: var(--p); }
        .action-btn { background: var(--p); color: white; border: none; padding: 14px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; }
        
        /* SİLME BUTONU - SADECE KURUCUDA ÇIKAR */
        .del-btn { background: #600; color: white; border: none; padding: 6px 12px; border-radius: 6px; cursor: pointer; font-size: 11px; margin-top: 10px; display: none; }
        .kurucu-modu .del-btn { display: inline-block !important; }

        /* ROZETLER */
        .badge { display: inline-block; padding: 4px 10px; border-radius: 5px; font-size: 10px; font-weight: bold; text-transform: uppercase; margin-bottom: 10px; }
        .badge-kurucu { background: var(--gold); color: #000; }
        .badge-admin { background: var(--p); color: #fff; }
        .badge-uye { background: #444; color: #fff; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h1 class="logo-text">AKKUŞLAR SEYAHAT</h1>
        <span class="kurucu-text">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>

    <div class="nav-label">TERMİNAL</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Terminal</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet İşlemleri</button>
    <button class="nav-btn" onclick="sayfa(4, this)">🛣️ Konvoy Saatleri</button>

    <div class="nav-label">ARŞİV & MEDYA</div>
    <button class="nav-btn" onclick="sayfa(5, this)">🎞️ Galeri (Resimler)</button>
    <button class="nav-btn" onclick="sayfa(6, this)">🎥 Video Merkezi</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar & Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🎵 Müzik Listesi</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🖼️ Arka Planlar</button>

    <div class="nav-label">SİSTEM</div>
    <button class="nav-btn" onclick="sayfa(11, this)">📞 İletişim & TikTok</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katıl</button>
    <button class="nav-btn" style="color:var(--p); border: 1px dashed var(--p);" onclick="yonetimGiris()">⚙️ YÖNETİM PANELİ</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <h1 style="font-family:'Space Grotesk'; font-size:40px; margin-bottom:10px;">AKKUŞLAR SEYAHAT</h1>
        <p style="color:#888; font-size:18px;">ETS2 Dünyasının En Prestijli Filosuna Hoş Geldiniz.</p>
        <div class="grid" style="margin-top:40px;">
            <div class="card"><h3>9</h3><p>Aktif Kaptan</p></div>
            <div class="card"><h3>24/7</h3><p>Canlı Terminal</p></div>
            <div class="card"><h3>%100</h3><p>Profesyonellik</p></div>
        </div>
    </div>

    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>

    <div id="p3" class="panel">
        <h1>🎫 Bilet Sistemi</h1>
        <div class="card" style="max-width:400px; margin-bottom:30px;">
            <input type="text" id="bilet-ad" placeholder="Yolcu Adı Soyadı...">
            <button class="action-btn" onclick="biletKes()">BİLETİ ONAYLA</button>
        </div>
        <div id="list-bilet" class="grid"></div>
    </div>

    <div id="p4" class="panel">
        <h1>🛣️ Konvoy Takvimi</h1>
        <div class="card">
            <table style="width:100%; text-align:left; border-collapse:collapse;">
                <tr style="border-bottom:1px solid #333;">
                    <p style="color:var(--p); font-weight:bold;">Rota: Bartın -> Denizli</p>
                    <p>Tarih: Her Cumartesi | Saat: 21:00</p>
                    <p>Sunucu: Simulation 1</p>
                </tr>
            </table>
        </div>
    </div>

    <div id="p5" class="panel"><h1>🎞️ Galeri</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p6" class="panel"><h1>🎥 Video Galerisi</h1><div id="list-video" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🛠️ Modlar & Kaplamalar</h1><div id="list-mod" class="grid"></div></div>
    <div id="p8" class="panel"><h1>🎵 Müzik Listesi</h1><div id="list-muzik" class="grid"></div></div>
    <div id="p10" class="panel"><h1>🖼️ Arka Planlar</h1><div id="list-bg" class
