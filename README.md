# HiloAssistant

> Modèle — à faire réviser par un conseiller juridique avant usage contractuel.

**Éditeur :** Hilo Tech inc. (Longueuil, Québec, Canada)
**Marque blanche :** disponible sous le nom « <Nom> Local Assistant »
**Support :** support@hilointelligence.ca
**Hub :** hilointelligence.ca

---

## 1. Présentation

HiloAssistant est une application macOS de productivité résidant dans la barre de menu. Elle offre des actions IA sur texte sélectionné (correction, reformulation, traduction, etc.), un gestionnaire d'historique de presse-papiers, un coffre de renseignements personnels chiffré, la capture d'écran/vidéo avec annotation et OCR, la dictée vocale, ainsi que l'enregistrement de procédures (clics + narration).

L'application est distribuée en dehors du Mac App Store. Ce dépôt public contient les binaires signés destinés aux services TI qui souhaitent auditer, déployer ou gérer HiloAssistant en environnement d'entreprise.

## 2. Prérequis

- **macOS** : version récente supportée par Apple (Apple Silicon et Intel).
- **Compte HiloIntelligence actif, palier « Plan 3 »** : l'application est **inutilisable sans connexion**. L'authentification se fait par OAuth 2.0 / OIDC avec PKCE via le fournisseur HiloIntelligence. Sans jeton valide, un verrou bloquant empêche l'accès à toute fonctionnalité.
- Si l'accès HiloIntelligence de l'utilisateur est révoqué ou expire, l'application se reverrouille automatiquement (règle de révocation) — aucune donnée locale ne demeure accessible tant que la session n'est pas rétablie.

## 3. Ce que l'application fait — et ne fait pas

**Fait :**
- Actions IA sur texte sélectionné, dictée vers texte, résumé/reformulation de captures d'écran.
- Historique de presse-papiers local avec masquage automatique des éléments sensibles.
- Coffre de renseignements personnels chiffré localement (adresses, notes, cartes — structure, sans CVV).
- Capture d'écran et vidéo avec annotation, OCR sur l'appareil.
- Enregistrement de procédures (clics + narration), avec synthèse optionnelle par IA.
- Mise à jour automatique via un mécanisme intégré (appcast).

**Ne fait pas :**
- **Aucune gestion de mots de passe.** Cette fonctionnalité a été retirée du produit.
- Aucun suivi publicitaire, aucun SDK d'analytique tiers.
- Aucune transmission de contenu sans action explicite de l'utilisateur (voir section 5).
- Aucune fonctionnalité utilisable hors ligne sans compte HiloIntelligence — ce n'est pas un outil autonome.

## 4. Permissions macOS requises

L'application demande les autorisations système suivantes. Chacune est optionnelle au sens où macOS peut la refuser, mais la fonctionnalité associée cesse alors de fonctionner.

| Permission | Pourquoi elle est requise |
|---|---|
| **Accessibilité** | Lire le texte sélectionné dans l'application active pour les actions IA, et écrire le résultat (remplacement) à l'endroit du curseur. Requis également pour l'enregistrement de procédures (détection des clics via libellés d'accessibilité) et le remplissage automatique de formulaires. |
| **Enregistrement de l'écran** | Requis par macOS pour toute capture d'image ou de vidéo de l'écran (captures ponctuelles, enregistrements de zone/plein écran). |
| **Micro** | Requis pour la fonction de dictée (reconnaissance vocale) et pour la capture audio lors d'un enregistrement vidéo, si l'utilisateur active le son. |

Aucune autre permission système (contacts, localisation, calendrier, etc.) n'est demandée.

## 5. Confidentialité en bref

Résumé technique à l'intention des équipes TI. Le détail complet, incluant les bases légales et les droits des personnes concernées, se trouve dans la politique de confidentialité (lien section 9).

