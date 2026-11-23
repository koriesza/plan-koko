# Raccourci « Plan Santé »

Ce raccourci lit tes données de l'app Santé (FC de repos, VFC, sommeil, entraînements Apple Watch avec la fréquence cardiaque) et les copie dans le presse-papiers. Dans l'app Plan hybride, onglet **Santé**, le bouton **Importer depuis Santé** les récupère.

Le même guide est disponible dans l'app, en bas de l'onglet Santé. Les noms des menus peuvent varier un peu selon ta version d'iOS.

## Format attendu par l'app

Une ligne par mesure, champs séparés par `;`, dates au format `yyyy-MM-dd'T'HH:mm:ss` :

```
RHR;2026-11-23T00:00:00;52
HRV;2026-11-23T03:10:00;48
SLP;2026-11-22T23:50:00;2026-11-23T01:50:00;Endormi (essentiel)
WK;2026-11-22T10:05:00;2026-11-22T11:07:00;10,4 km;Course à pied extérieure
HR;2026-11-22T10:05:05;121
WT;2026-11-23T08:00:00;90,8 kg
WST;2026-11-23T08:00:00;94 cm
```

Les unités (`BPM`, `ms`, `km`) et la virgule décimale sont acceptées.

## 1. Créer le raccourci

Ouvre l'app **Raccourcis**, touche **+**, nomme-le `Plan Santé`.

## 2. Bloc FC de repos

1. Action **Rechercher des échantillons de santé**. Type : **Fréquence cardiaque au repos**. Filtre **Date de début** · **dans les dernières** · `21` jours. Trier par **Date de début**, plus récent d'abord. Pas de limite.
2. Action **Répéter avec chaque élément**.
3. Dans la boucle : **Formater la date**. Touche la date, choisis **Élément répété**, puis sa propriété **Date de début**. Format **Personnalisé** : `yyyy-MM-dd'T'HH:mm:ss`.
4. Action **Texte** : `RHR;` + variable **Date formatée** + `;` + variable **Élément répété** (touche-la et choisis la propriété **Valeur**).
5. Action **Ajouter à la variable**, nom : `Export`.

## 3. Bloc VFC

Refais le bloc 2 avec le type **Variabilité de la fréquence cardiaque** (21 jours) et un texte qui commence par `HRV;`.

## 4. Bloc sommeil

1. Même principe, type **Analyse du sommeil**, filtre `4` jours.
2. Dans la boucle, **deux** actions Formater la date (même format) : **Date de début** et **Date de fin** de l'élément répété.
3. Texte : `SLP;` + début + `;` + fin + `;` + **Élément répété › Valeur**. Puis Ajouter à la variable `Export`.

## 5. Bloc entraînements (avec la FC)

1. Rechercher des échantillons de santé, type **Entraînements**, filtre `3` jours, puis Répéter avec chaque élément.
2. Dans la boucle : deux Formater la date (début et fin), puis Texte : `WK;` + début + `;` + fin + `;` + propriété **distance** de l'élément répété + `;` + propriété **type d'entraînement**. Ajouter à la variable `Export`.
3. Toujours dans la boucle : Rechercher des échantillons de santé, type **Fréquence cardiaque**, filtre **Date de début** · **est entre** · **Élément répété › Date de début** et **Élément répété › Date de fin**. Pas de limite.
4. Juste après, une 2e boucle **Répéter avec chaque élément** sur ces mesures : Formater la date (**Élément répété 2 › Date de début**, même format), Texte `HR;` + date + `;` + **Élément répété 2 › Valeur**, Ajouter à la variable `Export`.

## 6. Fin du raccourci

1. Après toutes les boucles : **Combiner le texte** (variable `Export`, séparateur **Nouvelles lignes**).
2. **Copier dans le presse-papiers**.
3. **Afficher une notification** : `Données copiées : ouvre Plan hybride et touche Importer`.
4. Lance-le une fois et accepte les autorisations Santé. Ouvre l'app, onglet Santé, **Importer depuis Santé**.

## 7. Poids et tour de taille (facultatif)

1. Même principe que le bloc FC de repos, avec le type **Poids** (60 jours) et un texte qui commence par `WT;`.
2. Si tu mesures ton tour de taille dans Santé : type **Tour de taille**, texte `WST;`. Sinon, saisis-le dans l'onglet Progrès de l'app.

## 8. Automatiser

1. Raccourcis › **Automatisation** › **+** › **Réveil** › « Quand le réveil est arrêté » (ou **Heure de la journée**, 7:30) › **Exécuter immédiatement** › raccourci `Plan Santé`.
2. Si ton iOS le propose : une 2e automatisation **Entraînement Apple Watch** › « Se termine » › même raccourci.
3. Chaque matin : ouvre l'app et touche **Importer**.

L'import affiche une erreur ou des lignes non reconnues ? Colle le texte copié dans la conversation avec Claude pour qu'il ajuste le format.
