<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V62</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; --admin: #00CCFF; --yonetici: #AA00FF; }
        * { box-sizing: border-box; transition: 0.3s; font-family: 'DM Sans', sans-serif; }
        h1, h2, h3, .nav-btn { font-family: 'Space Grotesk', sans-serif; }
        body { margin: 0; background: var(--bg); color: #EEE; display: flex; height: 100vh; overflow: hidden; }
        
        /* SOL NAVİGASYON */
        .sidebar { width: 280px; background: #000; border-right: 2px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-label { color: #444; font-size: 11px; font-weight: bold; text-transform: uppercase; margin: 15px 0 5px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 13px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover { background: var(--p); color: white; transform: translateX(5px); }
        .active-btn { background: var(--p) !important; color: white !important; }

        /* ANA EKRAN */
        .main { flex: 1; padding: 40px; overflow-y: auto; background: linear-gradient(135deg, #0A0A0A 0%, #1a0000 100%); }
        .panel { display: none; }
        .aktif { display: block !important; animation: fadeIn 0.5s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .card { background: var(--card); padding: 25px; border-radius: 15px; border: 1px solid #222; margin-bottom: 20px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; }
        .card img { width: 100%; border-radius: 10px; margin-bottom: 15px; border: 1px solid #333; }

        /* YETKİ KONTROLLERİ (SİHİRLİ SINIFLAR) */
        .kurucu-ozel { display: none; }
        .kurucu-mode .kurucu-ozel { display: block !important; }
        .ekle-ozel { display: none; }
        .kurucu-mode .ekle-ozel, .admin-mode .ekle-ozel, .yonetici-mode .ekle-ozel { display: block !important; }

        input, select, textarea { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 15px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 14px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; }
        .del-btn { background: #800 !important; font-size: 11px; padding: 5px !important; }
        
        .badge { padding: 4px 10px; border-radius: 5px; font-size: 11px; font-weight: bold; text-transform: uppercase; }
        .b-k { background: var(--gold); color: #000; }
        .b-a { background: var(--admin); color: #000; }
        .b-y { background: var(--yonetici); color: #fff; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0; letter-spacing:1px;">AKKUŞLAR SEYAHAT</h2>
        <div style="color:var(--gold); font-size:11px; margin-top:5px; font-weight:bold;">👑 Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</div>
    </div>
    
    <div class="nav-label">LOJİSTİK ÜS</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Üyeleri</button>
    <button class="nav-btn" onclick="sayfa(20, this)">🗺️ Konvoy Planı</button>
    
    <div class="nav-label">ATÖLYE & MEDYA</div>
    <button class="nav-btn" onclick="sayfa(8, this)">🖼️ Galeri</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Mod Merkezi</button>
    <button class="nav-btn" onclick="sayfa(9, this)">🎨 Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(11, this)">🎵 Müzik Listesi</button>
    
    <div class="nav-label">TOPLULUK</div>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Al</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katılım</button>
    <button class="nav-btn" onclick="sayfa(14, this)">📱 İletişim</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🎮 Mini Oyun</button>

    <div style="margin-top:auto; padding-top:20px;">
        <button class="nav-btn" style="color:var(--gold);" onclick="giris('kurucu')">👑 KURUCU YÖNETİM</button>
        <button class="nav-btn" style="color:var(--admin);" onclick="giris('admin')">🛡️ ADMİN GİRİŞİ</button>
        <button class="nav-btn" style="color:var(--yonetici);" onclick="giris('yonetici')">⚙️ YÖNETİCİ GİRİŞİ</button>
    </div>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <div class="card" style="background:linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url('https://images.unsplash.com/photo-1519003722824-192d992a6059?auto=format&fit=crop&q=80&w=1000'); background-size:cover; height:350px; display:flex; flex-direction:column; justify-content:center; align-items:center; border:2px solid var(--p);">
            <h1 style="font-size:45px; margin:0;">AKKUŞLAR SEYAHAT</h1>
            <p style="color:var(--gold); font-weight:bold; letter-spacing:2px;">GÜÇ, SADAKAT, YOLCULUK</p>
            <button class="action-btn" style="width:200px; margin-top:20px;" onclick="sayfa(12)">BİZE KATIL</button>
        </div>
        <div class="grid">
            <div class="card">
                <h3>💬 Ekip Sohbeti</h3>
                <div id="msg-list" style="height:250px; overflow-y:auto; background:#000; padding:15px; border-radius:10px; margin-bottom:10px; text-align:left; border:1px solid #222;"></div>
                <div style="display:flex; gap:5px;">
                    <input type="text" id="chat-nick" placeholder="Kaptan Adı..." style="width:30%;">
                    <input type="text" id="chat-input" placeholder="Mesaj..." style="width:70%;">
                    <button onclick="mesajAt()" style="background:var(--p); border:none; color:#fff; padding:0 15px; border-radius:8px; cursor:pointer;">➔</button>
                </div>
            </div>
            <div class="card">
                <h3>📢 Duyuru</h3>
                <p>Yeni konvoy rotası Berlin'den başlıyor. Tüm kaptanların tırlarını kırmızıya boyaması rica olunur.</p>
            </div>
        </div>
    </div>

    <div id="p2" class="panel">
        <h1>👥 Ekip Üyeleri</h1>
        <div id="kadro-listesi" class="grid"></div>
    </div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM PANELİ</h1>
        <div class="grid">
            <div class="card kurucu-ozel" style="border:2px solid var(--gold);">
                <h3 style="color:var(--gold);">👑 Kurucu Özel Yetki</h3>
                <p>Üye Onayla / Rol Ver / Sil</p>
                <input type="text" id="r-ad" placeholder="Kaptan Adı...">
                <select id="r-rol">
                    <option value="admin">🛡️ Admin</option>
                    <option value="yonetici">⚙️ Yönetici</option>
                    <option value="uye">👤 Üye</option>
                </select>
                <button class="action-btn" onclick="rolVer()">KAYDI TAMAMLA</button>
            </div>
            
            <div class="card ekle-ozel" style="border:2px solid var(--admin);">
                <h3>📦 İçerik Ekleme</h3>
                <select id="i-tip">
                    <option value="galeri">Resim Galerisi</option>
                    <option value="mod">ETS2 Modu</option>
                    <option value="kaplama">Kaplama (Skin)</option>
                    <option value="muzik">Müzik</option>
                </select>
                <input type="text" id="i-bas" placeholder="Başlık...">
                <input type="text" id="i-link" placeholder="İndirme Linki (Opsiyonel)">
                <input type="file" id="i-file">
                <button class="action-btn" onclick="yukle()">YÜKLE</button>
            </div>
        </div>
    </div>

    <div id="p12" class="panel">
        <h1>📩 Ekibe Katılım Başvurusu</h1>
        <div class="card" style="max-width:500px; margin:auto;">
            <input type="text" id="k-ad" placeholder="Adınız ve Oyun Adınız...">
            <input type="number" id="k-yas" placeholder="Yaşınız...">
            <textarea id="k-deneyim" placeholder="Daha önce hangi ekiplerdeydiniz?"></textarea>
            <button class="action-btn" onclick="basvuruAt()">BAŞVURUYU GÖNDER</button>
        </div>
        <div class="kurucu-ozel" style="margin-top:30px;">
            <h3>Gelen Başvurular (Sadece Sen Görürsün)</h3>
            <div id="list-basvuru"></div>
        </div>
    </div>

    <div id="p14" class="panel">
        <h1>📱 İletişim Bilgileri</h1>
        <div class="grid">
            <a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" class="card" style="text-decoration:none; color:inherit;">
                <h3>TikTok (Kurucu)</h3>
                <p>@kelebekmiisaliii</p>
            </a>
            <a href="https://www.tiktok.com/@akkusailesi20" target="_blank" class="card" style="text-decoration:none; color:inherit;">
                <h3>TikTok (Resmi Sayfa)</h3>
                <p>@akkusailesi20</p>
            </a>
        </div>
    </div>

    <div id="p8" class="panel"><h1>🖼️ Galeri</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🛠️ Mod Merkezi</h1><div id="list-mod" class="grid"></div></div>
    <div id="p9" class="panel"><h1>🎨 Kaplamalar</h1><div id="list-kaplama" class="grid"></div></div>
    <div id="p11" class="panel"><h1>🎵 Müzik Listesi</h1><div id="list-muzik" class="grid"></div></div>
    <div id="p20" class="panel"><h1>🗺️ Konvoy Planı</h1><div class="card">Sıradaki: Denizli - Prag | 21:00</div></div>
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
    let aktifRütbe = "uye";

    function giris(tip) {
        let s = prompt("Giriş Şifresi:");
        if(!s) return;
        if(tip === 'kurucu' && s === "1907akkus") {
            aktifRütbe = "kurucu";
            document.getElementById('app-body').className = 'kurucu-mode';
            sayfa(13);
        } else if(tip === 'admin' && s === "admin123") {
            aktifRütbe = "admin";
            document.getElementById('app-body').className = 'admin-mode';
            sayfa(13);
        } else if(tip === 'yonetici' && s === "yonetici123") {
            aktifRütbe = "yonetici";
            document.getElementById('app-body').className = 'yonetici-mode';
            sayfa(13);
        } else { alert("Yetki Reddedildi!"); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
    }

    // SOHBET (KAPTAN YAZISI KALDIRILDI)
    function mesajAt() {
        let n = document.getElementById('chat-nick').value;
        let m = document.getElementById('chat-input').value;
        if(n && m) { db.collection("sohbet").add({isim: n, msg: m, tarih: Date.now()}); document.getElementById('chat-input').value=""; }
    }
    db.collection("sohbet").orderBy("tarih","desc").limit(20).onSnapshot(s => {
        document.getElementById('msg-list').innerHTML = s.docs.map(d => `<div><b style="color:var(--p)">${d.data().isim}:</b> ${d.data().msg}</div>`).reverse().join('');
    });

    // İÇERİK YÜKLEME (ADMIN/YÖNETİCİ/KURUCU)
    function yukle() {
        const file = document.getElementById('i-file').files[0];
        const tip = document.getElementById('i-tip').value;
        if(!file) return alert("Dosya seçmelisin!");
        const ref = storage.ref(`akkuslar/${Date.now()}_${file.name}`);
        ref.put(file).then(s => s.ref.getDownloadURL()).then(url => {
            db.collection("icerik").add({
                tip: tip,
                bas: document.getElementById('i-bas').value,
                link: document.getElementById('i-link').value || "",
                url: url,
                tarih: Date.now()
            });
            alert("Sisteme eklendi!");
        });
    }

    // İÇERİK LİSTELEME
    db.collection("icerik").orderBy("tarih","desc").onSnapshot(s => {
        let g="", m="", k="", mu="";
        s.docs.forEach(d => {
            let data = d.data();
            let silButon = aktifRütbe === 'kurucu' ? `<button class="action-btn del-btn" onclick="sil('icerik','${d.id}')">SİL</button>` : '';
            let html = `<div class="card"><img src="${data.url}"><b>${data.bas}</b>${data.link ? `<br><a href="${data.link}" target="_blank" style="color:var(--gold)">İndir</a>`:''}${silButon}</div>`;
            if(data.tip === "galeri") g += html; else if(data.tip === "mod") m += html; else if(data.tip === "kaplama") k += html; else mu += html;
        });
        document.getElementById('list-galeri').innerHTML = g;
        document.getElementById('list-mod').innerHTML = m;
        document.getElementById('list-kaplama').innerHTML = k;
        document.getElementById('list-muzik').innerHTML = mu;
    });

    // KADRO VE ROL VERME (SADECE KURUCU)
    function rolVer() {
        if(aktifRütbe !== 'kurucu') return;
        db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value});
        alert("Üye sisteme tanımlandı.");
    }

    db.collection("ekip").onSnapshot(s => {
        document.getElementById('kadro-listesi').innerHTML = s.docs.map(d => {
            let data = d.data();
            let rb = data.rol === 'admin' ? 'b-a' : (data.rol === 'yonetici' ? 'b-y' : '');
            let sil = aktifRütbe === 'kurucu' ? `<button class="action-btn del-btn" onclick="sil('ekip','${d.id}')">KADRODAN AT</button>` : '';
            return `<div class="card"><span class="badge ${rb}">${data.rol}</span><br><b>${data.ad}</b><br>${sil}</div>`;
        }).join('');
    });

    // BAŞVURU SİSTEMİ
    function basvuruAt() {
        db.collection("basvurular").add({
            ad: document.getElementById('k-ad').value,
            yas: document.getElementById('k-yas').value,
            not: document.getElementById('k-deneyim').value,
            tarih: Date.now()
        });
        alert("Başvurunuz kurucuya iletildi!");
    }
    db.collection("basvurular").onSnapshot(s => {
        if(aktifRütbe === 'kurucu') {
            document.getElementById('list-basvuru').innerHTML = s.docs.map(d => `<div class="card"><b>${d.data().ad} (Yaş: ${d.data().yas})</b><br>${d.data().not}<br><button class="action-btn del-btn" onclick="sil('basvurular','${d.id}')">OKUNDU/SİL</button></div>`).join('');
        }
    });

    function sil(c, id) { if(aktifRütbe === 'kurucu' && confirm("Kesin olarak silinsin mi?")) db.collection(c).doc(id).delete(); }
</script>
</body>
</html>