**Reste localement sur l'appareil :**
- Historique du presse-papiers (SQLite local, éléments sensibles masqués).
- Coffre de renseignements personnels (chiffrement AES-GCM via CryptoKit).
- Captures d'écran, vidéos et résultat de l'OCR (traitement sur l'appareil, framework Vision d'Apple).
- Jeton d'accès OAuth (mémoire uniquement) ; jeton de rafraîchissement (Trousseau macOS).

**Transmis à la passerelle HiloIntelligence (ai.\<domaine\>.ca, via TLS), uniquement sur action explicite de l'utilisateur :**
- Texte sélectionné soumis à une action IA.
- Image ou texte issu d'une capture, si une action IA est déclenchée dessus.
- Contenu de procédures, lors de la synthèse par IA.
- Audio de dictée, uniquement si l'utilisateur active l'option infonuagique (opt-in) ; sinon la reconnaissance se fait entièrement sur l'appareil.

Ces transmissions sont authentifiées par le jeton OAuth de l'utilisateur. Un mode « Sans rétention » ajoute un en-tête `X-Hilo-Privacy: no-retention` aux requêtes (comportement de rétention à confirmer côté passerelle).

**Sous-traitants / tiers impliqués :** HiloIntelligence (passerelle IA) et le fournisseur de modèle de langage opérant derrière la passerelle ; frameworks Apple (système d'exploitation). Aucun SDK publicitaire ou d'analytique tiers.

**Télémétrie :** opt-in uniquement. Si activée, seules des métadonnées d'erreur ou de plantage sont envoyées (jamais le contenu des données de l'utilisateur), actuellement par courriel de rapport à support@hilointelligence.ca.

**Identité de compte :** identifiant stable (UUID « sub »), courriel et nom, fournis par HiloIntelligence lors de l'authentification.

**Juridiction :** traitement conforme en priorité à la *Loi sur la protection des renseignements personnels dans le secteur privé* (Loi 25, Québec) ; compatibilité avec le RGPD prévue pour la clientèle entreprise.

## 6. Installation

1. Télécharger le fichier `.dmg` (ou `.pkg`, selon la version publiée) le plus récent depuis la section [Releases](../../releases) de ce dépôt.
2. Glisser l'application dans `/Applications`.
3. Au premier lancement, l'assistant d'accueil guide l'utilisateur à travers l'octroi des permissions (section 4) et la connexion au compte HiloIntelligence de son organisation.

## 7. Mises à jour

HiloAssistant intègre un mécanisme de vérification et de notification de mise à jour basé sur un fichier de manifeste (appcast), sans dépendance externe. L'application vérifie périodiquement la disponibilité d'une nouvelle version publiée dans ce dépôt et notifie l'utilisateur ; l'installation demeure sous le contrôle de ce dernier (ou du parc géré, voir section 8).

## 8. Déploiement de masse (MDM)

Pour un déploiement en parc, un fichier `managed-config.json` permet de préconfigurer certains paramètres (par exemple le domaine HiloIntelligence de l'organisation) via une solution de gestion des appareils mobiles (MDM). Les équipes TI souhaitant déployer HiloAssistant à l'échelle de l'organisation, et pousser les permissions d'Accessibilité, d'Enregistrement de l'écran et de Micro via un profil de configuration, sont invitées à contacter support@hilointelligence.ca pour la documentation de déploiement détaillée et le gabarit `managed-config.json`.

## 9. Vérification d'intégrité

Les binaires publiés dans ce dépôt sont destinés à être signés avec un certificat **Developer ID** Apple (notarisation incluse) — cette étape est **à venir**. En attendant, les équipes TI qui souhaitent auditer un binaire avant déploiement sont invitées à contacter support@hilointelligence.ca pour obtenir les sommes de contrôle (checksums) de la version publiée.

## 10. Support

Pour toute question technique, tout signalement d'incident ou toute demande relative à la confidentialité des données : **support@hilointelligence.ca**

## 11. Documents légaux

- Politique de confidentialité : [DATE — lien à insérer]
- Conditions générales d'utilisation (CGU) : [DATE — lien à insérer]
- Contrat de licence utilisateur final (CLUF) : [DATE — lien à insérer]