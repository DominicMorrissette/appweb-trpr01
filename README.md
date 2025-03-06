Description du projet :

J'ai fait une application de gestion de jeux vidéo Qui permet de faire l'ajout, la suppression, la modification et l'affichage d'un jeu.
L'application permet d'afficher les détails d'un jeu, tout en restant user friendly. De plus, un form de modification est affiché dans la
card du jeu en question pour s'implifier la modification. Des Modals sont affichés lors de la modification, duplication et la suppression d'un jeux
Il y a aussi une bar de recherche réactive pour trouver un élément.

Tous les logos affichées dans les boutons sont fait par ChatGPT.

J'ai aussi utiliser la documentation de boostrap sur les Modal pour afficher mes messages de confirmation ou de suppression par exemple.

J'ai utiliser ChatGPT Pour m'aider à faire en partie l'affichage bootsrap de l'application, surtout pour bien placer les cards (je me suis battu avec eux :( )).

Je l'ai bien sûr aussi utiliser pour me débuger, mais ce n'est pas très important, je pense. Je le marque pareil au cas ou.

J'ai utiliser ChatGTP pour m'expliquer la démarche et me faire un plan d'attaque pour le ExportCSV:
Voici le prompt en question et sa réponse.

    Moi:
    Je veux que tu me donnes des explications et un guide pour faire cette énoncé sans me donner de code. Je veux pouvoir comprendre ce que tu fais et comment je suis sensé l'implémenté en tant que composant vue. Je répète, ne me donne pas de code. Explique et donne un plan pour le réaliser.

    Voici le récit:
    Je peux exporter la liste des produits en CSV Bouton "Exporter" permettant de télécharger la liste dans le format CSV.


    ChatGPT:
    📌 Explication et guide pour créer un composant Vue permettant d'exporter une liste en CSV
    Nous allons concevoir un composant réutilisable qui permet d'exporter une liste de produits en CSV. Ce composant prendra une liste d’objets en props, les transformera en format CSV, et permettra de télécharger le fichier lorsqu'on clique sur un bouton.

    1️⃣ Plan de conception du composant Vue
    Nous allons suivre une approche bien structurée :

    Définir les props :

    Le composant doit accepter une liste d'objets représentant les produits.
    Chaque objet aura des propriétés comme id, title, price, description, etc.
    Créer une fonction pour transformer les données en CSV :

    Construire une chaîne de texte avec les en-têtes de colonnes.
    Boucler sur chaque produit pour ajouter une ligne avec ses valeurs séparées par des virgules.
    Générer un fichier CSV téléchargeable :

    Convertir la chaîne en Blob (format de fichier).
    Créer une URL temporaire pour permettre le téléchargement.
    Créer un bouton d’exportation :

    Ajouter un bouton qui appelle la fonction de génération du CSV lorsqu'on clique dessus.
    Intégrer ce composant dans Store.vue :

    Passer la liste des produits en props.
    Placer le bouton dans l'interface utilisateur à l'endroit approprié.
    2️⃣ Détail de l'implémentation
    🟢 Étape 1 : Définition des props
    Le composant devra recevoir une liste de produits sous forme d'un tableau d’objets. Il faut que les props soient bien typés en TypeScript pour éviter les erreurs.

    🟢 Étape 2 : Construction du CSV
    Créer un en-tête CSV ("ID,Titre,Prix,Description,Note,Stock\n").
    Boucler sur la liste et extraire les informations sous forme de lignes séparées par des virgules.
    Gérer les chaînes de texte :
    Si une description contient une virgule, elle doit être entourée de guillemets ("...").
    Convertir les valeurs en texte si nécessaire (toString() pour éviter les erreurs).
    🟢 Étape 3 : Génération du fichier CSV
    Créer un Blob (un fichier temporaire en mémoire) contenant les données CSV.
    Générer une URL de téléchargement temporaire.
    Simuler un clic sur un lien invisible pour déclencher le téléchargement.
    Supprimer l’URL après le téléchargement pour libérer la mémoire.
    🟢 Étape 4 : Ajout d’un bouton d’exportation
    Ajouter un bouton Bootstrap (btn btn-success).
    Sur le @click, appeler la fonction de génération du CSV.
    Ajouter un petit emoji 📤 pour indiquer clairement la fonction du bouton.
    🟢 Étape 5 : Intégration dans Store.vue
    Importer le composant et l’afficher à l’endroit approprié (<ExportCSV :games="games" />).
    Passer la liste des jeux via les props.
    3️⃣ Conclusion et Résultat attendu
    À la fin de cette implémentation : ✔ Un bouton "Exporter" sera disponible.
    ✔ Lorsque l'on clique dessus, un fichier CSV sera généré et téléchargé.
    ✔ Chaque ligne du fichier contiendra un produit avec ses détails.
    ✔ Le format CSV sera propre et compatible avec Excel ou Google Sheets.

    4️⃣ Résumé du guide
    1️⃣ Créer un composant ExportCSV.vue.
    2️⃣ Passer la liste des produits en props.
    3️⃣ Construire un CSV en texte avec les en-têtes et les données.
    4️⃣ Générer un fichier téléchargeable en CSV.
    5️⃣ Ajouter un bouton qui déclenche le téléchargement.
    6️⃣ Intégrer le composant dans Store.vue.

    🚀 Avec ce guide, tu peux maintenant l’implémenter toi-même ! Dis-moi si tu veux plus d’explications sur un point précis. 😊

Voila, je pense que ca fait le tour. Si jamais tu as des questions sur mon code, n'hésite pas à venir m'en parler, je t'expliquerai volontier. Bonne correction Jimmy!!!
