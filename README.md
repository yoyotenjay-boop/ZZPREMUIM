<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BANTIA CORRECT SCORE</title>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <meta name="apple-mobile-web-app-title" content="Bantia Score">
    <meta name="theme-color" content="#0a0a0a">

    <style>
        :root { 
            --gold: #ffcc00; 
            --gold-dark: #b8860b;
            --dark: #0a0a0a; 
            --card: #161616;
        }
        body { 
            background: var(--dark); 
            color: #eee; 
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; 
            margin: 0; 
            padding: 0; 
            text-align: center; 
        }
        header {
            background: linear-gradient(180deg, #1a1a1a 0%, var(--dark) 100%);
            padding: 30px 10px;
            border-bottom: 2px solid var(--gold);
        }
        h1 { 
            margin: 0; 
            font-size: 2.2em; 
            letter-spacing: 2px; 
            color: var(--gold);
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        .annonce { 
            background: rgba(255, 204, 0, 0.1);
            color: var(--gold); 
            border: 1px solid var(--gold-dark); 
            padding: 12px; 
            margin: 20px auto;
            width: 85%;
            border-radius: 4px; 
            font-weight: bold;
            font-size: 0.9em;
        }
        .match-card { 
            background: var(--card);
            border: 1px solid #222;
            border-left: 4px solid var(--gold);
            padding: 20px; 
            margin: 15px auto; 
            width: 90%;
            max-width: 500px;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.6);
        }
        .match-card h2 { color: var(--gold); margin: 0 0 15px 0; font-size: 1.1em; border-bottom: 1px solid #333; padding-bottom: 10px; }
        .score-row { display: flex; justify-content: space-around; margin-top: 10px; }
        .score-box { background: #222; padding: 10px; border-radius: 5px; min-width: 90px; }
        .score-label { font-size: 0.7em; color: #888; display: block; margin-bottom: 5px; }
        .score-val { font-weight: bold; font-size: 1.2em; color: white; }
        .confiance { color: var(--gold); font-size: 0.8em; display: block; margin-top: 5px; }

        /* Panneau Admin */
        #admin-panel { display: none; background: #111; padding: 20px; border-radius: 10px; margin: 20px auto; border: 2px solid var(--gold); max-width: 400px; }
        input { background: #222; border: 1px solid #444; color: white; padding: 12px; margin: 8px 0; width: 90%; border-radius: 5px; font-size: 16px; }
        .btn-publier { background: var(--gold); color: black; font-weight: bold; border: none; padding: 15px; width: 95%; border-radius: 5px; cursor: pointer; margin-top: 10px; }
        .btn-delete { background: #8b0000; color: white; border: none; padding: 8px 15px; border-radius: 4px; cursor: pointer; margin-top: 15px; font-size: 11px; }
        .footer-secret { color: #1a1a1a; font-size: 10px; margin: 60px 0 30px 0; cursor: pointer; }
    </style>
</head>
<body>
    <header>
        <h1>𝕮𝑶𝑹𝑹𝑬𝑪𝑻 𝕾𝑪𝑶𝑹𝑬</h1>
    </header>

    <div id="annonce-serveur" class="annonce">MATCH D'AUJOURD'HUI </div>
    
    <div id="liste-matchs">
        </div>

    <h2 style="color: #222; margin-top: 40px;">[𝐁𝐎𝐍𝐍𝐄 𝐂𝐇𝐀𝐍𝐂𝐄]</h2>

    <div id="admin-panel">
        <h3 style="color: var(--gold);">ADMINISTRATION BANTIA</h3>
        <input type="text" id="add-eq" placeholder="Équipes (ex: PSG vs Real)">
        <input type="text" id="add-ht" placeholder="Score Mi-temps (HT)">
        <input type="number" id="add-cht" placeholder="% Confiance HT">
        <input type="text" id="add-ft" placeholder="Score Final (FT)">
        <input type="number" id="add-cft" placeholder="% Confiance FT">
        <button class="btn-publier" onclick="envoyerMatch()">PUBLIER LE MATCH</button>
    </div>

    <p class="footer-secret" onclick="ouvrirAdmin()">© 2025 Correct Score Analysis - Bantia</p>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, onSnapshot, collection, addDoc, deleteDoc, query, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // Ta configuration Firebase
        const firebaseConfig = {
            apiKey: "AIzaSyCrDehL-WzJP0jrw-i6iOTIXMNl9nrXOMI",
            authDomain: "bantiaapp.firebaseapp.com",
            projectId: "bantiaapp",
            storageBucket: "bantiaapp.firebasestorage.app",
            messagingSenderId: "1017776145922",
            appId: "1:1017776145922:web:baa915ce6bf6196fc9c91b"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // Récupérer l'annonce (News)
        onSnapshot(doc(db, "config", "news"), (s) => {
            if(s.exists()) document.getElementById("annonce-serveur").innerText = "📢 " + s.data().message;
        });

        // Afficher les matchs en temps réel
        onSnapshot(query(collection(db, "matchs"), orderBy("timestamp", "desc")), (s) => {
            const div = document.getElementById("liste-matchs");
            div.innerHTML = "";
            s.forEach((match) => {
                const m = match.data();
                div.innerHTML += `
                    <div class="match-card">
                        <h2>${m.equipes}</h2>
                        <div class="score-row">
                            <div class="score-box">
                                <span class="score-label">HALF TIME (HT)</span>
                                <span class="score-val">${m.ht}</span>
                                <span class="confiance">${m.cht}% Confiance</span>
                            </div>
                            <div class="score-box" style="border-left: 1px solid #333; padding-left: 10px;">
                                <span class="score-label">FULL TIME (FT)</span>
                                <span class="score-val">${m.ft}</span>
                                <span class="confiance">${m.cft}% Confiance</span>
                            </div>
                        </div>
                        <button class="btn-delete" style="display:none" class="admin-only" onclick="supprimer('${match.id}')">SUPPRIMER</button>
                    </div>`;
            });
        });

        // Fonction pour ouvrir l'admin
        window.ouvrirAdmin = () => {
            const mdp = prompt("Entrez le mot de passe Bantia :");
            if(mdp === "654091") {
                document.getElementById('admin-panel').style.display = 'block';
                document.querySelectorAll('.btn-delete').forEach(btn => btn.style.display = 'inline-block');
                alert("Accès autorisé. Bienvenue !");
            } else { alert("Code incorrect"); }
        };

        // Fonction pour publier un match
        window.envoyerMatch = async () => {
            const eq = document.getElementById('add-eq').value;
            if(!eq) return alert("Veuillez entrer le nom des équipes.");
            try {
                await addDoc(collection(db, "matchs"), {
                    equipes: eq,
                    ht: document.getElementById('add-ht').value || "À venir",
                    cht: document.getElementById('add-cht').value || "0",
                    ft: document.getElementById('add-ft').value || "À venir",
                    cft: document.getElementById('add-cft').value || "0",
                    timestamp: new Date()
                });
                alert("Match ajouté avec succès !");
                document.getElementById('add-eq').value = "";
            } catch(e) { 
                console.error(e);
                alert("Erreur serveur : Vérifiez vos règles Firestore."); 
            }
        };

        // Fonction pour supprimer
        window.supprimer = async (id) => {
            if(confirm("Supprimer définitivement ce match ?")) {
                await deleteDoc(doc(db, "matchs", id));
            }
        };
    </script>
</body>
</html>
