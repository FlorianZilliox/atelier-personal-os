# Framework Workflow & Jira

> **Version:** 1.1 | **Créé:** 9 février 2026
> **Objectif:** Bonnes pratiques workflows agiles et configuration Jira

---

## 1. Principes fondamentaux des workflows

### 1.1 Qu'est-ce qu'un workflow ?

Un workflow représente le cycle de vie d'un ticket : les états possibles et les transitions entre eux. C'est le reflet du processus de travail de l'équipe.

**Un bon workflow** :
- Reflète la réalité du travail (pas un idéal théorique)
- Permet de mesurer ce qui compte
- Est compris et utilisé par tous
- Évolue avec les besoins

---

## 2. Bonnes pratiques

### 2.1 Simplicité : moins de statuts = plus de clarté

**La pratique** : Limiter le nombre de statuts (5-8 maximum).

**Pourquoi c'est bien** :
- Moins de décisions à prendre → moins de friction
- Moins d'erreurs de classification
- Plus facile à comprendre pour les nouveaux
- Maintenance simplifiée
- Rapports plus lisibles

**Exemples** :
```
Minimum viable (5 statuts) :
BACKLOG → À FAIRE → EN COURS → À VALIDER → TERMINÉ

Standard (7 statuts) :
BACKLOG → À AFFINER → À FAIRE → EN COURS → CODE REVIEW → À TESTER → TERMINÉ
```

---

### 2.2 Point de passage obligatoire pour l'engagement

**La pratique** : Forcer tous les tickets à passer par "À FAIRE" avant "EN COURS".

**Pourquoi c'est bien** :
- Marque clairement l'engagement (début du cycle time)
- Permet de mesurer la prédictibilité
- Force une décision explicite : "ce ticket est prêt et prioritaire"
- Base fiable pour les rapports (EazyBI, vélocité, etc.)
- Évite les tickets "fantômes" commencés sans engagement

**Comment l'implémenter** :
- Supprimer la transition "N'importe lequel → EN COURS"
- Garder uniquement "À FAIRE → EN COURS"

---

### 2.3 Flexibilité dans les zones non critiques

**La pratique** : Permettre des transitions libres dans le backlog et pendant l'exécution.

