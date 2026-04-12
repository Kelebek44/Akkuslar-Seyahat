<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | RESMİ TERMİNAL V39</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; }
        * { box-sizing: border-box; scroll-behavior: smooth; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        /* SIDEBAR */
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .logo-text { color: var(--p); font-size: 20px; font-weight: 900; font-family: 'Space Grotesk'; margin: 0; }
        .kurucu-label { color: var(--gold); font-size: 12px; font-weight: bold; margin-top: 5px; display: block; }
        
        .nav-label { color: #444; font-size: 10px; font-weight: bold; text-transform: uppercase; margin: 15px 0 5px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 5px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; transition: 0.3s; }
        .nav-btn:hover { background: var(--p); color: white; }
        .active { background: var(--p) !important; color: white !important; }

        /* MAIN CONTENT */
        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif-panel { display: block !important; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 20px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #222; text-align: center; position: relative; }
        .card img { width: 100%; height: 150px; object-fit: cover; border-radius: 10px; margin-bottom: 15px; }
        
        /* YÖNETİM & ROL SİSTEMİ */
        .badge { display: inline-block; padding: 4px 10px; border-radius: 5px; font-size: 10px; font-weight: bold; text-transform: uppercase; margin-bottom: 10px; }
        .badge-kurucu { background: var(--gold); color: #000; }
        .badge-admin { background: #00CCFF; color: #000; }
        .badge-uye { background: #444; color: #fff; }
        
        .del-btn { background: #600; color: white; border: none; padding: 6px 12px; border-radius: 6px; cursor: pointer; display: none; margin-top: 10px; font-weight: bold; }
        .kurucu-modu .del-btn { display: inline-block !important; }

        input, select, textarea { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: white; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        
        .chat-box { height: 300px; overflow-y: auto; background: #050505; border: 1px solid #333; padding: 15px; border-radius: 10px; text-align: left; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h1 class="logo-text">AKKUŞLAR SEYAHAT</h1>
        <span class="kurucu-label">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>

    <div class="nav-label">TERMİNAL</div>
    <button class="nav-btn active" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Üyeleri</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🛣️ Konvoy Rotaları</button>
    <button class="nav-btn" onclick="sayfa(4, this)">🎫 Bilet Sistemi</button>

    <div class="nav-label">ARŞİV & ATÖLYE</div>
    <button class="nav-btn" onclick="sayfa(5, this)">🎞️ Galeri</button>
    <button class="nav-btn" onclick="sayfa(6, this)">🎥 Videolar</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🎨 Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(9, this)">🎵 Müzikler</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🖼️ Arka Planlar</button>

    <div class="nav-label">SİSTEM</div>
    <button class="nav-btn" onclick="sayfa(11, this)">🎮 Mini Oyun</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katıl</button>
    <button class="nav-btn" onclick="sayfa(13, this)">📞 İletişim (TikTok)</button>
    <button class="nav-btn" style="border: 1px dashed var(--p); color:var(--p); margin-top:10px;" onclick="yonetimGiris()">⚙️ YÖNETİM PANELİ</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif-panel">
        <img id="main-banner" style="width:100%; height:300px; object-fit:cover; border-radius:20px; border:2px solid var(--p); display:none; margin-bottom:20px;">
        <h1>🚛 AKKUŞLAR SEYAHAT TERMİNALİ</h1>
        <p>Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</p>
        <div class="grid">
            <div class="card"><h3 id="count-uye">0</h3><p>Kayıtlı Üye</p></div>
            <div class="card"><h3>AKTİF</h3><p>Sefer Durumu</p></div>
        </div>
    </div>

    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>

    <div id="p3" class="panel">
        <h1>🛣️ Konvoy Rotaları</h1>
        <div class="grid">
            <div class="card"><h3>İSTANBUL ➔ DENİZLİ</h3><p>Durum: Hazır</p></div>
            <div class="card"><h3>ANKARA ➔ HATAY</h3><p>Durum: Hazır</p></div>
            <div class="card"><h3>BARTIN ➔ DENİZLİ</h3><p>Durum: Hazır</p></div>
        </div>
    </div>

    <div id="p5" class="panel"><h1>🎞️ Galeri</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p6" class="panel"><h1>🎥 Videolar</h1><div id="list-video" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🛠️ Mod Merkezi</h1><div id="list-mod" class="grid"></div></div>
    <div id="p8" class="panel"><h1>🎨 Kaplamalar</h1><div id="list-kaplama" class="grid"></div></div>
    <div id="p9" class="panel"><h1>🎵 Müzik Listesi</h1><div id="list-muzik" class="grid"></div></div>
    <div id="p10" class="panel"><h1>🖼️ Arka Planlar</h1><div id="list-bg" class="grid"></div></div>

    <div id="p11" class="panel">
        <h1>🎮 Akkuşlar Tır Sürme</h1>
        <canvas id="gameCanvas" width="600" height="300" style="background:#111; border:3px solid var(--p); display:block; margin:auto;"></canvas>
    </div>

    <div id="p13" class="panel">
        <h1>📞 İletişim & TikTok</h1>
        <div class="grid">
            <div class="card"><h3>TikTok: Kelebek</h3><a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" class="action-btn" style="text-decoration:none;">TAKİP ET</a></div>
            <div class="card"><h3>TikTok: Akkuş Ailesi</h3><a href="https://www.tiktok.com/@akkusailesi20" target="_blank" class="action-btn" style="text-decoration:none;">TAKİP ET</a></div>
        </div>
    </div>

    <div id="p15" class="panel">
        <h1 style="color:var(--p)">⚙️ TERMİNAL YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div class="card" id="kurucu-panel" style="display:none; border:2px solid var(--gold);">
                <span class="badge badge-kurucu">Kurucu Yetkisi</span>
                <h3>Rol Ver / Üye Ekle</h3>
                <input type="text" id="e-ad" placeholder="Üye Adı...">
                <select id="e-rol"><option value="uye">Üye</option><option value="admin">Yönetici</option></select>
                <button class="action-btn" onclick="ekipEkle()">SİSTEME KAYDET</button>
                <hr style="border:1px solid #333; margin:15px 0;">
                <h3>Ana Sayfa Afişi</h3>
                <input type="text" id="afis-url" placeholder="Resim Linki...">
                <button class="action-btn" onclick="afisGuncelle()" style="background:var(--gold); color:#000;">AFİŞİ YAYINLA</button>
            </div>

            <div class="card">
                <span class="badge badge-admin">Editör Yetkisi</span>
                <h3>İçerik Yükle</h3>
                <select id="i-tip">
                    <option value="galeri">Resim</option>
                    <option value="video">Video</option>
                    <option value="mod">Mod</option>
                    <option value="kaplama">Kaplama</option>
                    <option value="muzik">Müzik</option>
                    <option value="bg">Arka Plan</option>
                </select>
                <input type="text" id="i-baslik" placeholder="Başlık...">
                <input type="text" id="i-url" placeholder="URL Linki...">
                <button class="action-btn" onclick="icerikEkle()">YAYINLA</button>
            </div>

            <div class="card">
                <h3>💬 Yönetici Sohbeti</h3>
                <div id="chat-list" class="chat-box"></div>
                <input type="text" id="chat-in" placeholder="Mesajınız...">
                <button class="action-btn" onclick="msgGonder()">GÖNDER</button>
            </div>
        </div>
        <button class="action-btn" style="background:#222; margin-top:20px;" onclick="location.reload()">GÜVENLİ ÇIKIŞ</button>
    </div>
</div>

<script>
    const firebaseConfig = {
      apiKey: "AIzaSyCowK8jtpCENPWxz0EpANeRghLXlYMVkcg",
      authDomain: "akkuslar-seyahat-f3577.firebaseapp.com",
      projectId: "akkuslar-seyahat-f3577",
      storageBucket: "akkuslar-seyahat-f3577.firebasestorage.app",
      messagingSenderId: "207828517432",
      appId: "1:207828517432:web:dedb92a79fe1d17aec9ccd"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    let rol = "uye";

    function yonetimGiris() {
        let s = prompt("Yetkili Şifresi:");
        if(s === "1907akkus") {
            rol = "kurucu";
            document.getElementById('app-body').classList.add('kurucu-modu');
            document.getElementById('kurucu-panel').style.display = "block";
            sayfa(15);
        } else if(s === "akkus123") {
            rol = "admin";
            sayfa(15);
        } else { alert("Hatalı Şifre!"); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif-panel'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
        document.getElementById('p' + n).classList.add('aktif-panel');
        if(btn) btn.classList.add('active');
        if(n === 11) initGame();
    }

    // ANA SAYFA AFİŞİ
    db.collection("ayarlar").doc("afis").onSnapshot(d => {
        if(d.exists && d.data().url) {
            const b = document.getElementById('main-banner');
            b.src = d.data().url; b.style.display = "block";
        }
    });
    function afisGuncelle() { if(rol === "kurucu") db.collection("ayarlar").doc("afis").set({url: document.getElementById('afis-url').value}); }

    // ÜYE SİSTEMİ
    db.collection("ekip").onSnapshot(s => {
        document.getElementById('count-uye').innerText = s.size;
        document.getElementById('list-ekip').innerHTML = s.docs.map(d => `
            <div class="card">
                <span class="badge badge-${d.data().rol}">${d.data().rol}</span><br>
                <b>${d.data().ad}</b><br>
                <button class="del-btn" onclick="sil('ekip','${d.id}')">KADRODAN SİL</button>
            </div>
        `).join('');
    });
    function ekipEkle() { if(rol === "kurucu") db.collection("ekip").add({ad: document.getElementById('e-ad').value, rol: document.getElementById('e-rol').value}); }

    // İÇERİK SİSTEMİ
    db.collection("icerik").onSnapshot(s => {
        const data = s.docs.map(d => ({id: d.id, ...d.data()}));
        const render = (tip, id) => {
            document.getElementById(id).innerHTML = data.filter(i => i.tip === tip).map(i => `
                <div class="card">${i.url.includes('http') ? `<img src="${i.url}">` : ''}<br><b>${i.i_baslik}</b><br>
                <button class="del-btn" onclick="sil('icerik','${i.id}')">İÇERİĞİ SİL</button></div>
            `).join('');
        };
        render('galeri','list-galeri'); render('video','list-video'); render('mod','list-mod');
        render('kaplama','list-kaplama'); render('muzik','list-muzik'); render('bg','list-bg');
    });
    function icerikEkle() { if(rol !== "uye") db.collection("icerik").add({tip: document.getElementById('i-tip').value, i_baslik: document.getElementById('i-baslik').value, url: document.getElementById('i-url').value}); }

    // SOHBET
    db.collection("sohbet").orderBy("tarih", "desc").limit(30).onSnapshot(s => {
        document.getElementById('chat-list').innerHTML = s.docs.map(d => `<div><b style="color:var(--gold)">${d.data().kim}:</b> ${d.data().msg}</div>`).reverse().join('');
    });
    function msgGonder() {
        let m = document.getElementById('chat-in').value;
        if(m) { db.collection("sohbet").add({kim: rol, msg: m, tarih: Date.now()}); document.getElementById('chat-in').value = ""; }
    }

    // SİLME YETKİSİ
    function sil(c, id) {
        if(rol === "kurucu") {
            if(confirm("Silmek istediğine emin misin? (Sadece Kurucu Silebilir)")) db.collection(c).doc(id).delete();
        } else { alert("YETKİ YOK: Silme işlemi sadece Kurucuya aittir!"); }
    }

    // OYUN
    function initGame() {
        const c = document.getElementById('gameCanvas'); const ctx = c.getContext('2d');
        let x = 50, y = 130;
        function draw() {
            ctx.clearRect(0,0,600,300);
            ctx.fillStyle = "red"; ctx.fillRect(x, y, 60, 30);
            ctx.fillStyle = "white"; ctx.fillText("AKKUŞLAR", x+5, y+18);
        }
        document.onkeydown = (e) => {
            if(e.key === "ArrowRight") x += 10; if(e.key === "ArrowLeft") x -= 10;
            if(e.key === "ArrowUp") y -= 10; if(e.key === "ArrowDown") y += 10;
            draw();
        };
        draw();
    }
</script>
</body>
</html>
