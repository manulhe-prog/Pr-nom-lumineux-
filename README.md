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
        input[type="text"] {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .checkbox-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin: 10px 0;
        }
        .checkbox-option {
            display: flex;
            align-items: flex-start;
            gap: 10px;
        }
        .idee-container {
            display: flex;
            gap: 10px;
            margin-left: 25px;
            margin-top: 5px;
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
        #recap {
            margin-top: 20px;
            padding: 15px;
            border: 1px dashed #2E7D32;
            border-radius: 8px;
            background-color: #f0f8f0;
        }
        .recap-title {
            font-weight: bold;
            color: #2E7D32;
            margin-bottom: 10px;
            text-align: center;
        }
        .recap-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px;
            margin: 5px 0;
            background: white;
            border-radius: 4px;
            box-shadow: 0 1px 2px rgba(0,0,0,0.1);
        }
        .recap-item-label {
            flex: 1;
        }
        .recap-item-price {
            color: #2E7D32;
            font-weight: bold;
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
        .hide {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculateur Lumière 3D</h1>

        <label for="nom">Prénom * :</label>
        <input type="text" id="nom" placeholder="Ex: Emmanuel">

        <label>Objets supplémentaires (choix multiples) :</label>
        <div class="checkbox-group">
            <div class="checkbox-option">
                <input type="checkbox" id="objet-0" name="objet" value="0">
                <label for="objet-0">Aucun objet</label>
            </div>
            <div class="checkbox-option">
                <input type="checkbox" id="objet-1" name="objet" value="1">
                <label for="objet-1">Petit objet (~1cm)</label>
                <div class="idee-container">
                    <input type="text" id="idee-1" placeholder="Idée pour cet objet (optionnel)" disabled>
                </div>
            </div>
            <div class="checkbox-option">
                <input type="checkbox" id="objet-3" name="objet" value="3">
                <label for="objet-3">Objet moyen (~5cm)</label>
                <div class="idee-container">
                    <input type="text" id="idee-3" placeholder="Idée pour cet objet (optionnel)" disabled>
                </div>
            </div>
            <div class="checkbox-option">
                <input type="checkbox" id="objet-5" name="objet" value="5">
                <label for="objet-5">Grand objet (~10cm)</label>
                <div class="idee-container">
                    <input type="text" id="idee-5" placeholder="Idée pour cet objet (optionnel)" disabled>
                </div>
            </div>
            <div class="checkbox-option">
                <input type="checkbox" id="objet-custom" name="objet" value="custom">
                <label for="objet-custom">Idée personnalisée</label>
                <div class="idee-container">
                    <input type="text" id="idee-custom" placeholder="Décrivez votre idée *" disabled>
                </div>
            </div>
        </div>

        <div id="recap" class="hide">
            <div class="recap-title">📋 Récapitulatif de votre sélection</div>
            <div id="recap-items"></div>
        </div>

        <button onclick="afficherRecap()">Afficher le récapitulatif</button>
        <button onclick="calculerPrix()" style="background-color: #1976D2;">Calculer le prix</button>

        <div id="resultat"></div>
        <div id="error" class="error"></div>
    </div>

    <script>
        // Active/désactive les champs d'idée en fonction des cases cochées
        const checkboxes = document.querySelectorAll('input[name="objet"]');
        checkboxes.forEach(checkbox => {
            const ideeId = `idee-${checkbox.id.split('-')[1]}`;
            const ideeInput = document.getElementById(ideeId) || document.getElementById('idee-custom');

            checkbox.addEventListener('change', function() {
                if (ideeInput) {
                    ideeInput.disabled = !this.checked;
                    if (!this.checked) {
                        ideeInput.value = '';
                    }
                }
            });
        });

        // Affiche le récapitulatif visuel
        function afficherRecap() {
            const nom = document.getElementById('nom').value.trim();
            const errorElement = document.getElementById('error');
            const recapDiv = document.getElementById('recap');
            const recapItemsDiv = document.getElementById('recap-items');

            // Réinitialise les messages
            errorElement.textContent = '';

            // Vérifie si un prénom est saisi
            if (nom === "") {
                errorElement.textContent = "Veuillez entrer un prénom !";
                recapDiv.classList.add('hide');
                return;
            }

            // Vérifie si au moins une option est sélectionnée
            const selectedCheckboxes = document.querySelectorAll('input[name="objet"]:checked');
            if (selectedCheckboxes.length === 0) {
                errorElement.textContent = "Veuillez sélectionner au moins un objet ou une idée personnalisée !";
                recapDiv.classList.add('hide');
                return;
            }

            // Vérifie si "Idée personnalisée" est cochée et que le champ est vide
            const customCheckbox = document.getElementById('objet-custom');
            if (customCheckbox.checked) {
                const customIdeeInput = document.getElementById('idee-custom');
                if (customIdeeInput.value.trim() === "") {
                    errorElement.textContent = "Veuillez décrire votre idée personnalisée !";
                    recapDiv.classList.add('hide');
                    return;
                }
            }

            // Affiche le récapitulatif
            recapItemsDiv.innerHTML = '';
            let totalRecap = 0;

            // Ajoute le prénom
            const nomItem = document.createElement('div');
            nomItem.className = 'recap-item';
            nomItem.innerHTML = `
                <span class="recap-item-label">Prénom: <strong>${nom}</strong></span>
                <span class="recap-item-price">-</span>
            `;
            recapItemsDiv.appendChild(nomItem);

            // Ajoute les objets sélectionnés
            selectedCheckboxes.forEach(checkbox => {
                if (checkbox.id === 'objet-0') {
                    return; // On ignore "Aucun objet"
                }

                const item = document.createElement('div');
                item.className = 'recap-item';

                if (checkbox.id === 'objet-custom') {
                    const idee = document.getElementById('idee-custom').value.trim();
                    item.innerHTML = `
                        <span class="recap-item-label">Idée personnalisée: <strong>"${idee}"</strong></span>
                        <span class="recap-item-price">0 €</span>
                    `;
                } else {
                    const prixObjet = parseFloat(checkbox.value);
                    totalRecap += prixObjet;
                    const idee = document.getElementById(`idee-${checkbox.id.split('-')[1]}`).value.trim();
                    const label = checkbox.nextElementSibling.textContent;
                    const displayLabel = idee ? `${label} (${idee})` : label;
                    item.innerHTML = `
                        <span class="recap-item-label">${displayLabel}</span>
                        <span class="recap-item-price">+${prixObjet} €</span>
                    `;
                }
                recapItemsDiv.appendChild(item);
            });

            // Affiche le prix de base
            const nombreLettres = nom.length;
            let prixBase = 26;
            if (nombreLettres > 4) {
                prixBase += (nombreLettres - 4) * 1.5;
            }
            const baseItem = document.createElement('div');
            baseItem.className = 'recap-item';
            baseItem.innerHTML = `
                <span class="recap-item-label">Prix de base (${nombreLettres} lettres)</span>
                <span class="recap-item-price">${prixBase.toFixed(2)} €</span>
            `;
            recapItemsDiv.appendChild(baseItem);

            // Affiche le total partiel (sans le prix de base)
            if (totalRecap > 0) {
                const totalItem = document.createElement('div');
                totalItem.className = 'recap-item';
                totalItem.style.fontWeight = 'bold';
                totalItem.style.backgroundColor = '#f0f8f0';
                totalItem.innerHTML = `
                    <span class="recap-item-label">Total objets supplémentaires</span>
                    <span class="recap-item-price">${totalRecap.toFixed(2)} €</span>
                `;
                recapItemsDiv.appendChild(totalItem);
            }

            recapDiv.classList.remove('hide');
        }

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

            // Vérifie si au moins une option est sélectionnée
            const selectedCheckboxes = document.querySelectorAll('input[name="objet"]:checked');
            if (selectedCheckboxes.length === 0) {
                errorElement.textContent = "Veuillez sélectionner au moins un objet ou une idée personnalisée !";
                return;
            }

            // Vérifie si "Idée personnalisée" est cochée et que le champ est vide
            const customCheckbox = document.getElementById('objet-custom');
            if (customCheckbox.checked) {
                const customIdeeInput = document.getElementById('idee-custom');
                if (customIdeeInput.value.trim() === "") {
                    errorElement.textContent = "Veuillez décrire votre idée personnalisée !";
                    return;
                }
            }

            // Calcul du prix de base (26€ pour 4 lettres, +1,5€ par lettre supplémentaire)
            const nombreLettres = nom.length;
            let prixBase = 26;
            if (nombreLettres > 4) {
                prixBase += (nombreLettres - 4) * 1.5;
            }

            // Calcul du prix des objets sélectionnés
            let prixTotal = prixBase;
            let descriptionObjets = [];

            selectedCheckboxes.forEach(checkbox => {
                if (checkbox.id === 'objet-0') {
                    // Aucun objet, on ne fait rien
                    return;
                }

                if (checkbox.id === 'objet-custom') {
                    const idee = document.getElementById('idee-custom').value.trim();
                    descriptionObjets.push(`Idée personnalisée: "${idee}"`);
                } else {
                    const prixObjet = parseFloat(checkbox.value);
                    prixTotal += prixObjet;
                    const idee = document.getElementById(`idee-${checkbox.id.split('-')[1]}`).value.trim();
                    const label = checkbox.nextElementSibling.textContent;
                    descriptionObjets.push(idee ? `${label} (${idee})` : label);
                }
            });

            // Affichage du résultat
            const objetsDescription = descriptionObjets.length > 0 ? descriptionObjets.join(', ') : 'Aucun objet';
            resultatElement.innerHTML =
                `Prix pour <strong>"${nom}"</strong> (${objetsDescription}) : <strong style="color: #2E7D32;">${prixTotal.toFixed(2)} €</strong>`;
        }
    </script>
</body>
</html>
