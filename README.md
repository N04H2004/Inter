# Démo — Réassort du jour avec coefficient de facteurs externes

Ouvrir `index.html` dans un navigateur (aucune installation, aucun serveur).

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

Le champ « Livraison » permet de rejouer une autre date (ex. 05/11/2026 = jour de versement CAF).

Les dates de calendrier sont réelles ; les articles, stocks, prix et sensibilités sont fictifs
et se modifient dans `PRODUCTS`, `MATCHES`, `WEATHER`… en tête du script.