**Pourquoi c'est bien** :
- Moins de friction au quotidien
- Autonomie de l'équipe
- Adaptation aux cas particuliers
- Réduit les contournements (si c'est trop rigide, les gens trichent)

**Zones recommandées** :
| Zone | Flexibilité | Raison |
|------|-------------|--------|
| Backlog (avant engagement) | Libre | Le refinement n'est pas linéaire |
| Engagement (À Faire) | Contrôlée | Point de mesure critique |
| Exécution (En Cours → Terminé) | Libre | Le dev flow varie selon les tickets |

---

### 2.4 Définition claire de chaque statut

**La pratique** : Documenter ce que signifie chaque statut (critères d'entrée et de sortie).

**Pourquoi c'est bien** :
- Évite les interprétations différentes entre membres
- Facilite l'onboarding
- Permet de détecter les tickets mal classés
- Base pour la Definition of Done

**Exemple** :
| Statut | Signification | Critères d'entrée |
|--------|---------------|-------------------|
| À FAIRE | Engagé dans le sprint, prêt à démarrer | Story points estimés, AC définis, pas de bloqueur |
| EN COURS | Quelqu'un travaille activement dessus | Assigné, branche créée |
| CODE REVIEW | Code terminé, en attente de revue | PR ouverte, tests passent |
| TERMINÉ | Livré et validé | Mergé, déployé, PO a validé |

---

### 2.5 Limites de WIP (Work In Progress)

**La pratique** : Limiter le nombre de tickets dans certaines colonnes.

**Pourquoi c'est bien** :
- Force à finir avant de commencer
- Révèle les goulots d'étranglement
- Réduit le context switching
- Améliore le temps de cycle
- Meilleure qualité (focus)

**Recommandations** :
| Colonne | Limite suggérée | Logique |
|---------|-----------------|---------|
| EN COURS | 1-2 par personne | Focus individuel |
| CODE REVIEW | 2-3 par équipe | Éviter l'accumulation |
| À TESTER | 3-5 par équipe | Feedback rapide |

---

### 2.6 Utiliser des flags plutôt que des statuts pour les états temporaires

**La pratique** : Pour "Bloqué" ou "En attente", utiliser un label/flag, pas un statut.

**Pourquoi c'est bien** :
- Le ticket reste dans son vrai statut (EN COURS reste EN COURS)
- Les métriques ne sont pas faussées
- Visibilité du blocage sans polluer le workflow
- Plus facile de tracker les blocages (filtre sur le flag)

**Implémentation** :
1. Créer un label/flag "Bloqué"
2. Ajouter un champ "Raison du blocage"
3. Revue quotidienne des flags en daily
4. Retirer le flag quand débloqué

---

### 2.7 Chemin rapide pour les urgences

**La pratique** : Permettre un fast-track pour les bugs critiques, tout en passant par le point de mesure.

**Pourquoi c'est bien** :
- Les urgences ne sont pas ralenties par le refinement
- Les métriques restent fiables (passage par À FAIRE)
- Équilibre entre réactivité et traçabilité

**Exemple** :
```
Standard :    BACKLOG → À AFFINER → À PRIORISER → À FAIRE → EN COURS
Fast track :  BACKLOG ─────────────────────────→ À FAIRE → EN COURS
              (condition : Priority = Highest)
```

---

### 2.8 Audit régulier du workflow

**La pratique** : Revoir le workflow tous les trimestres.

**Pourquoi c'est bien** :
- Supprime les statuts devenus inutiles
- Détecte les contournements
- Adapte aux évolutions du process
- Maintient la pertinence des métriques

**Questions d'audit** :
- [ ] Y a-t-il des statuts avec < 1h de temps moyen ?
- [ ] Des statuts jamais utilisés ?
- [ ] Des colonnes qui débordent systématiquement ?
- [ ] Les équipes se plaignent-elles de friction ?

---

## 3. Mauvaises pratiques

### 3.1 Trop de statuts (workflow "usine à gaz")

**La pratique** : Créer un statut pour chaque micro-étape du process.

**Pourquoi c'est mauvais** :
- Confusion : "c'est quoi la différence entre À Prioriser et À Planifier ?"
- Tickets mal classés → données faussées
- Temps perdu à déplacer les tickets
- Maintenance complexe
- Nouveaux membres perdus

**Exemple problématique** :
```
BACKLOG → À ANALYSER → À ESTIMER → À PLANIFIER → À PRIORISER → PRÊT → À FAIRE → EN COURS → EN REVUE → EN TEST UNITAIRE → EN TEST INTÉGRATION → EN RECETTE → À DÉPLOYER STAGING → À DÉPLOYER PROD → À VALIDER → TERMINÉ

→ 16 statuts = ingérable
```

**Symptômes** :
- Les gens ne savent plus où mettre leurs tickets
- Beaucoup de tickets dans les mauvaises colonnes
- Personne n'utilise certains statuts

---

### 3.2 Transitions "N'importe lequel" partout

**La pratique** : Permettre d'aller de n'importe quel statut vers n'importe quel autre.

**Pourquoi c'est mauvais** :
- Impossible de mesurer le cycle time correctement
- Tickets qui sautent des étapes critiques
- Pas de garantie que le process est suivi
- Rapports faussés (EazyBI, vélocité)
- Comportements incohérents entre membres

**Exemple** :
```
BACKLOG ───────────────────► EN COURS (sans passer par À FAIRE)
        ───────────────────► TERMINÉ (sans rien entre)
```

**Conséquence concrète** : Un ticket créé puis immédiatement passé en "Terminé" aura un cycle time de 0, faussant toutes les moyennes.

---

### 3.3 Colonnes "parking" / "cimetière à tickets"

**La pratique** : Avoir des statuts comme "En attente", "On Hold", "Suspendu", "Bloqué".

**Pourquoi c'est mauvais** :
- Les tickets y restent des semaines/mois sans que personne ne regarde
- Masque le vrai état du travail (WIP caché)
- Fausse le cycle time (explosion des durées)
- Crée de la dette : tickets obsolètes jamais nettoyés
- Déresponsabilise : "c'est bloqué, pas de ma faute"
- Le backlog "caché" qui grossit sans contrôle

**Symptômes** :
- 30 tickets "En attente" dont personne ne sait ce qu'ils attendent
- Des tickets dans "Bloqué" depuis 6 mois
- Personne n'ose supprimer ces tickets

**Solution** : Voir bonne pratique 2.6 (flags plutôt que statuts).

---

### 3.4 Colonnes qui débordent (goulots d'étranglement)

**La pratique** : Laisser s'accumuler les tickets dans certaines colonnes sans limite.

**Pourquoi c'est mauvais** :
- Travail commencé mais pas terminé = valeur non livrée
- Feedback retardé (la PR attend 5 jours → contexte perdu)
- Merge conflicts qui s'accumulent
- Frustration : "j'ai fini mais ça n'avance pas"
- Cycle time qui explose
- Fausse impression de productivité ("on code beaucoup")

**Exemples problématiques** :
- Code Review avec 15 tickets en attente
- À Tester qui grossit sprint après sprint
- À Valider où le PO ne passe jamais

**Conséquence** : L'équipe a l'impression de travailler beaucoup, mais le throughput (tickets réellement terminés) est faible.

---

### 3.5 Trop de contraintes / workflow "prison"

**La pratique** : Verrouiller toutes les transitions avec des conditions, validators, permissions.

**Pourquoi c'est mauvais** :
- Friction énorme au quotidien
- Les gens contournent (créent des tickets ailleurs, utilisent des workarounds)
- Perte de confiance dans l'outil
- Cas légitimes bloqués par des règles rigides
- Maintenance cauchemardesque
- L'outil devient l'ennemi plutôt que l'allié

**Exemple problématique** :
```
Pour passer en EN COURS :
- Story points obligatoires
- AC obligatoires (min 3)
- Sprint assigné
- Epic linkée
- Composant sélectionné
- Approbation du PO
- ...
```

**Symptôme** : Les gens créent des tickets dans un autre projet avec un workflow plus simple.

---

### 3.6 Colonnes fantômes (statuts inutiles)

**La pratique** : Garder des statuts qui ne sont plus utilisés ou qui ne servent à rien.

**Pourquoi c'est mauvais** :
- Pollution du board
- Confusion pour les nouveaux
- Maintenance inutile
- Parfois des tickets s'y perdent

**Exemples** :
- Statut "À Documenter" jamais utilisé
- Statut "À Déployer" alors que le déploiement est automatique
- Statut créé pour un projet ponctuel il y a 2 ans

---

### 3.7 Pas de Definition of Done pour "Terminé"

**La pratique** : Le statut "Terminé" n'a pas de critères clairs.

**Pourquoi c'est mauvais** :
- Chacun a sa propre interprétation de "terminé"
- Tickets marqués "Terminé" alors que le test n'est pas fait
- Tickets "Terminé" mais pas déployés
- Conflits : "je pensais que c'était fini" vs "non il manque X"
- Retours en arrière fréquents (Terminé → En Cours)

**Conséquence** : Le throughput affiché ne reflète pas la vraie valeur livrée.

---

### 3.8 Workflow différent par équipe sans raison

**La pratique** : Chaque équipe a son propre workflow, incompatible avec les autres.

**Pourquoi c'est mauvais** :
- Impossible de consolider les métriques
- Confusion quand quelqu'un change d'équipe
- Maintenance multipliée
- Difficile de comparer les équipes
- Rapports cross-équipes impossibles

**Exception acceptable** : Des variations mineures justifiées (ex: une équipe avec plus de QA a un statut "À Tester" dédié).

---

### 3.9 Utiliser le workflow pour "punir" ou "contrôler"

**La pratique** : Ajouter des contraintes pour forcer les gens à faire quelque chose qu'ils ne font pas.

**Pourquoi c'est mauvais** :
- Traite le symptôme, pas la cause
- Crée de la frustration et de la défiance
- Les gens trouvent des contournements
- Relation équipe/outil dégradée
- Le vrai problème (culture, compétences, process) n'est pas adressé

**Exemple** : "Les devs ne mettent pas les story points → on bloque la transition sans story points"
→ Résultat : Les devs mettent n'importe quoi pour passer.

**Meilleure approche** : Comprendre pourquoi ils ne le font pas et adresser la cause.

---

## 4. Structure type d'un workflow agile

### 4.1 Les 4 zones du workflow

```
┌─────────────────────────────────────────────────────┐
│                    ZONE BACKLOG                      │
│  Flexibilité : HAUTE                                 │
│  Objectif : Préparer le travail                      │
│                                                      │
│  BACKLOG → À CADRER → À AFFINER → À PRIORISER       │
└─────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────┐
│                  ZONE ENGAGEMENT                     │
│  Flexibilité : CONTRÔLÉE                             │
│  Objectif : Marquer l'engagement (point de mesure)   │
│                                                      │
│                     À FAIRE                          │
└─────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────┐
│                  ZONE EXÉCUTION                      │
│  Flexibilité : HAUTE                                 │
│  Objectif : Réaliser le travail                      │
│                                                      │
│  EN COURS → CODE REVIEW → TEST → DEPLOY → VALIDER   │
└─────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────┐
│                    ZONE DONE                         │
│  Flexibilité : CONTRÔLÉE                             │
│  Objectif : Confirmer la livraison                   │
│                                                      │
│              TERMINÉ    REFUSÉ                       │
└─────────────────────────────────────────────────────┘
```

### 4.2 Points de mesure critiques

| Transition | Métrique impactée | Importance |
|------------|-------------------|------------|
| → À FAIRE | Début du cycle time | **Critique** |
| → EN COURS | Lead time (temps de réaction) | Haute |
| → TERMINÉ | Fin du cycle time, throughput | **Critique** |

---

## 5. Configuration Jira

### 5.1 Concepts clés

| Concept | Description | Usage |
|---------|-------------|-------|
| **Workflow** | Ensemble statuts + transitions | Structure globale |
| **Statut** | État du ticket | Colonne du board |
| **Transition** | Passage d'un statut à un autre | Flèche entre colonnes |
| **Condition** | Règle pour autoriser une transition | Contrôle d'accès |
| **Validator** | Vérifie des champs avant transition | Qualité des données |
| **Post-function** | Action automatique après transition | Automatisation |

### 5.2 Conditions utiles

| Condition | Usage | Exemple |
|-----------|-------|---------|
| Statut précédent | Forcer un passage obligatoire | Seuls les tickets "À Faire" peuvent passer "En Cours" |
| Groupe utilisateur | Réserver des transitions | Seuls les QA peuvent valider |
| Champ rempli | Bloquer si incomplet | Story points requis |

### 5.3 Post-functions utiles

| Post-function | Usage |
|---------------|-------|
| Assigner à l'utilisateur courant | Auto-assignation au démarrage |
| Mettre à jour un champ | Horodater les transitions |
| Envoyer notification | Alerter le PO quand c'est à valider |

---

## 6. Patterns de workflow

### 6.1 Pattern "Point de passage obligatoire"

```
Avant (problème) :
BACKLOG ───────────────► EN COURS
    │                        ▲
    └──► À FAIRE ────────────┘

Après (corrigé) :
BACKLOG ──► À FAIRE ──► EN COURS
    ▲           │
    └───────────┘
```

### 6.2 Pattern "Retour arrière contrôlé"

**Retours autorisés** :
- TERMINÉ → EN COURS (bug découvert)
- À VALIDER → EN COURS (rejeté)
- CODE REVIEW → EN COURS (corrections)

**Retours interdits** :
- TERMINÉ → BACKLOG (créer un nouveau ticket)
- EN COURS → À AFFINER (le ticket était mal préparé)

### 6.3 Pattern "Fast track"

```
Standard :    BACKLOG → À AFFINER → À PRIORISER → À FAIRE → EN COURS
Fast track :  BACKLOG ─────────────────────────→ À FAIRE → EN COURS
```

---

## 7. Métriques et reporting

### 7.1 Métriques dépendant du workflow

| Métrique | Calcul | Dépend de |
|----------|--------|-----------|
| **Cycle Time** | Terminé - À Faire | Passage obligé par À Faire |
| **Lead Time** | Terminé - Création | Horodatage création |
| **Throughput** | Tickets terminés / période | Statut Terminé correct |
| **WIP** | Tickets en cours | Définition des statuts "actifs" |

### 7.2 Configuration EazyBI

1. **Point de départ** : "À Faire" (engagement)
2. **Point d'arrivée** : "Terminé" (avec resolution = Done)
3. **Exclure** : Backlog, Annulé, Refusé

---

## 8. Gouvernance

### 8.1 Qui peut modifier ?

| Rôle | Droits |
|------|--------|
| Admin Jira | Modification technique |
| Product Owner | Proposition de changements |
| Scrum Master / Coach | Analyse des problèmes |
| Équipe | Feedback sur l'usage |

### 8.2 Process de modification

1. Identifier le problème
2. Proposer une solution (schéma avant/après)
3. Valider avec les parties prenantes
4. Tester (projet pilote si possible)
5. Déployer avec communication
6. Mesurer l'amélioration

---

## 9. Checklist

### Création de workflow

- [ ] Statuts limités (< 10) ?
- [ ] Chaque statut a une définition claire ?
- [ ] Point d'engagement (À Faire) protégé ?
- [ ] Métriques clés mesurables ?
- [ ] Chemin pour les urgences ?

### Audit trimestriel

- [ ] Statuts jamais utilisés → supprimer
- [ ] Colonnes qui débordent → limites WIP
- [ ] Contournements → simplifier
- [ ] Métriques cohérentes ?

---

## 10. Ressources

- [Introduction to Jira Workflows - Atlassian](https://www.atlassian.com/software/jira/guides/workflows/overview)
- [Configure advanced workflows - Atlassian](https://support.atlassian.com/jira-cloud-administration/docs/configure-advanced-issue-workflows/)
- [Jira Workflow Best Practices - TitanApps](https://titanapps.io/blog/jira-workflow/)

---

*Framework créé le 9 février 2026 — V1.1 avec bonnes/mauvaises pratiques détaillées*
