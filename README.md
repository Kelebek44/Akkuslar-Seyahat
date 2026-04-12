<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V59</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; --admin: #00CCFF; --yonetici: #AA00FF; }
        * { box-sizing: border-box; transition: 0.2s; outline: none; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'Space Grotesk', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-label { color: #555; font-size: 10px; font-weight: bold; text-transform: uppercase; margin: 15px 0 5px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 13px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover { background: var(--p); color: white; }
        .active-btn { background: var(--p) !important; color: white !important; }

        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .hero-banner { width: 100%; height: 400px; object-fit: cover; border-radius: 20px; border: 4px solid var(--p); margin-bottom: 20px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; text-align: center; position: relative; }
        .card img { width: 100%; height: 180px; object-fit: cover; border-radius: 10px; margin-bottom: 10px; border: 1px solid #444; }

        .badge { padding: 4px 10px; border-radius: 5px; font-size: 11px; font-weight: bold; text-transform: uppercase; margin-bottom: 10px; display: inline-flex; align-items: center; gap: 5px; }
        .badge-kurucu { background: var(--gold); color: #000; }
        .badge-admin { background: var(--admin); color: #000; }
        .badge-yonetici { background: var(--yonetici); color: #fff; }
        .badge-uye { background: #444; color: #fff; }

        .del-btn { background: #600; color: white; border: none; padding: 8px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; width: 100%; font-weight: bold; }
        .kurucu-modu .del-btn, .admin-modu .del-btn { display: block !important; }

        input, select { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR</h2>
        <span style="color:var(--gold); font-size:12px;">👑 Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    
    <div class="nav-label">TERMİNAL</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Kadro</button>
    <button class="nav-btn" onclick="sayfa(15, this)">📜 Kurallar</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🖼️ Galeri & Modlar</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet İşlemleri</button>
    
    <div class="nav-label">YÖNETİM</div>
    <button class="nav-btn" style="color:var(--gold);" onclick="giris('kurucu')">👑 KURUCU GİRİŞİ</button>
    <button class="nav-btn" style="color:var(--admin);" onclick="giris('admin')">🛡️ ADMİN GİRİŞİ</button>
    <button class="nav-btn" style="color:var(--yonetici);" onclick="giris('yonetici')">⚙️ YÖNETİCİ GİRİŞİ</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <img id="main-afis" class="hero-banner" style="display:none;">
        <h1 style="color:var(--p);">AKKUŞLAR SEYAHAT</h1>
        <div class="card">
            <h3>💬 Üye Sohbet Telsizi</h3>
            <div id="msg-list" style="height:250px; overflow-y:auto; background:#000; padding:15px; border-radius:10px; margin-bottom:10px; text-align:left;"></div>
            <div style="display:flex; gap:5px;">
                <input type="text" id="chat-input" placeholder="Mesajınız...">
                <button onclick="mesajAt()" style="background:var(--p); border:none; color:#fff; padding:0 20px; border-radius:8px; cursor:pointer;">GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p2" class="panel">
        <h1>👥 Ekip Kadrosu</h1>
        <h3 style="color:var(--gold);">🛡️ YÖNETİM</h3>
        <div id="kadro-yonetim" class="grid" style="margin-bottom:30px;"></div>
        <h3 style="color:var(--p);">👤 KAPTANLAR</h3>
        <div id="kadro-uyeler" class="grid"></div>
    </div>

    <div id="p8" class="panel">
        <h1>🖼️ Galeri & Modlar</h1>
        <div id="list-icerik" class="grid"></div>
    </div>

    <div id="p3" class="panel">
        <h1>🎫 Bilet Sistemi</h1>
        <div class="grid">
            <div class="card">
                <h3>Bilet Al</h3>
                <input type="text" id="b-ad" placeholder="Ad Soyad...">
                <button class="action-btn" onclick="biletAt()">BİLETİ ONAYLA</button>
            </div>
            <div class="card">
                <h3>Aktif Biletler & İptal</h3>
                <p style="font-size:11px; color:#666;">İptal yetkisi Admin/Kurucuya aittir.</p>
                <div id="list-bilet"></div>
            </div>
        </div>
    </div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div id="pan-kurucu" class="card" style="display:none; border:2px solid var(--gold);">
                <span class="badge badge-kurucu">KURUCU</span>
                <h3>Rol Ver</h3>
                <input type="text" id="r-ad" placeholder="Kaptan Adı...">
                <select id="r-rol"><option value="admin">Admin</option><option value="yonetici">Yönetici</option><option value="uye">Üye</option></select>
                <button class="action-btn" onclick="rolVer()">ONAYLA</button>
                <hr style="opacity:0.1; margin:15px 0;">
                <input type="file" id="afis-file"><button class="action-btn" onclick="yukle('afis')">AFİŞ DEĞİŞTİR</button>
            </div>
            <div id="pan-editor" class="card" style="display:none; border:2px solid var(--admin);">
                <h3>Mod/Resim Yükle</h3>
                <input type="text" id="i-bas" placeholder="Başlık...">
                <input type="file" id="i-file"><button class="action-btn" onclick="yukle('icerik')">SİSTEME GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p15" class="panel"><h1>📜 Kurallar</h1><div class="card">1. Kurucu kararı kesindir.<br>2. Saygısızlık ihraç sebebidir.</div></div>
</div>

<script>
    const firebaseConfig = {
      apiKey: "AIzaSyCowK8jtpCENPWxz0EpANeRghLXlYMVkcg",
      authDomain: "akkuslar-seyahat-f3577.firebaseapp.com",
      projectId: "akkuslar-seyahat-f3577",
      storageBucket: "akkuslar-seyahat-f3577.appspot.com",
      messagingSenderId: "207828517432",
      appId: "1:207828517432:web:dedb92a79fe1d17aec9ccd"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    const storage = firebase.storage();
    let aktifRol = "uye";

    function giris(tip) {
        let s = prompt("Şifre:");
        if(!s) return;
        if(tip === 'kurucu' && s === "1907akkus") {
            aktifRol = "kurucu";
            document.getElementById('app-body').className = 'kurucu-modu';
            document.getElementById('pan-kurucu').style.display = "block";
            document.getElementById('pan-editor').style.display = "block";
            sayfa(13);
        } else if(tip === 'admin' && s === "admin123") {
            aktifRol = "admin";
            document.getElementById('app-body').className = 'admin-modu';
            document.getElementById('pan-editor').style.display = "block";
            document.getElementById('pan-kurucu').style.display = "none";
            sayfa(13);
        } else if(tip === 'yonetici' && s === "yonetici123") {
            aktifRol = "yonetici";
            document.getElementById('pan-editor').style.display = "block";
            document.getElementById('pan-kurucu').style.display = "none";
            sayfa(13);
        } else { alert("Hatalı!"); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
    }

    // KADRO ÇEKME
    db.collection("ekip").onSnapshot(s => {
        let yHtml = "", uHtml = "";
        s.docs.forEach(d => {
            let data = d.data();
            let i = data.rol === "admin" ? "🛡️" : (data.rol === "yonetici" ? "⚙️" : "👤");
            let c = data.rol === "admin" ? "badge-admin" : (data.rol === "yonetici" ? "badge-yonetici" : "badge-uye");
            let k = `<div class="card"><span class="badge ${c}">${i} ${data.rol}</span><br><b>${data.ad}</b><br><button class="del-btn" onclick="sil('ekip','${d.id}')">SİL</button></div>`;
            if(data.rol === "uye") uHtml += k; else yHtml += k;
        });
        document.getElementById('kadro-yonetim').innerHTML = yHtml;
        document.getElementById('kadro-uyeler').innerHTML = uHtml;
    });

    // SOHBET
    function mesajAt() {
        let m = document.getElementById('chat-input').value;
        if(m) { db.collection("sohbet").add({msg: m, tarih: Date.now()}); document.getElementById('chat-input').value=""; }
    }
    db.collection("sohbet").orderBy("tarih","desc").limit(30).onSnapshot(s => {
        document.getElementById('msg-list').innerHTML = s.docs.map(d => `<div><b style="color:var(--gold)">Kaptan:</b> ${d.data().msg}</div>`).reverse().join('');
    });

    // DOSYA YÜKLEME VE GÖRÜNTÜLEME
    function yukle(tip) {
        const file = tip === 'afis' ? document.getElementById('afis-file').files[0] : document.getElementById('i-file').files[0];
        if(!file) return alert("Seç!");
        const ref = storage.ref(`depo/${Date.now()}_${file.name}`);
        ref.put(file).then(s => s.ref.getDownloadURL()).then(url => {
            if(tip === 'afis') db.collection("ayarlar").doc("afis").set({url});
            else db.collection("icerik").add({bas: document.getElementById('i-bas').value, url, tarih: Date.now()});
            alert("Yüklendi!");
        });
    }

    db.collection("icerik").orderBy("tarih","desc").onSnapshot(s => {
        document.getElementById('list-icerik').innerHTML = s.docs.map(d => `<div class="card"><img src="${d.data().url}"><b>${d.data().bas}</b><br><button class="del-btn" onclick="sil('icerik','${d.id}')">SİL</button></div>`).join('');
    });

    function rolVer() { if(aktifRol === "kurucu") db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value}); }
    function biletAt() { db.collection("biletler").add({ad: document.getElementById('b-ad').value}); }
    function sil(c, id) { if((aktifRol === "kurucu" || aktifRol === "admin") && confirm("Sil?")) db.collection(c).doc(id).delete(); }
    
    db.collection("biletler").onSnapshot(s => { document.getElementById('list-bilet').innerHTML = s.docs.map(d => `<div>🎫 ${d.data().ad} <button class="del-btn" onclick="sil('biletler','${d.id}')" style="display:inline; width:auto; padding:2px 5px;">İPTAL</button></div>`).join(''); });
    db.collection("ayarlar").doc("afis").onSnapshot(d => { if(d.exists) { document.getElementById('main-afis').src = d.data().url; document.getElementById('main-afis').style.display = "block"; }});
</script>
</body>
</html>
