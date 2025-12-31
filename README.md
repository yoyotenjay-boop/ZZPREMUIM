<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CORRECT SCORE</title>
    <style>
        :root { --gold:#d4af37; --dark: #050505; }
        body { background: var(--dark); color: white; font-family: sans-serif; margin: 0; padding: 20px; text-align: center; }
        .annonce { color: var(--gold); border: 1px solid var(--gold); padding: 10px; border-radius: 8px; margin-bottom: 20px; }
        
        /* Panneau Admin caché par défaut */
        #admin-panel { 
            display: none; 
            background: #111; 
            padding: 15px; 
            border-radius: 10px; 
            margin: 20px auto; 
            border: 1px solid #333;
            max-width: 400px;
        }
        
        input { background: #222; border: 1px solid #444; color: white; padding: 10px; margin: 5px 0; width: 90%; border-radius: 5px; }
        .btn-publier { background: var(--gold); color: black; font-weight: bold; border: none; padding: 12px; width: 95%; border-radius: 5px; cursor: pointer; }
        .match-card { border-bottom: 1px dashed #333; padding: 20px 0; position: relative; }
        
        /* Bouton supprimer discret pour l'admin */
        .btn-delete { background: #ff4d4d; color: white; border: none; padding: 5px; border-radius: 3px; font-size: 10px; cursor: pointer; margin-top: 10px; }
    </style>
</head>
<body>
    <h1>𝕮𝑶𝑹𝑹𝑬𝑪𝑻 𝕾𝑪𝑶𝑹𝑬</h1>
    <div id="annonce-serveur" class="annonce">Chargement des analyses...</div>
    
    <div id="liste-matchs"></div>

    <h1>[𝐁𝐎𝐍𝐍𝐄 𝐂𝐇𝐀𝐍𝐂𝐄]</h1>

    <div id="admin-panel">
        <h3 style="color: var(--gold);">ESPACE ADMINISTRATEUR</h3>
        <input type="text" id="add-eq" placeholder="Équipes (ex: ARSENAL VS LYON)">
        <input type="text" id="add-ht" placeholder="Score HT">
        <input type="number" id="add-cht" placeholder="% Confiance HT">
        <input type="text" id="add-ft" placeholder="Score FT">
        <input type="number" id="add-cft" placeholder="% Confiance FT">
        <button class="btn-publier" onclick="envoyerMatch()">PUBLIER LE MATCH</button>
    </div>

    <p onclick="ouvrirAdmin()" style="color: #1a1a1a; font-size: 10px; margin-top: 80px; cursor: pointer;">© 2025 Correct Score Analysis</p>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, onSnapshot, collection, addDoc, deleteDoc, query, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

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

        // Affichage des news
        onSnapshot(doc(db, "config", "news"), (s) => {
            if(s.exists()) document.getElementById("annonce-serveur").innerText = "📢 " + s.data().message;
        });

        // Affichage des matchs (Public)
        onSnapshot(query(collection(db, "matchs"), orderBy("timestamp", "desc")), (s) => {
            const div = document.getElementById("liste-matchs");
            div.innerHTML = "";
            s.forEach((match) => {
                const m = match.data();
                div.innerHTML += `
                    <div class="match-card">
                        <h2 style="color:var(--gold)">${m.equipes}</h2>
                        <p>𝐇𝐓: ${m.ht} (${m.cht}%)</p>
                        <p>𝐅𝐓: ${m.ft} (${m.cft}%)</p>
                        <button class="btn-delete" style="display:none" class="admin-only" onclick="supprimer('${match.id}')">Supprimer</button>
                    </div>`;
            });
        });

        // Fonction SECRÈTE pour ouvrir l'admin
        window.ouvrirAdmin = () => {
            const mdp = prompt("Code Administrateur :");
            if(mdp === "654091") {
                document.getElementById('admin-panel').style.display = 'block';
                // Affiche aussi les boutons supprimer
                document.querySelectorAll('.btn-delete').forEach(btn => btn.style.display = 'inline-block');
                alert("Bienvenue Bantia ! Mode admin activé.");
            } else {
                alert("Accès refusé.");
            }
        };

        // Envoyer un match
        window.envoyerMatch = async () => {
            const eq = document.getElementById('add-eq').value;
            if(!eq) return alert("Remplis au moins les équipes");

            try {
                await addDoc(collection(db, "matchs"), {
                    equipes: eq,
                    ht: document.getElementById('add-ht').value,
                    cht: document.getElementById('add-cht').value,
                    ft: document.getElementById('add-ft').value,
                    cft: document.getElementById('add-cft').value,
                    timestamp: new Date()
                });
                alert("Match publié ! ✅");
                document.getElementById('add-eq').value = "";
            } catch(e) { alert("Erreur serveur"); }
        };

        // Supprimer un match
        window.supprimer = async (id) => {
            if(confirm("Supprimer ce match ?")) {
                await deleteDoc(doc(db, "matchs", id));
            }
        };
    </script>
</body>
</html>
