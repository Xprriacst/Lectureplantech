# Proposition — Outil d'automatisation des devis

**Client** : Menuiserie Pro (Nicolas Autric)
**Auteur** : Alexandre Hebert
**Date** : Mai 2026
**Statut** : Version 1 — pour discussion

---

## 1. Synthèse

Nous proposons de concevoir, développer et déployer un outil web qui automatise la production des devis menuiserie : à partir d'une **dictée vocale** par le chargé d'affaires, l'IA reconstitue la "Moulinette" Excel actuelle (Calcul de surfaces + Calcul de devis avec formules), réduisant le temps de saisie d'environ **30 minutes à moins de 5 minutes** par devis.

Le projet est découpé en **3 phases avec jalons de validation**, chacune livrant une valeur autonome. Vous pouvez vous arrêter après n'importe quelle phase sans perdre l'investissement précédent.

| Phase | Livrable principal | Durée | Budget |
|---|---|---|---|
| **1 — MVP Voix → Excel** | App web : dictée vocale + saisie manuelle + export .xlsx | 4–6 sem | **7 000 €** |
| **2 — Industrialisation** | Lecture auto du plan (OCR) + multi-utilisateur + calibration heures (RAG sur historique) | 5–7 sem | 7 500 € |
| **3 — Intégration Odoo** | Devis envoyé bout-en-bout + signature électronique | 3–4 sem | 5 000 € |
| | | **Total** | **19 500 €** |

> **Décision de phase** : à la fin de chaque phase, un procès-verbal de recette acte la livraison et déclenche (ou non) le démarrage de la suivante. Vous n'êtes engagé contractuellement que sur la phase en cours.

---

## 2. Pourquoi cet outil

### 2.1 Le contexte que vous nous avez partagé

- Le calcul d'un devis prend actuellement **30 min à 1 h** de saisie manuelle dans la Moulinette + le Calcul de devis Excel, principalement à cause de la décomposition par matériau et de l'ajustement des formules à chaque chantier.
- L'exercice 2024 a généré une **perte de ~180 k€**, identifiée comme due à un sous-chargement systématique des heures de main-d'œuvre (pas un problème de prix client). Aucun outil ne permet aujourd'hui d'alerter sur cet écart.
- Le taux de gain sur devis est de **50 %+**, ce qui signifie que chaque heure gagnée sur la production de devis se transforme directement en capacité commerciale supplémentaire.
- Une migration vers Odoo est en cours, ce qui ouvre la possibilité d'une intégration native devis → facture → relance.

### 2.2 Notre lecture

Le sujet n'est pas seulement d'**accélérer** la saisie. C'est aussi d'**éviter la prochaine perte de 180 k€** en outillant la décision d'estimation des heures. La Phase 2 du projet (calibration historique) est ce qui rentabilise l'outil sur l'année.

### 2.3 Notre principe directeur

L'expertise menuiserie reste celle de Nicolas et de l'équipe. L'outil ne remplace pas le savoir : il en **réduit la friction d'application** et capitalise l'historique pour aider les nouveaux entrants.

---

## 3. Phase 1 — MVP Voix → Excel *(4–6 semaines — 7 000 €)*

### Objectif
Mettre entre les mains de Nicolas (et d'un chargé d'affaires) un outil utilisable au quotidien, qui produit un fichier Excel exploitable directement.

### Périmètre fonctionnel

**Écran 1 — Téléversement**
- Upload du plan PDF (stocké comme document de référence visuel)
- Upload optionnel d'un CCTP
- **Pas d'OCR / pas d'analyse automatique du plan dans cette phase**

**Écran 2 — Structure du projet**
- Liste vide au démarrage avec 2 actions :
  - **+ Ajouter un ensemble** (manuel) : saisie du code, titre, description, dimensions L×P×H, quantité
  - **✨ Générer la structure par IA** : à partir d'une courte description textuelle ou vocale du projet (« 7 meubles : un banc d'entrée, un meuble TV… »), l'IA propose une liste de MI 01…N avec dimensions estimées. Tout reste modifiable.

