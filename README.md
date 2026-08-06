# HiloAssistant

> Modèle — à faire réviser par un conseiller juridique avant usage contractuel.

**Éditeur :** Hilo Tech inc. (Longueuil, Québec, Canada)
**Marque blanche :** disponible sous le nom « <Nom> Local Assistant »
**Support :** support@hilointelligence.ca
**Hub :** hilointelligence.ca

---

## 1. Présentation

HiloAssistant est une application de productivité résidant dans la **barre de menus** (macOS) ou dans la **zone de notification** (Windows). Elle offre des actions IA sur texte sélectionné (correction, reformulation, traduction, etc.), un gestionnaire d'historique de presse-papiers, un coffre de renseignements personnels chiffré, la capture d'écran/vidéo avec annotation et OCR, la dictée vocale, ainsi que l'enregistrement de procédures (clics + narration).

L'application est distribuée en dehors du Mac App Store et en dehors du Microsoft Store. Ce dépôt public contient les binaires signés destinés aux services TI qui souhaitent auditer, déployer ou gérer HiloAssistant en environnement d'entreprise.

**Les deux plateformes sont publiées ici, sous des étiquettes distinctes :** les versions macOS portent une étiquette `v…` (ex. `v0.5.3`), les versions Windows une étiquette `win-…` (ex. `win-v0.9.14.93`). La version Windows est actuellement en **alpha** et publiée comme *pre-release*.

## 2. Prérequis

- **macOS** : version récente supportée par Apple (Apple Silicon et Intel).
- **Windows** : Windows 10 version 1809 (build 17763) ou Windows 11, **64 bits**. Droits administrateur requis **une seule fois**, à l'installation, pour approuver le certificat de signature (voir section 9). Aucun runtime à installer : tout ce dont l'application a besoin est inclus dans le paquet.
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

## 4. Permissions requises

### 4.1 macOS

L'application demande les autorisations système suivantes. Chacune est optionnelle au sens où macOS peut la refuser, mais la fonctionnalité associée cesse alors de fonctionner.

| Permission | Pourquoi elle est requise |
|---|---|
| **Accessibilité** | Lire le texte sélectionné dans l'application active pour les actions IA, et écrire le résultat (remplacement) à l'endroit du curseur. Requis également pour l'enregistrement de procédures (détection des clics via libellés d'accessibilité) et le remplissage automatique de formulaires. |
| **Enregistrement de l'écran** | Requis par macOS pour toute capture d'image ou de vidéo de l'écran (captures ponctuelles, enregistrements de zone/plein écran). |
| **Micro** | Requis pour la fonction de dictée (reconnaissance vocale) et pour la capture audio lors d'un enregistrement vidéo, si l'utilisateur active le son. |

Aucune autre permission système (contacts, localisation, calendrier, etc.) n'est demandée.

### 4.2 Windows

Le modèle d'autorisation de Windows diffère de celui de macOS, et la différence est importante pour une évaluation TI : **seul le micro fait l'objet d'un consentement explicite**. La lecture du texte sélectionné et la capture d'écran ne sont pas, sous Windows, protégées par une autorisation système distincte.

