# Démo — Réassort du jour avec coefficient de facteurs externes

Ouvrir `index.html` dans un navigateur (aucune installation, aucun serveur).

L'écran reprend l'interface **Mercalys Réappro** (Proposition de commande fournisseur) et y ajoute :
- un bloc **Facteurs externes** (météo de demain + tuiles cliquables, dont les facteurs locaux) ;
- dans la liste fournisseurs, les colonnes **Coef. ext.**, **Nb colis ajusté**, **Nb UVC ajusté** (en orange) ;
- l'écran **Détail des articles de la proposition de commande** (double-clic sur un fournisseur), repris de Mercalys (Itm8, Cdt, Classe, Type prév., Stk, Rot. Vte, Nb colis calculés / validés, Nb UVC…) avec la colonne **Coef. ext.** à côté de Nb UVC et le bouton **Explications calculs** qui détaille le coefficient ;
- pour 40 EPICERIE SUCREE LENT, les 18 articles de la page 1 de l'écran réel (sensibilités fictives) ;
- un bouton **Coef. externes** pour comparer avec la proposition brute.

Chaque fournisseur a sa propre date de livraison et sa période de couverture : les facteurs sont recalculés
pour la période du fournisseur sélectionné (ex. 01.FRAIS MEA : sam. 17 → dim. 18/10 ; 40 EPICERIE SUCREE LENT : mar. 20 → lun. 26/10).
La météo (prévision du lendemain) ne s'applique qu'aux livraisons à J+2 au plus.

**Scénario** : Intermarché Express, 143 boulevard de Cessole, 06100 Nice.
Commande du jeudi 15/10/2026 pour livraison le vendredi 16/10/2026, qui couvre ven. → dim. (pas de livraison le week-end).

- **Proposition auto** : quantité calculée par le réapprovisionnement automatique.
- **Coef. externe** : 1 + Σ (sensibilité article × intensité du facteur), borné entre 0,50 et 2,50.
  Survoler le coefficient affiche le détail par facteur.
- **Conseillé** = proposition auto × coefficient, arrondi au colis (PCB).
- Le gestionnaire modifie une quantité (saisie ou ± 1 colis) ou valide tout en un clic.

## Facteurs (intensité calculée sur la période de couverture)

| Groupe | Facteur | Donnée utilisée |
|---|---|---|
| Météo Nice | Douceur / fraîcheur, pluie | Prévision ven. 16/10 : 15–23 °C, sec ; normale 22 °C |
| Pouvoir d'achat | Versement CAF (APL, RSA, alloc.) | Le 5 du mois (sam. → ven., dim. → lun.) : dernier lun. 5/10, prochain jeu. 5/11 |
| Pouvoir d'achat | Salaires | Dernier jour ouvré du mois |
| Football | Matchs diffusés | Strasbourg–PSG sam. 17/10, Angers–OM et Lyon–OGC Nice dim. 18/10… |
| Calendrier | Vacances zone B | Toussaint du sam. 17/10 au dim. 1/11 (reprise lun. 2/11) |
| Calendrier | Halloween, jours fériés, promo | 31/10 ; Toussaint 1/11 (dimanche), 11/11 |
| Local | Ouverture le dimanche, écoles du quartier, tourisme, événements en ville | Spécifique au magasin de Nice Cessole |

Les dates de calendrier sont réelles ; les articles, stocks, prix et sensibilités sont fictifs
et se modifient dans `PRODUCTS`, `MATCHES`, `WEATHER`… en tête du script.

---

# Démo — Drive Intermarché personnalisé (côté client)

Ouvrir `drive.html` dans un navigateur (desktop, aucune installation). Client fictif **Jean**, magasin **Intermarché Express Nice Cessole**.

L'interface reprend le drive actuel (en-tête, menu Rayons, fiche produit, panier, Mon compte) et ajoute :

- **Profil de consommation** (Compte › *Mon profil conso*) : déduit des 6 dernières commandes (foyer, nombre de personnes,
  préférences de consommation, budget, marques, temps de cuisine), modifiable à tout moment, avec retour au profil déduit.
  Il pilote les recommandations, l'ordre des remplacements et les recettes (aperçu en direct sur la page).
- **Remplacements anticipés** : quand on ajoute un produit souvent en rupture sur le drive (taux non affiché au client), la plateforme propose
  tout de suite 3 remplacements classés selon le profil. Le client peut les accepter, les refuser ou les modifier
  (retirer, réordonner, choisir d'autres produits). Les consignes apparaissent dans le panier et à la validation.
- **Local & nouveautés du magasin** : spécialités de Nice (socca chips, olives de Nice AOP, blettes, pissaladière, tourte de blettes,
  pan bagnat, vin de Bellet…) et nouveautés propres au magasin, sur l'accueil et dans le menu Rayons.
- **Bandeau du magasin** sous « Livraison offerte » : messages qui défilent et lien Instagram, définis par le point de vente
  dans `STORE_BANNER`, en tête du script.
- **Recettes** (30, dont 11 niçoises) : classées selon tous les critères du profil (foyer, nombre de personnes, préférences,
  budget, marques, temps de cuisine). Les recettes niçoises n'apparaissent que si le profil inclut « Produits locaux ».
  Chaque recette indique préparation, cuisson, difficulté, matériel, étapes avec les quantités et une astuce. On choisit le
  nombre de personnes, on coche ce qu'on a déjà, et la recette ajoute les produits au panier (avec remplacements si besoin).
  - **Deux montants** : *approvisionnement* (produits entiers achetés au drive) et *coût de la recette* au prorata des quantités
    utilisées, par personne. Les fruits et légumes vendus au poids sont vendus à la pièce : poids moyen × prix au kilo.
  - **PDF** : téléchargement de la recette (jsPDF 2.5.1, licence MIT, intégré au fichier : fonctionne hors connexion).
  - **Vidéo** : lecteur animé avec un avatar chef, sous-titres et voix de synthèse du navigateur (simulation, pas une vraie vidéo).

Produits, prix, taux de rupture et producteurs sont fictifs. Ils se modifient dans `RAW`, `ORDERS`, `RECIPES` et `DERIVED`.
