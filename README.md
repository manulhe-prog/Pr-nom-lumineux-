<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur Lumière 3D</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
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
        input[type="text"], input[type="number"] {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .checkbox-group {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin: 10px 0;
        }
        .checkbox-option {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        .idee-container {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }
        .idee-container input {
            flex: 1;
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
            margin-top: 15px;
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
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculateur Lumière 3D</h1>

        <label for="nom">Prénom * :</label>
        <input type="text" id="nom" placeholder="Ex: Emmanuel">

        <label>Objet supplémentaire :</label>
        <div class="checkbox-group">
            <div class="checkbox-option">
                <input type="checkbox" id="objet-0" name="objet" value="0" checked>
                <label for="objet-0">Aucun objet</label>
            </div>
            <div class="checkbox-option">
                <input type="checkbox" id="objet-1" name="objet" value="1">
                <label for="objet-1">Petit objet (~1cm) +1€</label>
            </div>
            <div class="checkbox-option">
                <input type="checkbox" id="objet-3" name="objet" value="3">
                <label for="objet-3">Objet moyen (~5cm) +3€</label>
            </div>
            <div class="checkbox-option">
                <input type="checkbox" id="objet-5" name="objet" value="5">
                <label for="objet-5">Grand objet (~10cm) +5€</label>
            </div>
        </div>

        <div class="idee-container">
            <input type="checkbox" id="objet-custom" name="objet" value="custom">
            <label for="objet-custom">Idée personnalisée :</label>
            <input type="text" id="idee" placeholder="Ex: Une étoile en suspension" disabled>
        </div>

        <button onclick="calculerPrix()">Calculer le prix</button>

        <div id="resultat"></div>
        <div id="error" class="error"></div>
    </div>

    <script>
        // Désactive les autres cases si une case est cochée
        const checkboxes = document.querySelectorAll('input[name="objet"]');
        const ideeInput = document.getElementById('idee');

        checkboxes.forEach(checkbox => {
            checkbox.addEventListener('change', function() {
                if (this.id === 'objet-custom') {
                    ideeInput.disabled = !this.checked;
                    if (!this.checked) {
                        ideeInput.value = '';
                    }
                }

                // Désactive les autres cases si celle-ci est cochée
                if (this.checked) {
                    checkboxes.forEach(otherCheckbox => {
                        if (otherCheckbox !== this) {
                            otherCheckbox.checked = false;
                        }
                    });
                }
            });
        });

        function calculerPrix() {
            const nom = document.getElementById('nom').value.trim();
            const errorElement = document.getElementById('error');
            const resultatElement = document.getElementById('resultat');

            // Réinitialise les messages
            errorElement.textContent = '';
            resultatElement.innerHTML = '';

            // Vérifie si un prénom est saisi
            if (nom === "") {
                errorElement.textContent = "Veuillez entrer un prénom !";
                return;
            }

            // Vérifie si une option est sélectionnée
            const selectedCheckbox = document.querySelector('input[name="objet"]:checked');
            if (!selectedCheckbox) {
                errorElement.textContent = "Veuillez sélectionner un objet ou une idée personnalisée !";
                return;
            }

            // Si "Idée personnalisée" est sélectionnée, vérifie que le champ est rempli
            if (selectedCheckbox.id === 'objet-custom' && ideeInput.value.trim() === "") {
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

            if (selectedCheckbox.id === 'objet-custom') {
                descriptionObjet = `Idée personnalisée: "${ideeInput.value.trim()}"`;
            } else {
                prixObjet = parseFloat(selectedCheckbox.value);
                const options = {
                    '0': 'Aucun objet',
                    '1': 'Petit objet (~1cm)',
                    '3': 'Objet moyen (~5cm)',
                    '5': 'Grand objet (~10cm)'
                };
                descriptionObjet = options[selectedCheckbox.value];
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