**Écran 3 — Saisie par ensemble (cœur de l'outil)**
- Vue côte à côte : plan PDF à gauche / saisie à droite
- Pour chaque ensemble :
  - Bouton **✨ Estimer par IA** : génère une première estimation (pièces, matières, heures) modifiable
  - Bouton **🎙️ Décrire ce meuble** : dictée vocale qui parse en temps réel et remplit la Moulinette (pièces, dimensions L×l, matériaux)
  - Toutes les cellules sont **éditables manuellement** à tout moment
  - Indicateurs de confiance par cellule : 🟢 saisie, 🔵 vocal, 🟠 à vérifier
  - Sliders d'heures BE / Fab / Pose
  - Total estimé HT margé calculé en temps réel

**Écran 4 — Synthèse et export**
- Tableau récapitulatif par ensemble (matière, MO, SST, marge, total)
- KPI globaux : sous-total HT, marge globale, total marché +10 %
- Panneau "Paramètres" éditable (taux horaires, coefs)
- **Bouton "Télécharger Excel"** : génère un .xlsx **strictement compatible** avec votre Moulinette actuelle, formules préservées

### Sous-jalons (rendez-vous bimensuels)
- **S1** — atelier de démarrage : audit de 3 devis historiques, glossaire métier figé (Tr, po, Mt, Ban, ray, fileur…), spec de la base de connaissances (prix matières, taux horaires, coefs)
- **S2** — pipeline dictée + parsing LLM + base de connaissances v1, **collecte des embeddings dans Supabase** (préparation RAG Phase 2)
- **S3** — UI de saisie, export Excel fidèle au modèle existant
- **S4** — bouton "✨ Générer / Estimer par IA" + panneau paramètres éditable
- **S5** — tests sur 5 devis réels avec Nicolas et un chargé d'affaires
- **S6** — recette, ajustements, mise en production

### Livrables
- Application web hébergée (Netlify ou Vercel), accès protégé par mot de passe
- Documentation utilisateur (5 pages)
- Code source livré (vous restez propriétaire)
- Export .xlsx 100 % compatible avec votre modèle actuel

### Critère de validation (gate)
> Sur **5 devis réels** tirés au hasard :
> - Temps de saisie moyen < **15 min** (vs ~30–60 min actuellement)
> - Écart de prix final < **8 %** vs version manuelle d'un chargé d'affaires
> - Fichier Excel exportable directement utilisable par le chargé d'affaires en aval

### Hors périmètre
- Lecture automatique du plan (OCR) → Phase 2
- Comptes utilisateurs multiples → Phase 2
- Historique des devis et calibration → Phase 2
- Intégration Odoo → Phase 3

### Coûts récurrents prévus
- Hébergement : ~25 €/mois
- API IA (LLM + transcription voix) : ~1–3 € par devis. Pour 30 devis/mois → ~60–90 €/mois
- **Total ~85–115 €/mois**. Refacturable au coût ou inclus dans un plafond mensuel forfaitaire (à discuter).

---

## 4. Phase 2 — Industrialisation *(5–7 semaines — 7 500 €)*

### Objectif
Passer d'un outil utilisé par 1 ou 2 personnes à un outil partagé par l'équipe, et **adresser le problème de fond** (sous-chargement des heures).

### Périmètre fonctionnel
- **Lecture automatique du plan** (OCR/vision) : détection des références MI, extraction des cotes, pré-remplissage de la structure des ensembles
- **Comptes utilisateurs** (Nicolas + chargés d'affaires) avec historique personnel
- **Calibration heures vs historique** : l'app propose les heures BE / Fab / Pose en se basant sur les meubles similaires déjà chiffrés, et **alerte en rouge** si l'estimation est sous la moyenne — c'est ce mécanisme qui aurait évité la perte 2024
- **Versioning des devis** : V1 envoyée, V2 négociation, V3 signée
- **Tableau de bord** : devis en cours, taux de gain, marge moyenne par typologie
- Gestion fine des cas particuliers (matériau facturé au panneau entier, sous-traitance complexe, déplacements hors région)
- Restructuration de la Moulinette si nécessaire (à co-construire avec vous pour gagner en précision)

### Livrables
- App avec authentification, 3 comptes opérationnels minimum
- Module de calibration des heures + alerte sous-chargement
- Import des 30 derniers devis pour amorcer la base historique
- Tableau de bord de pilotage

### Critère de validation (gate)
> Les 3 chargés d'affaires ont chacun produit **3 devis** sans aide externe, et l'alerte sous-chargement s'est déclenchée sur au moins 1 cas avec correction effective.

---

## 5. Phase 3 — Intégration Odoo + email signé *(3–4 semaines — 5 000 €)*

### Objectif
Boucler le cycle : du plan reçu à la signature électronique du client, sans rupture manuelle.

### Périmètre fonctionnel
- Bouton "Envoyer vers Odoo" → création du devis dans votre système comptable
- Génération PDF du devis avec votre charte (depuis Odoo ou interne)
- Envoi automatique au client par email avec lien de signature électronique (Yousign ou DocuSeal)
- Webhook retour : devis signé → notification dans l'app + mise à jour Odoo

### Livrables
- Connecteur Odoo opérationnel
- Workflow d'envoi automatisé
- Signature électronique testée sur 3 devis réels
- Documentation des mappings (article, client, taxe)

### Critère de validation (gate)
> Un devis complet est envoyé au client en **moins de 5 min** depuis l'upload du plan, signé électroniquement sans relance manuelle.

---

## 6. Sujet annexe — Réconciliation BL / Factures

Vous avez évoqué un besoin parallèle d'automatiser la réconciliation des bons de livraison et des factures (devis concurrent à 5 000 € + 200 €/mois jugé excessif).

**Notre proposition** : traitons-le séparément après la Phase 1, sur un mini-projet de **1 semaine de cadrage + 2–3 semaines de développement**, budget estimé **3 500–5 000 € one-shot** (pas d'abonnement). Périmètre à figer après cadrage. *À sortir du présent devis.*

---

## 7. Approche méthodologique

### 7.1 Rythme et gouvernance

| Cadence | Format | Participants |
|---|---|---|
| Hebdomadaire | Point d'avancement 30 min | Nicolas + Alexandre + 1 ingénieur |
| Bimensuelle | Démo + recette intermédiaire | + 1 chargé d'affaires |
| Fin de phase | Recette + PV signé | Toutes parties prenantes |

### 7.2 Données à fournir par vos soins

Pour démarrer Phase 1 efficacement :
- 30 devis historiques (Moulinette + Calcul + plan PDF + version envoyée)
- Liste fournisseurs matières + grille de prix actuelle
- Liste sous-traitants + grille de tarifs
- 2–3 CCTP types
- Accès à un chargé d'affaires (~2 h/sem pour les tests)

### 7.3 Stack technique envisagée

| Brique | Choix | Raison |
|---|---|---|
| Front | React + TypeScript + Tailwind | Déjà éprouvé sur le proto, écosystème mature |
| Back | Node.js (functions serverless, ~5 endpoints) | Pas d'infra à maintenir, scale auto |
| **Base de données + auth + vecteurs** | **Supabase** (PostgreSQL + `pgvector` + Auth + Storage) | EU (Frankfurt), tout-en-un, code 100 % portable (SQL standard), ~25 €/mois en pro |
| LLM (parsing & génération) | Claude Sonnet 4 (API Anthropic) | Précision technique, latence faible, conformité EU |
| Transcription vocale | Whisper (API) | Standard du marché, support français excellent |
| Hébergement front + edge | Vercel ou Netlify (région EU) | Déploiement automatique sur chaque commit |
| Export Excel | librairie `exceljs` | Préservation native des formules de votre Moulinette |

**Point d'attention sur l'amélioration continue (RAG)**

Le module de **calibration heures** de la Phase 2 repose sur un mécanisme RAG (*Retrieval-Augmented Generation*) :

1. À chaque devis validé, on stocke dans Supabase l'embedding vectoriel du meuble (description + matériaux + dimensions) et la Moulinette validée correspondante.
2. Pour un nouveau meuble, on récupère les **K meubles passés les plus similaires** (cosine similarity sur `pgvector`).
3. On injecte ces exemples dans le prompt du LLM comme références few-shot.
4. Le modèle produit alors une estimation **calibrée sur votre historique réel**, pas sur des moyennes génériques.

→ Concrètement : plus vous utilisez l'outil, plus il propose des heures BE / Fab / Pose proches de ce que vous facturez vraiment. C'est ce mécanisme qui adresse la perte 180 k€.

La **collecte des embeddings démarre dès la Phase 1** (coût négligeable, ~5 ms par devis) ; la **récupération RAG s'active en Phase 2** quand l'historique atteint un seuil de pertinence (~30 devis).

### 7.4 Propriété intellectuelle

Le **code source est livré et reste votre propriété** à l'issue de chaque phase. Vous pouvez à tout moment reprendre la maintenance en interne ou changer de prestataire. Pas d'enfermement technique.

### 7.5 Hébergement et données

- Vos données restent hébergées en Europe (Netlify EU + AWS Frankfurt)
- Aucune donnée client n'est envoyée à l'IA en clair sans transformation
- Vous restez seul propriétaire des données ; aucune réutilisation pour entraînement

---

## 8. Modalités contractuelles

### 8.1 Calendrier de paiement

Par phase :
- 30 % à la commande
- 70 % à la recette validée (PV signé)

### 8.2 Engagements

- **Notre engagement** : livraison conforme au périmètre figé en début de phase, dans les délais annoncés (+/- 1 semaine), avec recette objective.
- **Votre engagement** : disponibilité du sponsor (Nicolas) et d'un référent utilisateur pendant les sprints, fourniture des données dans les délais convenus.

### 8.3 Maintenance après Phase 3

Optionnel : **forfait de maintenance 400 €/mois** comprenant
- Hébergement + monitoring
- Correctifs de bugs sous 5 jours ouvrés
- 1 h de support utilisateur par mois
- Mise à jour des modèles IA (1×/an)

Pas obligatoire ; vous pouvez reprendre la main en interne.

---

## 9. Comparaison concurrentielle

Pour information, sur la base des éléments que vous nous avez communiqués :

| Prestataire | Périmètre | Budget |
|---|---|---|
| Concurrent A (lecture plan) | Lecture plan + extraction Excel | 3 500 € |
| Concurrent A (Odoo) | Lien Odoo + rédaction | 3 000 € |
| **Concurrent A — Total** | | **6 500 €** |
| Concurrent B (réconciliation) | Réconciliation BL ↔ factures | 5 000 € + 200 €/mois |
| **Notre proposition (Phases 1 à 3)** | MVP voix + industrialisation + Odoo | **19 500 €** |

**Différences de fond** :
1. Nous traitons la **dictée vocale** (cœur du gain de temps), pas seulement la lecture de plan
2. La Phase 2 inclut le module **calibration heures** qui adresse votre vraie perte (180 k€)
3. **Code source livré + zéro vendor lock-in**
4. Recette objective par jalon : sortie possible après chaque phase

---

## 10. Prochaines étapes

1. **Validation de cette proposition** (Phase 1 uniquement engageante à ce stade)
2. **Signature du bon de commande Phase 1** (7 000 € — paiement 30 % à la commande, 70 % à la recette)
3. **Atelier de démarrage** dans les 2 semaines (audit de 3 devis, glossaire, base de connaissances)
4. **Démos bimensuelles** + recette en fin de Phase 1 (~6 semaines)
5. **Décision Go/No-go Phase 2** à la livraison de la Phase 1

Je reste à votre disposition pour toute question.

— Alexandre Hebert