| Capacité déclarée par le paquet | Ce qu'elle permet |
|---|---|
| **`microphone`** | Dictée et capture du son lors d'un enregistrement vidéo. Soumis au réglage *Confidentialité et sécurité → Microphone* de Windows ; l'utilisateur peut le révoquer à tout moment, et la fonction cesse alors de fonctionner. |
| **`internetClient`** | Appels à la passerelle HiloIntelligence (authentification, actions IA, transcription). |
| **`runFullTrust`** | Application de bureau classique empaquetée en MSIX (interopérabilité Win32 : raccourcis globaux, automatisation d'interface, capture). |
| **`unvirtualizedResources`** | Écriture des données de l'utilisateur dans les vrais dossiers `%LOCALAPPDATA%` / `%APPDATA%`, plutôt que dans le stockage virtualisé du paquet, afin que réglages et données survivent aux mises à jour. |

**Sans invite système :**
- **Lecture du texte sélectionné et écriture du remplacement** : via l'API d'automatisation d'interface de Windows (UI Automation) et le presse-papiers. Windows ne demande aucun consentement pour cela — contrairement à l'autorisation d'Accessibilité de macOS.
- **Capture d'écran et d'écran vidéo** : via l'API de capture graphique de Windows. Aucune invite d'autorisation ; l'utilisateur voit toutefois l'indicateur de capture du système pendant un enregistrement.

Aucune autre capacité restreinte n'est déclarée (ni localisation, ni contacts, ni calendrier, ni caméra).

## 5. Confidentialité en bref

Résumé technique à l'intention des équipes TI. Le détail complet, incluant les bases légales et les droits des personnes concernées, se trouve dans la politique de confidentialité (lien section 11).

**Reste localement sur l'appareil :**

| Donnée | macOS | Windows |
|---|---|---|
| Historique du presse-papiers | SQLite local, éléments sensibles masqués | Identique (SQLite local) |
| Coffre de renseignements personnels | Chiffrement AES-256-GCM (CryptoKit) | Chiffrement AES-256-GCM identique au format d'octets près ; la clé maîtresse réside dans le **Gestionnaire d'informations d'identification**, protégée par DPAPI dans la portée du compte Windows de l'utilisateur |
| Captures d'écran, vidéos, résultat de l'OCR | Traitement sur l'appareil (framework Vision) | Traitement sur l'appareil (moteur OCR intégré à Windows). Requiert un module de langue installé ; à défaut, l'OCR est signalé comme indisponible plutôt que d'envoyer l'image ailleurs |
| Jeton d'accès OAuth | Mémoire uniquement | Mémoire uniquement |
| Jeton de rafraîchissement | Trousseau macOS | Gestionnaire d'informations d'identification |

**Transmis à la passerelle HiloIntelligence (ai.\<domaine\>.ca, via TLS), uniquement sur action explicite de l'utilisateur :**
- Texte sélectionné soumis à une action IA.
- Image ou texte issu d'une capture, si une action IA est déclenchée dessus.
- Contenu de procédures, lors de la synthèse par IA.
- Audio, dans les conditions décrites ci-dessous — elles **diffèrent selon la plateforme**.

> **Différence de plateforme à ne pas manquer — traitement de la voix.**
> Sur **macOS**, la dictée se fait entièrement **sur l'appareil** ; l'audio n'est transmis que si l'utilisateur active explicitement l'option infonuagique (opt-in).
> Sur **Windows**, il n'existe **aucun moteur vocal local** : la dictée, la transcription de la narration des procédures et celle de la piste audio des vidéos passent **toutes** par la passerelle HiloIntelligence. Déclencher l'une de ces fonctions **transmet l'audio**, sans exception et sans repli hors ligne. Les fonctions vocales sont par conséquent inutilisables sans connexion. Aucune transcription n'a lieu tant que l'utilisateur ne déclenche pas la fonction, et l'application l'annonce dans ses réglages ; il n'y a pas d'écoute d'arrière-plan.

Ces transmissions sont authentifiées par le jeton OAuth de l'utilisateur. Un mode « Sans rétention » ajoute un en-tête `X-Hilo-Privacy: no-retention` aux requêtes (comportement de rétention à confirmer côté passerelle).

**Sous-traitants / tiers impliqués :** HiloIntelligence (passerelle IA) et le fournisseur de modèle de langage opérant derrière la passerelle ; frameworks du système d'exploitation (Apple ou Microsoft selon la plateforme). Aucun SDK publicitaire ou d'analytique tiers.

**Télémétrie :** opt-in uniquement. Si activée, seules des métadonnées d'erreur ou de plantage sont envoyées (jamais le contenu des données de l'utilisateur), actuellement par courriel de rapport à support@hilointelligence.ca.

**Identité de compte :** identifiant stable (UUID « sub »), courriel et nom, fournis par HiloIntelligence lors de l'authentification.

**Juridiction :** traitement conforme en priorité à la *Loi sur la protection des renseignements personnels dans le secteur privé* (Loi 25, Québec) ; compatibilité avec le RGPD prévue pour la clientèle entreprise.

## 6. Installation

Les versions des deux plateformes cohabitent dans la section [Releases](../../releases) : étiquettes `v…` pour macOS, `win-…` pour Windows.

### 6.1 macOS

1. Télécharger le fichier `.dmg` (ou `.pkg`, selon la version publiée) le plus récent.
2. Glisser l'application dans `/Applications`.
3. Au premier lancement, l'assistant d'accueil guide l'utilisateur à travers l'octroi des permissions (section 4.1) et la connexion au compte HiloIntelligence de son organisation.

### 6.2 Windows

Prendre la version `win-…` la plus récente et y télécharger **`HiloAssistant-<version>-installer.zip`** — le kit de première installation.

