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
        input, select {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
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
        }
        button:hover {
            background-color: #45a049;
        }
        #resultat {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
            color: #333;
        }
        .slider-container {
            margin: 20px 0;
        }
        .slider-container label {
            display: block;
            margin-bottom: 5px;
        }
        input[type="range"] {
            width: 100%;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculateur de Lumière 3D</h1>
        <p>Entrez votre prénom et choisissez un objet supplémentaire :</p>

        <input type="text" id="nom" placeholder="Ex: Emmanuel" required>

        <div class="slider-container">
            <label for="prixObjet">Prix de l'objet supplémentaire (1€ - 5€) : <span id="valeurObjet">3€</span></label>
            <input type="range" id="prixObjet" min="1" max="5" value="3" step="1">
        </div>

        <button onclick="calculerPrix()">Calculer le prix</button>

        <div id="resultat"></div>
    </div>

    <script>
        // Mettre à jour l'affichage du slider
        document.getElementById('prixObjet').addEventListener('input', function() {
            document.getElementById('valeurObjet').textContent = this.value + '€';
        });

        // Fonction de calcul
        function calculerPrix() {
            const nom = document.getElementById('nom').value.trim();
            const prixObjet = parseFloat(document.getElementById('prixObjet').value);

            if (nom === "") {
                alert("Veuillez entrer un prénom !");
                return;
            }

            const nombreLettres = nom.length;
            let prixBase = 26; // Prix pour 4 lettres

            if (nombreLettres > 4) {
                const lettresSupplementaires = nombreLettres - 4;
                prixBase += lettresSupplementaires * 1.5;
            }

            const prixTotal = prixBase + prixObjet;

            document.getElementById('resultat').innerHTML =
                `Prix pour "${nom}" : <span style="color: #4CAF50;">${prixTotal.toFixed(2)} €</span>`;
        }
    </script>
</body>
</html>
