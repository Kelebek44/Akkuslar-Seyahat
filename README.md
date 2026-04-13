<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V79</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@700&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; --admin: #00CCFF; --yon: #AA00FF; }
        * { box-sizing: border-box; transition: 0.3s; font-family: 'DM Sans', sans-serif; }
        body { margin: 0; background: var(--bg); color: #EEE; display: flex; height: 100vh; overflow: hidden; }
        .sidebar { width: 280px; background: #000; border-right: 2px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 5px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 11px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover, .active-btn { background: var(--p); color: white; transform: translateX(5px); }
        .main { flex: 1; padding: 40px; overflow-y: auto; background: radial-gradient(circle at top, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #222; margin-bottom: 20px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 15px; }
        .badge { padding: 4px 10px; border-radius: 5px; font-size: 10px; font-weight: bold; display: flex; align-items: center; gap: 5px; justify-content: center; }
        .b-kurucu { background: var(--gold); color: #000; }
        .b-admin { background: var(--admin); color: #000; }
        .b-yonetici { background: var(--yon); color: #fff; }
        .kurucu-only, .admin-only, .yonetici-only, .editor-only { display: none; }
        .kurucu-mode .kurucu-only, .kurucu-mode .editor-only { display: block !important; }
        .admin-mode .admin-only, .admin-mode .editor-only { display: block !important; }
        .yonetici-mode .yonetici-only, .yonetici-mode .editor-only { display: block !important; }
        .del-btn { background: #800; color: white; border: none; padding: 8px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; width: 100%; }
        .kurucu-mode .del-btn { display: block !important; }
        input, select, textarea { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 15px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR SEYAHAT</h2>
        <div style="color:var(--gold); font-size:11px; margin-top:5px;">👑 Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</div>
    </div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Terminal Ana Ekran</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👤 Ekip Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(4, this)">📜 Ekip Kuralları</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🖼️ Galeri / Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar / Müzikler</button>
    <button class="nav-btn" onclick="sayfa(12, this)">🛡️ Yönetim Telsizi</button>
    <button class="nav-btn" onclick="sayfa(5, this)">✍️ Ekibe Katıl</button>
    <button class="nav-btn" onclick="sayfa(15, this)">📞 İletişim</button>
    <div style="margin-top:auto; padding-top:20px; border-top:1px solid #222;">
        <button class="nav-btn" style="color:var(--gold);" onclick="giris('kurucu')">👑 KURUCU GİRİŞ</button>
        <button class="nav-btn" style="color:var(--admin);" onclick="giris('admin')">🛡️ ADMİN GİRİŞ</button>
        <button class="nav-btn" style="color:var(--yon);" onclick="giris('yonetici')">⚙️ YÖNETİCİ GİRİŞ</button>
    </div>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <div class="grid">
            <div class="card" style="text-align:center;"><h3>Normal Üye</h3><h1 id="count-uye" style="color:var(--p)">0</h1></div>
            <div class="card" style="text-align:center;"><h3>Yönetim Kadrosu</h3><h1 id="count-yon" style="color:var(--gold)">0</h1></div>
        </div>
        <div class="card">
            <h3>📢 Ekip Telsizi (Üye Sohbet)</h3>
            <div id="msg-list-uye" style="height:250px; overflow-y:auto; background:#000; padding:15px; border-radius:10px; border:1px solid #222;"></div>
            <div style="display:flex; gap:10px; margin-top:15px;">
                <input type="text" id="nick-uye" placeholder="İsim" style="width:20%;">
                <input type="text" id="msg-uye" placeholder="Mesaj..." style="width:80%;">
                <button onclick="mesajGonder('sohbet_uye', 'nick-uye', 'msg-uye')" class="action-btn" style="width:100px;">GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p12" class="panel">
        <h1>🛡️ Yönetim Özel Telsiz</h1>
        <div class="card">
            <div id="msg-list-yon" style="height:350px; overflow-y:auto; background:#000; padding:15px; border-radius:10px; border:2px solid var(--gold);"></div>
            <div style="display:flex; gap:10px; margin-top:15px;">
                <input type="text" id="nick-yon" placeholder="Yönetici İsmi" style="width:20%;">
                <input type="text" id="msg-yon" placeholder="Özel Mesaj..." style="width:80%;">
                <button onclick="mesajGonder('sohbet_yon', 'nick-yon', 'msg-yon')" class="action-btn" style="background:var(--gold); color:black; width:100px;">GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p2" class="panel"><h1>👤 Ekip Kadrosu</h1><div id="kadro-listesi" class="grid"></div></div>
    <div id="p4" class="panel"><h1>📜 Ekip Kuralları</h1><div class="card">1. Kurucuya mutlak sadakat.<br>2. Küfür ve saygısızlık ihraç sebebidir.<br>3. Konvoy disiplinine uyulur.</div></div>
    
    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div class="card editor-only">
                <h3>DOSYA YÜKLE</h3>
                <select id="i-tip">
                    <option value="galeri">Galeri / Kaplama</option>
                    <option value="mod">Mod Merkezi</option>
                    <option value="muzik">Müzik Arşivi</option>
                </select>
                <input type="text" id="i-bas" placeholder="Dosya Başlığı">
                <input type="file" id="i-file">
                <button class="action-btn" onclick="yukle()">YÜKLEMEYİ BAŞLAT</button>
            </div>
            <div class="card admin-only yonetici-only kurucu-only"><h3>📥 ÜYE ONAYLARI</h3><div id="list-basvuru"></div></div>
            <div class="card kurucu-only" style="border:2px solid var(--gold);">
                <h3>👑 ROL YÖNETİMİ</h3>
                <input type="text" id="r-ad" placeholder="İsim">
                <select id="r-rol"><option value="admin">Admin</option><option value="yonetici">Yönetici</option><option value="uye">Üye</option></select>
                <button class="action-btn" onclick="rolVer()">SİSTEME İŞLE</button>
            </div>
        </div>
    </div>

    <div id="p8" class="panel"><h1>🖼️ Galeri & Kaplamalar</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🛠️ Modlar & Müzikler</h1><div id="list-mod" class="grid"></div></div>
    <div id="p15" class="panel">
        <h1>📞 İletişim</h1>
        <div class="card">
            <h3>Tiktok Hesaplarımız</h3>
            <p><a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" style="color:var(--p)">@kelebekmiisaliii</a></p>
            <p><a href="https://www.tiktok.com/@akkusailesi20" target="_blank" style="color:var(--p)">@akkusailesi20</a></p>
        </div>
    </div>
    <div id="p5" class="panel"><h1>✍️ Ekibe Katıl</h1><div class="card"><input type="text" id="k-ad" placeholder="Ad Soyad..."><button onclick="basvur()" class="action-btn">BAŞVURU YAP</button></div></div>
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
        if(tip==='kurucu' && s==='1907akkus') { aktifRol="kurucu"; document.getElementById('app-body').className='kurucu-mode'; sayfa(13); }
        else if(tip==='admin' && s==='admin123') { aktifRol="admin"; document.getElementById('app-body').className='admin-mode'; sayfa(13); }
        else if(tip==='yonetici' && s==='yonetici123') { aktifRol="yonetici"; document.getElementById('app-body').className='yonetici-mode'; sayfa(13); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p'+n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
    }

    // ANLIK VERİ MOTORU
    db.collection("ekip").onSnapshot(s => {
        let u=0, y=0;
        document.getElementById('kadro-listesi').innerHTML = s.docs.map(d => {
            const dt = d.data();
            if(dt.rol==='uye') u++; else y++;
            const ikon = dt.rol==='admin' ? '🛡️' : (dt.rol==='yonetici' ? '⚙️' : '👤');
            return `<div class="card"><span class="badge b-${dt.rol}">${ikon} ${dt.rol}</span><br><b>${dt.ad}</b><br><button class="del-btn" onclick="sil('ekip','${d.id}')">KADRODAN AT</button></div>`;
        }).join('');
        document.getElementById('count-uye').innerText = u;
        document.getElementById('count-yon').innerText = y;
    });

    db.collection("icerik").orderBy("tarih","desc").onSnapshot(s => {
        let g="", m="";
        s.docs.forEach(d => {
            const dt = d.data();
            const h = `<div class="card">${dt.tip==='muzik'?'🎵':`<img src="${dt.url}" style="width:100%; border-radius:8px;">`} <br><b>${dt.bas}</b><br><button class="del-btn" onclick="sil('icerik','${d.id}')">SİL</button></div>`;
            if(dt.tip==="galeri") g+=h; else m+=h;
        });
        document.getElementById('list-galeri').innerHTML = g;
        document.getElementById('list-mod').innerHTML = m;
    });

    async function yukle() {
        const file = document.getElementById('i-file').files[0];
        const tip = document.getElementById('i-tip').value;
        const bas = document.getElementById('i-bas').value;
        if(!file || !bas) return alert("Eksik bilgi!");
        const ref = storage.ref(`depo/${Date.now()}_${file.name}`);
        await ref.put(file);
        const url = await ref.getDownloadURL();
        await db.collection("icerik").add({tip, bas, url, tarih: Date.now()});
        alert("Yüklendi!");
    }

    function mesajGonder(col, nickId, msgId) {
        const n = document.getElementById(nickId).value;
        const m = document.getElementById(msgId).value;
        if(n && m) db.collection(col).add({isim: n, msg: m, tarih: Date.now()});
        document.getElementById(msgId).value = "";
    }

    db.collection("sohbet_uye").orderBy("tarih","desc").limit(15).onSnapshot(s => { document.getElementById('msg-list-uye').innerHTML = s.docs.map(d => `<div><b>${d.data().isim}:</b> ${d.data().msg}</div>`).reverse().join(''); });
    db.collection("sohbet_yon").orderBy("tarih","desc").limit(15).onSnapshot(s => { document.getElementById('msg-list-yon').innerHTML = s.docs.map(d => `<div style="color:var(--gold)"><b>[YÖNETİM] ${d.data().isim}:</b> ${d.data().msg}</div>`).reverse().join(''); });
    
    function rolVer() { if(aktifRol==='kurucu') db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value}); }
    function basvur() { db.collection("basvurular").add({ad: document.getElementById('k-ad').value}); alert("Başvuru İletildi!"); }
    db.collection("basvurular").onSnapshot(s => { if(aktifRol!=='uye') document.getElementById('list-basvuru').innerHTML = s.docs.map(d => `<div class="card">👤 ${d.data().ad} <button class="action-btn" style="width:auto; padding:5px 10px;" onclick="sil('basvurular','${d.id}')">ONAYLA/SİL</button></div>`).join(''); });
    function sil(c, id) { if(confirm("Silinsin mi?")) db.collection(c).doc(id).delete(); }
</script>
</body>
</html>
