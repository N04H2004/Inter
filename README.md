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
| Calendrier | Fêtes religieuses & culturelles | Diwali, Hanoucca, Épiphanie, Noël orthodoxe, Chandeleur, Ramadan, Aïd el-Fitr, Pâques, Pessah, Aïd el-Kébir, Roch Hachana (dates musulmanes provisoires) |
| Calendrier | Halloween, jours fériés, promo | 31/10 ; Toussaint 1/11 (dimanche), 11/11 |
| Local | Ouverture le dimanche, écoles du quartier, tourisme, événements en ville | Spécifique au magasin de Nice Cessole |

Les dates de calendrier sont réelles ; les articles, stocks, prix et sensibilités sont fictifs
et se modifient dans `PRODUCTS`, `MATCHES`, `WEATHER`… en tête du script.

Le champ **Simuler une date** (barre du haut) rejoue toute la proposition à une autre date ; le lien « simuler » de chaque fête y amène directement. La météo n'est disponible que pour la date réelle de la démo.

### Produits saisonniers et fin de fête
- **Article saisonnier à rotation 0** (ex. pain azyme) : quand la fête entre dans la période couverte, la quantité est calculée à partir
  des ventes de la même fête l'an dernier (recalées sur la date de la fête) × tendance − stock − commandes en cours. Repère « 🗓️ Saison ».
- **Rotation gonflée par une fête** (ex. lben pendant le Ramadan) : la rotation Mercalys (moyenne 28 j) est recalée sur une base hors fête.
  Au début de la fête → accélérateur ; après la fête → **frein suggéré** dans la colonne Frein / Acc (ex. × 0,17 le lendemain du Ramadan).
- Pour voir ces cas : lien « simuler » / « simuler la fin » dans la tuile Fêtes, ou dates 16/04/2027 (Pessah) et 09/03/2027 (fin du Ramadan).
