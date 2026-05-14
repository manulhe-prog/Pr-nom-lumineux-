<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur de Lumière 3D</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input[type="text"], select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }
        .checkbox-group {
            margin: 10px 0;
            padding: 10px;
            background-color: #f9f9f9;
            border-radius: 5px;
        }
        .checkbox-group label {
            font-weight: normal;
            display: block;
            margin: 5px 0;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 12px;
            border: none;
            border-radius: 5px;
            width: 100%;
            font-size: 16px;
            cursor: pointer;
            margin-top: 10px;
        }
        button:hover {
            background-color: #45a049;
        }
        #resultat {
            margin-top: 20px;
            padding: 10px;
            background-color: #e8f5e9;
            border-radius: 5px;
            font-size: 18px;
            text-align: center;
            font-weight: bold;
        }
        .idee-container {
            margin-top: 10px;
            display: none;
        }
        .idee-container input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculateur de Lumière 3D</h1>
        <p>Personnalisez votre lumière en forme de prénom !</p>

        <div class="form-group">
            <label for="nom">Votre prénom :</label>
            <input type="text" id="nom" placeholder="Ex: Emmanuel" required>
        </div>

        <div class="form-group">
            <label for="objet">Objet supplémentaire (optionnel) :</label>
            <select id="objet" onchange="toggleIdea()">
                <option value="0">Aucun objet (+0€)</option>
                <option value="1">Petit objet (~1cm, ex: cœur sur un "i") (+1€)</option>
                <option value="3">Objet moyen (~5cm, ex: étoile, lune) (+3€)</option>
                <option value="5">Grand objet (~10cm, ex: animal, symbole) (+5€)</option>
                <option value="custom">Autre (proposer une idée)</option>
            </select>
        </div>

        <div class="idee-container" id="ideeContainer">
            <label for="idee">Décrivez votre idée :</label>
            <input type="text" id="idee" placeholder="Ex: Un nuage de 7cm">
        </div>

        <button onclick="calculerPrix()">Calculer le prix</button>

        <div id="resultat"></div>
    </div>

    <script>
        // Affiche/masque le champ "idée" si "Autre" est sélectionné
        function toggleIdea() {
            const objetSelect = document.getElementById('objet');
            const ideeContainer = document.getElementById('ideeContainer');
            if (objetSelect.value === 'custom') {
                ideeContainer.style.display = 'block';
            } else {
                ideeContainer.style.display = 'none';
            }
        }

        // Fonction de calcul
        function calculerPrix() {
            const nom = document.getElementById('nom').value.trim();
            const objetSelect = document.getElementById('objet');
            const idee = document.getElementById('idee').value.trim();

            if (nom === "") {
                alert("Veuillez entrer un prénom !");
                return;
            }

            // Calcul du prix de base
            const nombreLettres = nom.length;
            let prixBase = 26; // Prix pour 4 lettres

            if (nombreLettres > 4) {
                const lettresSupplementaires = nombreLettres - 4;
                prixBase += lettresSupplementaires * 1.5;
            }

            // Calcul du prix de l'objet
            let prixObjet = 0;
            let descriptionObjet = "";

            if (objetSelect.value === 'custom') {
                if (idee === "") {
                    alert("Veuillez décrire votre idée ou choisir un objet !");
                    return;
                }
                prixObjet = 0; // À ajuster selon ton modèle (ex: prix fixe ou à calculer plus tard)
                descriptionObjet = ` (Idée personnalisée: "${idee}")`;
            } else {
                prixObjet = parseFloat(objetSelect.value);
                const options = {
                    '0': '',
                    '1': ' (Petit objet ~1cm)',
                    '3': ' (Objet moyen ~5cm)',
                    '5': ' (Grand objet ~10cm)'
                };
                descriptionObjet = options[objetSelect.value] || '';
            }

            const prixTotal = prixBase + prixObjet;

            // Affichage du résultat
            const resultatElement = document.getElementById('resultat');
            resultatElement.innerHTML =
                `Prix pour "${nom}"${descriptionObjet} : <span style="color: #2E7D32;">${prixTotal.toFixed(2)} €</span>`;
        }
    </script>
</body>
</html>
