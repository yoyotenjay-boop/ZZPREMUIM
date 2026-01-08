<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YARGA MOTO | Cyber-Bogandé 2026</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #00f2ff; 
            --secondary: #ff00c8; 
            --bg-light: #121420; 
            --card-bg: rgba(255, 255, 255, 0.07);
            --accent: #00ff88; 
            --neon-shadow: 0 0 20px rgba(0, 242, 255, 0.5);
        }
        
        body { margin: 0; background: var(--bg-light); color: #e0e0e0; font-family: 'Poppins', sans-serif; overflow-x: hidden; }
        header { background: rgba(18, 20, 32, 0.85); backdrop-filter: blur(15px); border-bottom: 2px solid var(--primary); position: sticky; top: 0; z-index: 1000; padding: 15px; box-shadow: var(--neon-shadow); }
        .header-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
        .logo-area h1 { margin: 0; font-family: 'Orbitron', sans-serif; color: #fff; font-size: 3.5em; letter-spacing: 3px; text-shadow: 0 0 8px var(--primary); }
        
        .logo-area h5 { margin: 0; font-family: 'Orbitron', sans-serif; color: #fff; font-size: 1.4em; letter-spacing: 3px; text-shadow: 0 0 8px var(--primary); }
        .btn-top { background: rgba(0, 242, 255, 0.1); border: 1px solid var(--primary); color: var(--primary); padding: 8px 12px; border-radius: 10px; cursor: pointer; font-weight: bold; transition: 0.3s; }
        
        #notif-badge { position: absolute; top: -5px; right: -5px; background: var(--secondary); color: white; font-size: 10px; width: 18px; height: 18px; border-radius: 50%; display: none; align-items: center; justify-content: center; font-weight: bold; }

        .search-zone { width: 100%; margin-top: 10px; }
        #search-input { width: 100%; padding: 14px; border-radius: 12px; border: 1px solid rgba(0, 242, 255, 0.3); background: rgba(255,255,255,0.05); color: #fff; box-sizing: border-box; outline: none; }

        .full-panel { position: fixed; bottom: -100%; left: 0; width: 100%; height: 92vh; background: rgba(15, 17, 26, 0.98); backdrop-filter: blur(25px); z-index: 4000; transition: 0.5s; padding: 25px; box-sizing: border-box; border-top: 2px solid var(--primary); border-radius: 30px 30px 0 0; overflow-y: auto; }
        .full-panel.open { bottom: 0; }
        
        #side-menu { position: fixed; top: 0; right: -100%; width: 85%; height: 100%; background: rgba(18, 20, 32, 0.98); backdrop-filter: blur(20px); z-index: 2000; transition: 0.4s; padding: 25px; box-sizing: border-box; border-left: 2px solid var(--primary); }
        #side-menu.open { right: 0; }
        .close-menu { text-align: right; font-size: 2em; color: var(--secondary); margin-bottom: 10px; cursor: pointer; }

        .opt-btn { display: block; width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid transparent; color: #888; text-align: left; margin-bottom: 8px; border-radius: 10px; font-size: 0.85em; transition: 0.3s; font-family: 'Orbitron'; }
        .opt-btn.active { border-color: var(--primary); color: #fff; background: rgba(0, 242, 255, 0.15); }

        .main-terrain { padding: 15px; display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .piece-card { background: var(--card-bg); border-radius: 20px; overflow: hidden; border: 1px solid rgba(255,255,255,0.05); transition: 0.3s; }
        .piece-img { width: 100%; height: 160px; object-fit: cover; }
        .piece-name { padding: 12px; font-size: 0.8em; text-align: center; color: #fff; font-weight: 600; text-transform: uppercase; }

        #admin-panel { display: none; background: #000; padding: 20px; border-radius: 20px; border: 2px dashed var(--secondary); margin: 15px; }
        #admin-panel input, #admin-panel select, #admin-panel textarea { width: 100%; padding: 12px; margin-bottom: 10px; background: #111; border: 1px solid #333; color: #fff; border-radius: 8px; box-sizing: border-box; }
        .admin-list-item { display: flex; justify-content: space-between; padding: 10px; border-bottom: 1px solid #222; font-size: 0.8em; }
        .btn-del { color: var(--secondary); font-weight: bold; border: none; background: none; }
        
        #modal-detail { position: fixed; inset: 0; background: rgba(0,0,0,0.9); z-index: 5000; display: none; align-items: center; justify-content: center; }
        .icon-circle { width: 55px; height: 55px; border-radius: 15px; border: 1px solid; display: flex; align-items: center; justify-content: center; text-decoration: none; }
        
        .status-box { padding: 10px; border-radius: 10px; text-align: center; font-family: 'Orbitron'; font-size: 0.7em; margin-bottom: 20px; border: 1px solid rgba(255,255,255,0.1); }
        .contact-row { display: flex; align-items: center; gap: 10px; margin-top: 15px; font-size: 0.9em; color: var(--primary); }
    </style>
</head>
<body>

<header>
    <div class="header-top">
        <button class="btn-top" id="loc-btn">🛰️</button>
        <div class="logo-area"><h1>𝕭𝖆𝖓𝖙𝖎𝖆 </h1>
        <h5>𝔼.𝐂𝑶𝑴𝑴𝑬𝑹𝑪𝑬</h5></div>
        <div style="display: flex; gap: 8px;">
            <button class="btn-top" onclick="toggleNotif(true)" style="position:relative;">🔔 <span id="notif-badge"></span></button>
            <button class="btn-top" onclick="toggleMenu(true)">☰</button>
        </div>
    </div>
    <div class="search-zone">
        <input type="text" id="search-input" placeholder="Recherche du stock..." onkeyup="filtrer();">
    </div>
</header>

<div id="admin-panel">
    <h2 style="color:var(--secondary); font-family:'Orbitron'; font-size:1em;">🛠️ CONSOLE ZZ</h2>
    <input type="text" id="add-nom" placeholder="Nom de la pièce">
    <input type="number" id="add-prix" placeholder="Prix (FCFA)">
    <select id="add-moto">
        <option value="SIRIUS">SIRIUS</option>
        <option value="SANILI">SANILI</option>
        <option value="SANYA">SANYA</option>
        <option value="HAOJUE">HAOJUE</option>
        <option value="WINNER">WINNER</option>
        <option value="AUTRES">AUTRES</option>
    </select>
    <input type="text" id="add-img-url" placeholder="Lien (URL) de l'image">
    <button onclick="uploadProduct()" id="btn-pub" style="width:100%; background:var(--primary); padding:15px; border:none; border-radius:10px; font-weight:bold; margin-bottom:20px;">PUBLIER </button>

    <h2 style="color:var(--accent); font-family:'Orbitron'; font-size:0.8em;">📢 ANNONCE</h2>
    <textarea id="notif-text" placeholder="Message Flux Sortant..."></textarea>
    <div style="display:grid; grid-template-columns: 1fr 1fr; gap:10px;">
        <button onclick="updateAnnonce()" style="background:var(--accent); padding:10px; border:none; border-radius:10px; font-weight:bold;">ENVOYER</button>
        <button onclick="deleteAnnonce()" style="background:var(--secondary); padding:10px; border:none; border-radius:10px; font-weight:bold;">SUPPRIMER</button>
    </div>

    <h2 style="color:var(--secondary); font-family:'Orbitron'; font-size:0.8em; margin-top:20px;">🗑️ SUPPRIMER DES ARTICLES</h2>
    <div id="admin-delete-list"></div>
</div>

<div id="panel-loc" class="full-panel">
    <div class="close-menu" onclick="toggleLoc(false)">✕</div>
    <h2 style="color:var(--primary); font-family: 'Orbitron';"> 𝔹𝕆𝔾𝔸ℕ𝔻𝔼-𝔹𝕌ℝ𝕂𝕀ℕ𝔸 𝔽𝔸𝕊𝕆</h2>
    <div style="border-radius:20px; overflow:hidden; border:1px solid var(--primary); height: 250px;">
        <iframe width="100%" height="100%" src="https://maps.google.com/maps?q=Bogande,Burkina%20Faso&t=&z=13&ie=UTF8&iwloc=&output=embed" frameborder="0" style="filter: invert(90%) hue-rotate(180deg) contrast(1.2);"></iframe>
    </div>
    <div class="contact-row">📞 <span>+226 74 96 30 87</span></div>
    <div class="contact-row">✉️ <span>bantiaecommerce@gmail.com</span></div>
</div>

<div id="panel-notif" class="full-panel">
    <div class="close-menu" onclick="toggleNotif(false)">✕</div>
    <h2 style="color:var(--secondary); font-family: 'Orbitron';">🛰️ FLUX </h2>
    <div id="msg-client" style="padding:20px; border-left:4px solid var(--secondary); background:rgba(255,0,200,0.05); border-radius:10px; white-space: pre-wrap;">Aucun message en cours...</div>
</div>

<div id="side-menu">
    <div class="close-menu" onclick="toggleMenu(false)">✕</div>
    
    <div id="shop-status" class="status-box">VÉRIFICATION DU RÉSEAU...</div>

    <div class="menu-section">
        <span style="color:var(--primary); font-family:'Orbitron'; font-size:0.7em; letter-spacing:2px;">𝑴𝑶𝑫𝑬𝑳 MOTO</span>
        <div style="margin-top:15px;">
            <button class="opt-btn active m-btn" onclick="setM('SIRIUS', this)"> MODÈLE SIRIUS</button>
            <button class="opt-btn m-btn" onclick="setM('SANILI', this)">SANILI </button>
            <button class="opt-btn m-btn" onclick="setM('SANYA', this)">SANYA TECH</button>
            <button class="opt-btn m-btn" onclick="setM('HAOJUE', this)">MODÈLE HAOJUE </button>
            <button class="opt-btn m-btn" onclick="setM('WINNER', this)">WINNER X</button>
            <button class="opt-btn m-btn" onclick="setM('AUTRE', this)">AUTRES...</button>
        </div>
    </div>
</div>

<div class="main-terrain" id="product-list"></div>

<div id="modal-detail" onclick="this.style.display='none'">
    <div class="modal-box" onclick="event.stopPropagation()" style="background: #1a1d2e; border: 1px solid var(--primary); border-radius: 30px; padding: 20px; width: 85%;">
        <img id="m-img" style="width:100%; border-radius:20px;">
        <h3 id="m-title" style="margin-top:20px; font-family: 'Orbitron'; color: #fff;"></h3>
        <div style="font-size: 1.8em; color: var(--primary); font-family: 'Orbitron';" id="m-price"></div>
        <div style="display:flex; justify-content:center; gap:20px; margin-top:20px;">
            <a id="m-wa" class="icon-circle" style="border-color:var(--accent); color:var(--accent);">W/A</a>
            <a href="tel:+226654091" class="icon-circle" style="border-color:var(--primary); color:var(--primary);">📞</a>
        </div>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
    import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp, deleteDoc, doc, setDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

    const firebaseConfig = {
        apiKey: "AIzaSyCrDehL-WzJP0jrw-i6iOTIXMNl9nrXOMI",
        authDomain: "bantiaapp.firebaseapp.com",
        projectId: "bantiaapp",
        storageBucket: "bantiaapp.firebasestorage.app",
        appId: "1:1017776145922:web:baa915ce6bf6196fc9c91b"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);

    let allP = [];
    let selM = 'SIRIUS';
    let pressTimer;

    function checkStatus() {
        const hour = new Date().getHours();
        const statusBox = document.getElementById('shop-status');
        if(hour >= 8 && hour < 18) {
            statusBox.innerHTML = "𝐁𝐎𝐔𝐓𝐈𝐐𝐔𝐄: <span style='color:var(--accent)'>OUVERT</span> (-18H)";
            statusBox.style.borderColor = "var(--accent)";
        } else {
            statusBox.innerHTML = "𝐁𝐎𝐔𝐓𝐈𝐐𝐔𝐄: <span style='color:var(--secondary)'>FERMÉ</span> (OUVRE À 08H)";
            statusBox.style.borderColor = "var(--secondary)";
        }
    }
    checkStatus();
    setInterval(checkStatus, 60000);

    const locBtn = document.getElementById('loc-btn');
    locBtn.addEventListener('touchstart', () => {
        pressTimer = setTimeout(() => {
            if(prompt("CODE?") === "654091") document.getElementById('admin-panel').style.display='block';
        }, 3000);
    });
    locBtn.addEventListener('touchend', () => {
        clearTimeout(pressTimer);
        if(document.getElementById('admin-panel').style.display !== 'block') toggleLoc(true);
    });

    window.uploadProduct = async () => {
        const btn = document.getElementById('btn-pub');
        const url = document.getElementById('add-img-url').value;
        const nom = document.getElementById('add-nom').value;
        const prix = document.getElementById('add-prix').value;
        const moto = document.getElementById('add-moto').value;

        if(!url || !nom || !prix) return alert("Remplis tout !");
        
        btn.disabled = true;
        btn.innerText = "PUBLICATION...";

        try {
            await addDoc(collection(db, "yarga_v5"), {
                nom: nom, prix: prix, moto: moto, img: url, timestamp: serverTimestamp()
            });
            alert("UNITÉ AJOUTÉE !");
            location.reload();
        } catch(e) { alert("Erreur"); }
        btn.disabled = false;
        btn.innerText = "PUBLIER UNITÉ";
    };

    window.updateAnnonce = async () => {
        const txt = document.getElementById('notif-text').value;
        if(!txt) return;
        await setDoc(doc(db, "yarga_info", "annonce"), { texte: txt, active: true });
        alert("ANNONCE ENVOYÉE !");
    };

    window.deleteAnnonce = async () => {
        await setDoc(doc(db, "yarga_info", "annonce"), { texte: "", active: false });
        alert("ANNONCE SUPPRIMÉE !");
    };

    onSnapshot(query(collection(db, "yarga_v5"), orderBy("timestamp", "desc")), (s) => {
        allP = []; 
        const delList = document.getElementById('admin-delete-list');
        delList.innerHTML = "";
        s.forEach(d => {
            const data = {id: d.id, ...d.data()};
            allP.push(data);
            delList.innerHTML += `<div class="admin-list-item">${data.nom} <button class="btn-del" onclick="deleteItem('${d.id}')">SUPPR</button></div>`;
        });
        afficher();
    });

    onSnapshot(doc(db, "yarga_info", "annonce"), (d) => {
        const badge = document.getElementById('notif-badge');
        const msgBox = document.getElementById('msg-client');
        if(d.exists() && d.data().active && d.data().texte !== "") {
            msgBox.innerText = d.data().texte;
            badge.style.display = "flex";
            badge.innerText = "1";
        } else {
            msgBox.innerText = "Aucun message en cours...";
            badge.style.display = "none";
        }
    });

    window.deleteItem = async (id) => { if(confirm("Supprimer?")) await deleteDoc(doc(db, "yarga_v5", id)); };

    window.toggleMenu = (o) => document.getElementById('side-menu').classList.toggle('open', o);
    window.toggleLoc = (o) => document.getElementById('panel-loc').classList.toggle('open', o);
    window.toggleNotif = (o) => {
        document.getElementById('panel-notif').classList.toggle('open', o);
        if(o) document.getElementById('notif-badge').style.display = "none";
    };
    window.setM = (m, b) => { selM = m; document.querySelectorAll('.m-btn').forEach(x => x.classList.remove('active')); b.classList.add('active'); toggleMenu(false); afficher(); };

    window.afficher = () => {
        const div = document.getElementById('product-list');
        div.innerHTML = "";
        allP.filter(p => p.moto === selM).forEach(p => {
            div.innerHTML += `<div class="piece-card" onclick="ouvrirD('${p.nom}','${p.prix}','${p.img}')">
                <img src="${p.img}" class="piece-img">
                <div class="piece-name">${p.nom}</div>
            </div>`;
        });
    };

    window.ouvrirD = (n, p, i) => {
        document.getElementById('m-title').innerText = n;
        document.getElementById('m-price').innerText = p + " F";
        document.getElementById('m-img').src = i;
        document.getElementById('m-wa').href = "https://wa.me/22674963087?text=COMMANDE: " + n;
        document.getElementById('modal-detail').style.display = 'flex';
    };
</script>
</body>
</html>