# Guide de personnalisation - Matrice de risque et champs personnalisés

Ce guide explique comment utiliser la matrice de risque personnalisée avec votre système de priorité à 4 niveaux et comment intégrer vos champs personnalisés (complexité, type d'action, périodicité).

## 1. Matrice de risque personnalisée

### Fichier créé
- **Fichier**: `backend/library/libraries/risk-matrix-custom-priority-4x4.yaml`
- **URN**: `urn:custom:risk:library:risk-matrix-priority-4x4`

### Structure de la matrice

La matrice utilise un système 5×5 (5 niveaux de probabilité × 5 niveaux d'impact) qui mappe vers 4 niveaux de risque correspondant à vos priorités :

| Priorité | Niveau de risque | Plage de score | Couleur |
|----------|------------------|---------------|---------|
| **P4** | Risque Faible | [1-20[ | Vert (#02A45A) |
| **P3** | Risque Modéré | [20-36[ | Orange (#FFA600) |
| **P2** | Risque Elevé | [36-64[ | Rouge clair (#FF1A00) |
| **P1** | Risque Extrême | [64-120] | Rouge foncé (#C00000) |

### Import de la matrice

1. **Via l'interface web** :
   - Allez dans **Governance → Library**
   - Cliquez sur **Load**
   - Sélectionnez le fichier `risk-matrix-custom-priority-4x4.yaml`
   - La matrice sera disponible pour vos évaluations de risque

2. **Via fichiers système** (si vous avez accès au système de fichiers) :
   - Le fichier est déjà placé dans `backend/library/libraries/`
   - Redémarrez le backend pour que la matrice soit détectée

### Utilisation dans les évaluations de risque

Lors de la création ou modification d'une **Risk Assessment** :
1. Sélectionnez la matrice "Matrice de risque personnalisée - Système de priorité 4 niveaux"
2. Les scénarios de risque utiliseront automatiquement cette matrice
3. Les niveaux de risque calculés correspondront à vos priorités (P1-P4)

## 2. Mapping des champs personnalisés

### Table 1 : Périodicité (Durée de remédiation)

**Champ existant dans CISO Assistant** : `eta` (Estimated Time of Arrival) sur les objets `AppliedControl` et `RiskScenario`

**Mapping recommandé** :

| Votre périodicité | Utilisation du champ `eta` |
|-------------------|----------------------------|
| QuickWin (0-3 mois) | `eta` = Date actuelle + 3 mois |
| Court terme (3-12 mois) | `eta` = Date actuelle + 12 mois |
| Moyen terme (12-18 mois) | `eta` = Date actuelle + 18 mois |
| Long terme (>18 mois) | `eta` = Date actuelle + 24 mois (ou plus) |
| Périodique | Utiliser un `eta` récurrent ou créer un contrôle périodique |

**Note** : Le champ `eta` est disponible dans :
- **Applied Controls** (Contrôles appliqués)
- **Risk Scenarios** (Scénarios de risque)
- **Requirement Assessments** (Évaluations d'exigences)

### Table 2 : Niveau de complexité

**Champ existant dans CISO Assistant** : `effort` sur `AppliedControl`

**Mapping recommandé** :

| Votre complexité | Champ `effort` existant | Description |
|------------------|------------------------|-------------|
| **Faible** | `XS` (Extra Small) ou `S` (Small) | Pas de coût d'acquisition, modification simple |
| **Moyen** | `M` (Medium) | <10 jours/homme, <100k dirhams |
| **Important** | `L` (Large) ou `XL` (Extra Large) | >10 jours/homme, >100k dirhams |

**Alternative** : Vous pouvez utiliser le champ `cost` (coût) sur `AppliedControl` pour stocker le montant en dirhams.

### Table 3 : Type de l'action

**Champ existant dans CISO Assistant** : `category` sur `AppliedControl`

**Mapping recommandé** :

| Votre type d'action | Champ `category` existant | Notes |
|---------------------|--------------------------|-------|
| **Action Organisationnelle** | `policy` ou `process` | Modifications de processus/politiques |
| **Action Technique** | `technical` | Interventions sur SI/infrastructures |
| **Action Organisationnelle et Technique** | `procedure` | Combinaison des deux |

**Champs disponibles** :
- `policy` : Politique
- `process` : Processus
- `technical` : Technique
- `physical` : Physique
- `procedure` : Procédure

### Table 4 : Priorité de l'action

**Champ existant dans CISO Assistant** : `priority` sur `AppliedControl` et `Finding`

**Mapping automatique** : La priorité est automatiquement déterminée par le niveau de risque calculé par la matrice :

| Niveau de risque (matrice) | Priorité CISO Assistant | Votre priorité |
|----------------------------|------------------------|----------------|
| P4 - Risque Faible [1-20[ | `priority = 4` | Priorité 4 |
| P3 - Risque Modéré [20-36[ | `priority = 3` | Priorité 3 |
| P2 - Risque Elevé [36-64[ | `priority = 2` | Priorité 2 |
| P1 - Risque Extrême [64-120] | `priority = 1` | Priorité 1 |

**Note** : Le champ `priority` est déjà présent sur les objets `AppliedControl` et `Finding`. Vous pouvez le définir manuellement ou créer une logique automatique basée sur le niveau de risque.

## 3. Personnalisation avancée (optionnel)

Si vous avez besoin de champs supplémentaires spécifiques, vous pouvez :

### Option A : Utiliser les champs existants avec des conventions

Créez un document de référence interne qui mappe vos besoins aux champs existants de CISO Assistant.

### Option B : Étendre les modèles (développement)

Si vous avez besoin de champs dédiés, vous pouvez étendre les modèles Django :

1. **Créer une migration** pour ajouter les champs :
   ```python
   # Exemple dans backend/core/models.py
   class AppliedControl(...):
       # Champs existants...
       
       # Nouveaux champs personnalisés
       complexity_level = models.CharField(
           max_length=20,
           choices=[
               ("faible", "Faible"),
               ("moyen", "Moyen"),
               ("important", "Important"),
           ],
           null=True,
           blank=True,
       )
       
       action_type = models.CharField(
           max_length=50,
           choices=[
               ("organisationnelle", "Action Organisationnelle"),
               ("technique", "Action Technique"),
               ("mixte", "Action Organisationnelle et Technique"),
           ],
           null=True,
           blank=True,
       )
       
       periodicity = models.CharField(
           max_length=50,
           choices=[
               ("quickwin", "QuickWin (0-3 mois)"),
               ("court_terme", "Court terme (3-12 mois)"),
               ("moyen_terme", "Moyen terme (12-18 mois)"),
               ("long_terme", "Long terme (>18 mois)"),
               ("periodique", "Périodique"),
           ],
           null=True,
           blank=True,
       )
   ```

2. **Mettre à jour les serializers** dans `backend/core/serializers.py`

3. **Mettre à jour les formulaires frontend** dans `frontend/src/lib/components/Forms/`

4. **Créer et appliquer la migration** :
   ```bash
   cd backend
   poetry run python manage.py makemigrations
   poetry run python manage.py migrate
   ```

## 4. Utilisation pratique

### Workflow recommandé

1. **Créer une évaluation de risque** avec votre matrice personnalisée
2. **Identifier les scénarios de risque** - les priorités seront calculées automatiquement
3. **Créer des Applied Controls** pour chaque action de remédiation :
   - Définir la `priority` (P1-P4) basée sur le niveau de risque
   - Définir l'`effort` (complexité) : XS/S/M/L/XL
   - Définir la `category` (type d'action) : policy/process/technical/procedure
   - Définir l'`eta` (périodicité) : date d'échéance
   - Optionnel : définir le `cost` (coût en dirhams)

### Exemple de création d'un Applied Control

```
Nom : Mise en place d'une politique de sécurité
Description : ...
Priority : 1 (P1 - Risque Extrême)
Effort : L (Large - Complexité importante)
Category : policy (Action Organisationnelle)
ETA : 2025-04-27 (Court terme - 3 mois)
Cost : 50000 (50k dirhams)
```

## 5. Rapports et filtres

Vous pouvez filtrer et générer des rapports basés sur :
- **Priority** : P1, P2, P3, P4
- **Effort** : Complexité (XS/S/M/L/XL)
- **Category** : Type d'action
- **ETA** : Périodicité/délais

Ces filtres sont disponibles dans l'interface web de CISO Assistant.

## 6. Support et questions

Pour toute question ou besoin d'assistance supplémentaire :
- Consultez la documentation officielle : https://intuitem.gitbook.io/ciso-assistant
- Rejoignez la communauté Discord : https://discord.gg/qvkaMdQ8da

