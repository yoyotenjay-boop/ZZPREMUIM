
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BANTIA ELITE PREDICTIONS</title>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@700&family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --gold: #ffcc00; 
            --gold-gradient: linear-gradient(145deg, #ffcc00, #b8860b);
            --dark: #0f0f0f; 
            --card: #1a1a1a; 
            --win: #00ff88; 
        }

        body { 
            background: var(--dark); 
            color: #ffffff; 
            font-family: 'Poppins', sans-serif; 
            margin: 0; 
            padding: 0; 
            text-align: center;
        }

        header {
            background: linear-gradient(180deg, #1a1a1a 0%, var(--dark) 100%);
            padding: 40px 10px;
            border-bottom: 1px solid rgba(255, 204, 0, 0.3);
            box-shadow: 0 4px 20px rgba(0,0,0,0.8);
        }

        h1 { 
            font-family: 'Cinzel', serif; 
            color: var(--gold); 
            margin: 0; 
            font-size: 1.8em; 
            letter-spacing: 4px;
            text-shadow: 0 0 15px rgba(255, 204, 0, 0.5);
        }

        .tabs { display: flex; justify-content: center; gap: 10px; padding: 20px 0; background: #0a0a0a; }
        .tab-btn { 
            background: #222; 
            border: 1px solid #333; 
            color: #888; 
            padding: 10px 20px; 
            border-radius: 30px; 
            font-size: 0.8em; 
            cursor: pointer; 
            transition: 0.3s;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .tab-btn.active { 
            background: var(--gold-gradient); 
            color: black; 
            border-color: var(--gold);
            font-weight: bold;
            box-shadow: 0 0 10px rgba(255, 204, 0, 0.4);
        }

        .container { padding: 20px; max-width: 450px; margin: 0 auto; }

        .match-card { 
            background: var(--card); 
            border-radius: 20px; 
            padding: 25px; 
            margin-bottom: 25px; 
            border: 1px solid #333; 
            position: relative; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.7);
            border-top: 2px solid var(--gold);
        }

        .match-time {
            font-size: 0.75em;
            color: #888;
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .match-teams { 
            font-family: 'Cinzel', serif;
            color: var(--gold); 
            font-size: 1.2em; 
            margin-bottom: 10px; 
            display: block; 
        }

        .confiance {
            display: inline-block;
            font-size: 0.8em;
            color: var(--win);
            margin-bottom: 15px;
            font-weight: bold;
            letter-spacing: 1px;
        }

        .score-grid { 
            display: grid; 
            grid-template-columns: 1fr 1fr; 
            gap: 15px; 
            background: rgba(0,0,0,0.4); 
            padding: 20px; 
            border-radius: 15px;
            border: 1px solid #222;
        }

        .score-item { text-align: center; }
        .label { font-size: 0.65em; color: #777; display: block; margin-bottom: 8px; text-transform: uppercase; }
        .val { font-size: 1.5em; font-weight: 600; color: #fff; }

        .win-badge { 
            position: absolute; 
            top: -10px; 
            right: 20px; 
            background: var(--win); 
            color: #000; 
            font-size: 10px; 
            font-weight: 900; 
            padding: 5px 15px; 
            border-radius: 50px; 
            box-shadow: 0 0 10px var(--win);
        }

        .email-fixed {
            position: fixed;
            bottom: 25px;
            right: 25px;
            background: var(--gold-gradient);
            width: 55px;
            height: 55px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            text-decoration: none;
            font-size: 24px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
            z-index: 1000;
            transition: 0.3s;
        }

        #admin-panel { display: none; background: #111; padding: 25px; border: 1px solid var(--gold); margin: 20px; border-radius: 20px; }
        input, select { width: 90%; padding: 12px; margin: 10px 0; background: #000; border: 1px solid #333; color: white; border-radius: 10px; }
        .btn-add { background: var(--gold-gradient); color: black; border: none; padding: 15px; width: 100%; font-weight: bold; border-radius: 10px; cursor: pointer; }
        
        .footer { color: #222; font-size: 9px; margin: 60px 0; cursor: pointer; }
    </style>
</head>
<body>

    <header>
        <h1>𝐵𝐴𝑁𝑇𝐼𝐴    𝐄𝐋𝐈𝐓𝐄</h1>
    </header>

    <div class="tabs">
        <button class="tab-btn active" id="btn-live" onclick="switchTab('live')">Dernières Analyses</button>
        <button class="tab-btn" id="btn-history" onclick="switchTab('history')">Archives Victoires</button>
    </div>

    <div class="container" id="liste-matchs"></div>

    <a href="mailto:bantiaelite@gmail.com" class="email-fixed">✉️</a>

    <div id="admin-panel" class="container">
        <h3 style="color: var(--gold);">SYSTÈME ADMIN</h3>
        <input type="text" id="eq" placeholder="Nom du Match">
        <input type="text" id="time" placeholder="Heure du Match (ex: 20:45)">
        <input type="text" id="ht" placeholder="Score HT">
        <input type="text" id="ft" placeholder="Score FT">
        <input type="text" id="conf" placeholder="Confiance % (ex: 95%)">
        <select id="status">
            <option value="en_cours">Match en cours</option>
            <option value="gagne">Gagné ✅</option>
        </select>
        <button class="btn-add" onclick="envoyerMatch()">PUBLIER SUR L'APP</button>
    </div>

    <p class="footer" onclick="ouvrirAdmin()">© 2026 Bantia Elite Secure</p>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, collection, addDoc, deleteDoc, doc, query, orderBy, onSnapshot } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCrDehL-WzJP0jrw-i6iOTIXMNl9nrXOMI",
            authDomain: "bantiaapp.firebaseapp.com",
            projectId: "bantiaapp",
            storageBucket: "bantiaapp.firebasestorage.app",
            appId: "1:1017776145922:web:baa915ce6bf6196fc9c91b"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        let currentTab = 'live';

        window.switchTab = (tab) => {
            currentTab = tab;
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('btn-' + tab).classList.add('active');
            chargerDonnees();
        };

        function chargerDonnees() {
            onSnapshot(query(collection(db, "matchs"), orderBy("timestamp", "desc")), (s) => {
                const div = document.getElementById("liste-matchs");
                div.innerHTML = "";
                s.forEach((match) => {
                    const m = match.data();
                    if(currentTab === 'live' && m.status === 'gagne') return;
                    if(currentTab === 'history' && m.status !== 'gagne') return;
                    
                    div.innerHTML += `
                        <div class="match-card">
                            ${m.status === 'gagne' ? '<span class="win-badge">WINNER</span>' : ''}
                            <span class="match-time">🕒 ${m.heure || '--:--'}</span>
                            <span class="match-teams">${m.equipes}</span>
                            <div class="confiance">CONFIANCE: ${m.confiance || "90%"}</div>
                            <div class="score-grid">
                                <div class="score-item"><span class="label">HALF TIME</span><span class="val">${m.ht}</span></div>
                                <div class="score-item" style="border-left: 1px solid #333"><span class="label">FULL TIME</span><span class="val">${m.ft}</span></div>
                            </div>
                            <button class="btn-del" style="display:none; color:#ff4444; border:none; background:none; margin-top:20px; font-size:10px;" onclick="supprimer('${match.id}')">[EFFACER]</button>
                        </div>`;
                });
            });
        }

        window.ouvrirAdmin = () => {
            const t = prompt("Code Maître :");
            if(btoa(t) === "NjU0MDkx") {
                document.getElementById('admin-panel').style.display = 'block';
                document.querySelectorAll('.btn-del').forEach(b => b.style.display = 'inline-block');
            }
        };

        window.envoyerMatch = async () => {
            await addDoc(collection(db, "matchs"), {
                equipes: document.getElementById('eq').value,
                heure: document.getElementById('time').value,
                ht: document.getElementById('ht').value,
                ft: document.getElementById('ft').value,
                confiance: document.getElementById('conf').value,
                status: document.getElementById('status').value,
                timestamp: new Date()
            });
            alert("Match publié !");
            document.getElementById('eq').value = "";
            document.getElementById('time').value = "";
        };

        window.supprimer = async (id) => { if(confirm("Supprimer ?")) await deleteDoc(doc(db, "matchs", id)); };
        chargerDonnees();
    </script>
</body>
</html>