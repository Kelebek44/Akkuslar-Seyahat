<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V83</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@700&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: rgba(20,20,20,0.85); --gold: #FFD700; --admin: #00CCFF; --yon: #AA00FF; }
        * { box-sizing: border-box; transition: 0.3s; font-family: 'DM Sans', sans-serif; }
        body { 
            margin: 0; 
            background: var(--bg); 
            color: #EEE; 
            display: flex; 
            height: 100vh; 
            overflow: hidden; 
            background-size: cover !important; 
            background-position: center !important; 
            background-attachment: fixed !important;
        }
        .sidebar { width: 280px; background: rgba(0,0,0,0.95); border-right: 2px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; z-index: 10; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 5px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 11px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover, .active-btn { background: var(--p); color: white; transform: translateX(5px); }
        .main { flex: 1; padding: 40px; overflow-y: auto; background: rgba(0,0,0,0.3); z-index: 5; }
        .panel { display: none; }
        .aktif { display: block !important; }
        .card { background: var(--card); padding: 25px; border-radius: 15px; border: 1px solid #333; margin-bottom: 20px; backdrop-filter: blur(10px); }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; }
        .badge { padding: 4px 10px; border-radius: 5px; font-size: 10px; font-weight: bold; margin-bottom: 10px; display: inline-block; text-transform: uppercase; }
        .b-kurucu { background: var(--gold); color: #000; }
        .b-admin { background: var(--admin); color: #000; }
        .b-yonetici { background: var(--yon); color: #fff; }
        
        /* Yetki Görünürlükleri */
        .kurucu-only, .editor-only { display: none; }
        .kurucu-mode .kurucu-only, .kurucu-mode .editor-only { display: block !important; }
        .admin-mode .editor-only, .yonetici-mode .editor-only { display: block !important; }
        
        .del-btn { background: #800; color: white; border: none; padding: 8px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; width: 100%; }
        .kurucu-mode .del-btn { display: block !important; }
        
        input, select, textarea { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 15px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        .back-btn { background: #333; color: #fff; padding: 8px 15px; border-radius: 5px; border: none; cursor: pointer; margin-bottom: 20px; font-weight: bold; }
        .prev-img { width: 100%; border-radius: 8px; border: 1px solid #444; margin-bottom: 10px; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0; font-family:'Space Grotesk';">AKKUŞLAR SEYAHAT</h2>
        <div style="color:var(--gold); font-size:12px; margin-top:5px; font-weight:bold;">👑 乂✯ҠƐꝈƐβƐҠ✯乂</div>
        <div style="color:#888; font-size:10px; text-transform:uppercase; letter-spacing:1px;">Kurucu</div>
    </div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Ekran</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Üyeleri</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🛡️ Yönetim Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(4, this)">📜 Ekip Kuralları</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🖼️ Galeri / Kaplama</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar / Müzikler</button>
    <button class="nav-btn" onclick="sayfa(12, this)">🛡️ Yönetim Telsizi</button>
    <button class="nav-btn" onclick="sayfa(5, this)">✍️ Ekibe Katıl</button>
    <button class="nav-btn" onclick="sayfa(15, this)">📞 İletişim</button>

    <div style="margin-top:auto; padding-top:20px; border-top:1px solid #222;">
        <button class="nav-btn" style="color:var(--gold);" onclick="giris('kurucu')">👑 KURUCU GİRİŞİ</button>
        <button class="nav-btn" style="color:var(--admin);" onclick="giris('admin')">🛡️ ADMİN GİRİŞİ</button>
        <button class="nav-btn" style="color:var(--yon);" onclick="giris('yonetici')">⚙️ YÖNETİCİ GİRİŞİ</button>
    </div>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <div class="card" style="text-align:center; background:rgba(0,0,0,0.6);">
            <h1 style="font-size:3.5rem; margin:0; font-family:'Space Grotesk'; color:white;">AKKUŞLAR SEYAHAT</h1>
            <p style="color:var(--gold); font-weight:bold; letter-spacing:2px;">👑 KURUCU: 乂✯ҠƐꝈƐβƐҠ✯乂</p>
        </div>
        <div class="grid">
            <div class="card" style="text-align:center;"><h3>Normal Üye</h3><h1 id="count-uye" style="color:var(--p); margin:0;">0</h1></div>
            <div class="card" style="text-align:center;"><h3>Yönetim Kadrosu</h3><h1 id="count-yon" style="color:var(--gold); margin:0;">0</h1></div>
        </div>
        <div class="card">
            <h3>📢 Ekip Telsizi</h3>
            <div id="msg-list-uye" style="height:250px; overflow-y:auto; background:rgba(0,0,0,0.8); padding:15px; border-radius:10px; border:1px solid #333;"></div>
            <div style="display:flex; gap:10px; margin-top:15px;">
                <input type="text" id="nick-uye" placeholder="İsim" style="width:20%;">
                <input type="text" id="msg-uye" placeholder="Mesaj..." style="width:80%;">
                <button onclick="mesajGonder('sohbet_uye', 'nick-uye', 'msg-uye')" class="action-btn" style="width:100px;">GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p2" class="panel"><button class="back-btn" onclick="sayfa(1)">🏠 Geri Dön</button><h1>👥 Ekip Üyeleri</h1><div id="list-uye-adlari" class="grid"></div></div>
    <div id="p3" class="panel"><button class="back-btn" onclick="sayfa(1)">🏠 Geri Dön</button><h1>🛡️ Yönetim Kadrosu</h1><div id="list-yon-adlari" class="grid"></div></div>
    <div id="p8" class="panel"><button class="back-btn" onclick="sayfa(1)">🏠 Geri Dön</button><h1>🖼️ Galeri & Kaplamalar</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p7" class="panel"><button class="back-btn" onclick="sayfa(1)">🏠 Geri Dön</button><h1>🛠️ Modlar & Müzikler</h1><div id="list-mod" class="grid"></div></div>

    <div id="p13" class="panel">
        <button class="back-btn" onclick="sayfa(1)">🏠 Geri Dön</button>
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div class="card editor-only">
                <h3>DOSYA/RESİM EKLE (URL)</h3>
                <select id="i-tip"><option value="galeri">Galeri / Kaplama</option><option value="mod">Mod Merkezi</option></select>
                <input type="text" id="i-bas" placeholder="Başlık">
                <input type="text" id="i-url" placeholder="URL Linkini Yapıştır">
                <button class="action-btn" onclick="urlIleYukle()">EKLE</button>
            </div>
            <div class="card kurucu-only" style="border:2px solid var(--gold);">
                <h3>🖼️ ANA EKRAN RESMİ</h3>
                <input type="text" id="bg-url" placeholder="Arka Plan URL Yapıştır">
                <button class="action-btn" onclick="setBg()">TAM EKRAN YAP</button>
            </div>
            <div class="card editor-only"><h3>📥 ÜYE BAŞVURULARI</h3><div id="list-basvuru"></div></div>
            <div class="card kurucu-only"><h3>👑 ROL VERME</h3><input type="text" id="r-ad" placeholder="İsim"><select id="r-rol"><option value="admin">Admin</option><option value="yonetici">Yönetici</option><option value="uye">Üye</option></select><button class="action-btn" onclick="rolVer()">KAYDET</button></div>
        </div>
    </div>

    <div id="p15" class="panel">
        <button class="back-btn" onclick="sayfa(1)">🏠 Geri Dön</button>
        <h1>📞 İletişim Bilgileri</h1>
        <div class="card" style="text-align:center;">
            <p>TikTok 1: <a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" style="color:var(--p)">@kelebekmiisaliii</a></p>
            <p>TikTok 2: <a href="
