# OUTLET-DASHBOARD
Objectif principal :
Analyser les performances de ventes, les caractéristiques des articles et des points de vente pour identifier des tendances, des opportunités d’amélioration et optimiser les décisions stratégiques.
## 1. KPI utilisés :
   - Total Sales – Revenus totaux.
   - Average Sales – Revenu moyen par vente.
   - Number of Items – Nombre total d’articles vendus.
   - satisfaction – Note moyenne des clients.

## 2. Description du Dataset

Colonnes principales :

   - Item_Identifier : Identifiant unique pour chaque produit.
   - Item_Weight : Poids de chaque produit.
   - Item_Fat_Content : Indique si le produit est à faible teneur en gras (« Low Fat ») ou non.
   - Item_Visibility : Pourcentage de la zone d’exposition allouée au produit en magasin.
   - Item_Type : Catégorie ou type de produit.
   - Item_MRP : Prix de vente maximum du produit.
   - Outlet_Identifier : Identifiant unique pour chaque magasin.
   - Outlet_Establishment_Year : Année de création du magasin.
   - Outlet_Size : Taille du magasin (« Small », « Medium », « High »).
   - Outlet_Location_Type : Type de région (« Tier 1 », « Tier 2 », « Tier 3 »).
   - Outlet_Type : Type de magasin (« Grocery Store » ou « Supermarket »).
   - Item_Outlet_Sales : Ventes du produit dans le magasin (à prédire).
## 3. Outils utilisés :

- Power BI Desktop et Power BI Services : Pour la création et la publication de visualisations interactives.
- DAX (Data Analysis Expressions) : Pour calculer des mesures et des KPI personnalisés.
- Gateway : Pour connecter Power BI aux sources de données locales.
- Rafraîchissement des Données (Refresh) : Automatisation des mises à jour des données.
- RLS (Row-Level Security) : Pour appliquer des restrictions de sécurité à l’accès des données.
- Connexion avec Excel : Importation des données source à partir de fichiers Excel vers Power BI.


## 4. Étapes de Réalisation

### 1. Collecte des Besoins:

- Comprendre les attentes des parties prenantes :
- Quels produits performent le mieux dans chaque magasin ?
- Quelle est l’influence de la taille et du type de magasin sur les ventes ?
- Comment maximiser les ventes en fonction des types de produits ?

### 2. Préparation des Données:
   
   - Identifier et traiter les valeurs manquantes (ég. : imputation de « Item_Weight » avec la moyenne).
   - Uniformiser les valeurs de « Item_Fat_Content » (ég. : convertir « low fat » et « LF » en « Low Fat 
       »).
   - Supprimer les valeurs nulles ou non pertinentes dans les colonnes critiques (ég. : suppression des lignes avec des valeurs nulles pour « Item_Weight »).
   - verifier l'intégrité des données
### 3.Modélisation des Données:
##### Objectif de la Modélisation :
Diviser le dataset en une table factuelle et des tables dimensionnelles pour améliorer l’efficacité et la lisibilité du modèle.
##### Création des Tables :
 - FACT_Sales : Contient les données transactionnelles et mesures principales : 
   Item Identifier, Outlet Identifier, Item Visibility, Sales, Rating.
```bash
 Item Identifier, Outlet Identifier, Item Visibility, Sales, Rating.
```
- DIM_Item : Contient les informations descriptives des articles :

```bash
Item Identifier, Item Fat Content, Item Type, Item Weight.
```
- DIM_Outlet : Contient les informations descriptives des magasins :

```bash
Outlet Identifier, Outlet Establishment Year, Outlet Location Type, Outlet Size, Outlet Type, Outlet Age (colonne calculée).
```

##### Création des Relations :

Connecter FACT_Sales[Item Identifier] avec DIM_Item[Item Identifier].

Connecter FACT_Sales[Outlet Identifier] avec DIM_Outlet[Outlet Identifier].

##### Configuration des Relations :

Relations de type One-to-Many pour assurer une navigation fluide entre les tables.

### 4. Calcul des KPI avec DAX
#### Total Sales :
```bash
Total Sales = SUM(FACT_Sales[Sales])
```
#### Average Sales :
```bash
Avg sales = AVERAGE(FACT_Sales[Sales])
```
#### Number of Items :
```bash
Nomber of item = COUNTROWS(FACT_Sales)
```
#### Average Rating  :
```bash
Avg Rating = AVERAGE(FACT_Sales[Rating]) 
```


## 5. Implémentation de la Sécurité au Niveau des Lignes (RLS)

Objectif :

Limiter l’accès aux données en fonction des rôles et des besoins des utilisateurs (par exemple, restreindre les données en fonction du type de localisation des magasins).

Étapes :

Accédez à l’onglet Modeling dans Power BI Desktop.

Cliquez sur Manage Roles.

Créez un nouveau rôle pour chaque niveau (par exemple : Tier 1, Tier 2, Tier 3).

Sélectionnez la table concernée, ici DIM_Outlet.

Ajoutez une règle de filtre pour limiter les données visibles, comme :
```bash
[Outlet Location Type] = "Tier 3"
```
- Vérification :

- Testez les rôles en cliquant sur View as Role pour vérifier que seules les données correspondant au filtre sont affichées.

##### Déploiement :

- Publiez le rapport sur Power BI Service.

- Assignez les rôles aux utilisateurs via Power BI Service.


## 6. Configuration de l’Actualisation Automatique avec Gateway

Objectif :

Automatiser le rafraîchissement des données dans Power BI Service pour garantir l’accès à des données toujours à jour.

Installation et Configuration du Gateway :

Téléchargez et installez le Power BI Gateway depuis le site officiel de Microsoft.

Connectez-vous avec vos identifiants Power BI.

Configurez le Gateway en sélectionnant la machine locale et en l’enregistrant dans votre espace Power BI Service.

Configuration des Sources de Données :

Dans Power BI Service, accédez à Settings > Manage Gateways.

Ajoutez une nouvelle source de données et liez-la au fichier Excel utilisé pour votre rapport.

Configurez les informations d’identification (par exemple, utilisateur Windows pour accéder au fichier).

Mise en Place de l’Actualisation Automatique :

Ouvrez votre rapport publié dans Power BI Service.

Accédez à Datasets > Settings.

Planifiez des actualisations automatiques (par exemple, toutes les 24 heures).

Vérification :

Testez le fonctionnement en forçant une actualisation manuelle pour vérifier que le Gateway fonctionne correctement.

















## License

[MIT](https://choosealicense.com/licenses/mit/)
