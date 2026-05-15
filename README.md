
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur Lumière 3D</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f9f9f9;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        h1 {
            color: #2E7D32;
            text-align: center;
        }
        label {
            display: block;
            margin-top: 12px;
            font-weight: bold;
        }
        input, select {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #2E7D32;
            color: white;
            border: none;
            padding: 12px;
            width: 100%;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 10px;
        }
        button:hover {
            background-color: #1B5E20;
        }
        #resultat {
            margin-top: 20px;
            padding: 15px;
            text-align: center;
            font-size: 18px;
            border-radius: 4px;
            background-color: #e8f5e9;
        }
        .error {
            color: #d32f2f;
            margin-top: 10px;
            text-align: center;
        }
        #custom-idee-container {
            display: none;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculateur Lumière 3D</h1>

        <label for="nom">Prénom * :</label>
        <input type="text" id="nom" placeholder="Ex: Emmanuel">

        <label for="objet">Objet supplémentaire :</label>
        <select id="objet">
            <option value="0">Aucun objet</option>
            <option value="1">Petit objet (~1cm) +1€</option>
            <option value="3">Objet moyen (~5cm) +3€</option>
            <option value="5">Grand objet (~10cm) +5€</option>
            <option value="custom">Idée personnalisée</option>
        </select>

        <div id="custom-idee-container">
            <label for="idee">Décrivez votre idée * :</label>
            <input type="text" id="idee" placeholder="Ex: Une étoile en suspension">
        </div>

        <button onclick="calculerPrix()">Calculer le prix</button>

        <div id="resultat"></div>
        <div id="error" class="error"></div>
    </div>

    <script>
        // Affiche/masque le champ "Idée personnalisée"
        document.getElementById('objet').addEventListener('change', function() {
            const customContainer = document.getElementById('custom-idee-container');
            if (this.value === 'custom') {
                customContainer.style.display = 'block';
            } else {
                customContainer.style.display = 'none';
            }
        });

        function calculerPrix() {
            const nom = document.getElementById('nom').value.trim();
            const objetSelect = document.getElementById('objet');
            const idee = document.getElementById('idee').value.trim();
            const errorElement = document.getElementById('error');
            const resultatElement = document.getElementById('resultat');

            // Réinitialise les messages
            errorElement.textContent = '';
            resultatElement.innerHTML = '';

            // Vérifications
            if (nom === "") {
                errorElement.textContent = "Veuillez entrer un prénom !";
                return;
            }

            if (objetSelect.value === 'custom' && idee === "") {
                errorElement.textContent = "Veuillez décrire votre idée !";
                return;
            }

            // Calcul du prix de base (26€ pour 4 lettres, +1,5€ par lettre supplémentaire)
            const nombreLettres = nom.length;
            let prixBase = 26;
            if (nombreLettres > 4) {
                prixBase += (nombreLettres - 4) * 1.5;
            }

            // Calcul du prix de l'objet
            let prixObjet = 0;
            let descriptionObjet = "";

            if (objetSelect.value === 'custom') {
                descriptionObjet = `Idée personnalisée: "${idee}"`;
            } else {
                prixObjet = parseFloat(objetSelect.value);
                const options = {
                    '0': 'Aucun objet',
                    '1': 'Petit objet (~1cm)',
                    '3': 'Objet moyen (~5cm)',
                    '5': 'Grand objet (~10cm)'
                };
                descriptionObjet = options[objetSelect.value];
            }

            // Prix total
            const prixTotal = prixBase + prixObjet;

            // Affichage du résultat
            resultatElement.innerHTML =
                `Prix pour <strong>"${nom}"</strong> (${descriptionObjet}) : <strong style="color: #2E7D32;">${prixTotal.toFixed(2)} €</strong>`;
        }
    </script>
</body>
</html>