1. **Extraire l'archive** (clic droit → *Extraire tout…*) **avant de lancer quoi que ce soit.** Windows marque les fichiers provenant d'une archive téléchargée, et PowerShell refuse alors le script **sans afficher le moindre message**. C'est la cause la plus fréquente des « il ne se passe rien ».
2. Double-cliquer **`Installer.cmd`**, puis choisir *Exécuter* si Windows affiche un avertissement.
3. Accepter l'invite de **contrôle de compte d'utilisateur** : l'installateur se relance avec les privilèges nécessaires.
4. L'installateur enchaîne trois étapes — approbation du certificat, installation du paquet, vérification de la version installée.
5. Lancer **HiloAssistant** depuis le menu Démarrer. L'application est **résidente dans la zone de notification** : aucune fenêtre ne s'ouvre au démarrage, et l'icône se trouve souvent dans le menu des icônes masquées (le chevron `^`).

Le fichier `.msix` publié **à côté** du kit sert aux mises à jour automatiques d'une installation existante ; il n'est pas prévu pour une première installation, le certificat n'étant alors pas encore approuvé.

## 7. Mises à jour

HiloAssistant intègre un mécanisme de vérification de mise à jour fondé sur un fichier de manifeste (appcast), sans dépendance externe. **Chaque plateforme lit son propre manifeste** : `appcast.json` pour macOS, `appcast-windows.json` pour Windows. Les deux fichiers vivent à la racine de ce dépôt et ne sont pas interchangeables.

- **macOS** : l'application vérifie la disponibilité d'une nouvelle version et notifie l'utilisateur ; l'installation demeure sous son contrôle (ou celui du parc géré, section 8).
- **Windows** : l'application vérifie au lancement, télécharge le paquet, contrôle son empreinte SHA-256 **et l'identité du paquet**, puis installe par-dessus la version en place et redémarre. L'autorisation micro et les données de l'utilisateur sont conservées : la mise à jour ne désinstalle jamais l'application.

## 8. Déploiement de masse (MDM)

Pour un déploiement en parc, un fichier `managed-config.json` permet de préconfigurer certains paramètres (par exemple le domaine HiloIntelligence de l'organisation). Il est appliqué automatiquement au lancement, sans intervention de l'utilisateur.

| Plateforme | Emplacements lus (dans l'ordre) |
|---|---|
| macOS | `/Library/Application Support/HiloAssistant/managed-config.json` |
| Windows | `%ProgramData%\HiloAssistant\managed-config.json` (portée machine), puis `%APPDATA%\HiloAssistant\managed-config.json` (portée utilisateur) |

Les équipes TI souhaitant déployer HiloAssistant à l'échelle de l'organisation — et, sous macOS, pousser les permissions d'Accessibilité, d'Enregistrement de l'écran et de Micro via un profil de configuration — sont invitées à contacter support@hilointelligence.ca pour la documentation de déploiement détaillée et le gabarit `managed-config.json`.

## 9. Vérification d'intégrité

### 9.1 macOS

Les binaires publiés dans ce dépôt sont destinés à être signés avec un certificat **Developer ID** Apple (notarisation incluse) — cette étape est **à venir**. En attendant, les équipes TI qui souhaitent auditer un binaire avant déploiement sont invitées à contacter support@hilointelligence.ca pour obtenir les sommes de contrôle (checksums) de la version publiée.

### 9.2 Windows

Les paquets Windows sont signés avec un **certificat auto-signé** de Hilo Tech inc. — et non avec un certificat émis par une autorité de certification publique. C'est la raison pour laquelle la première installation demande des droits administrateur : elle dépose ce certificat dans le magasin des *personnes de confiance* de la machine.

| | |
|---|---|
| Sujet du certificat | `CN=HiloTech` |
| Empreinte SHA-1 | `43ABD11D56DF3827A9FA21632D43497FE3F60F56` |

Une équipe TI peut vérifier une livraison sans l'installer :

```powershell
# empreinte du certificat qui a signé le paquet
(Get-AuthenticodeSignature .\HiloAssistant-<version>.msix).SignerCertificate.Thumbprint

# empreinte du contenu, à comparer au champ « sha256 » de appcast-windows.json
(Get-FileHash .\HiloAssistant-<version>.msix -Algorithm SHA256).Hash
```

Le manifeste `appcast-windows.json` publie la somme SHA-256 et la taille exacte de chaque paquet ; c'est cette valeur que l'application contrôle avant d'installer une mise à jour. Le certificat de signature reste **stable d'une version à l'autre** : un paquet dont l'éditeur aurait changé est refusé par le mécanisme de mise à jour.

## 10. Support

Pour toute question technique, tout signalement d'incident ou toute demande relative à la confidentialité des données : **support@hilointelligence.ca**

## 11. Documents légaux

- Politique de confidentialité : [DATE — lien à insérer]
- Conditions générales d'utilisation (CGU) : [DATE — lien à insérer]
- Contrat de licence utilisateur final (CLUF) : [DATE — lien à insérer]
