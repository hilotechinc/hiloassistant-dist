# Politique de confidentialité — HiloAssistant

*Modèle — à faire réviser par un conseiller juridique avant usage contractuel.*

**Dernière mise à jour : [DATE]**

---

## 1. Objet et champ d'application

La présente Politique de confidentialité (la « Politique ») décrit comment **Hilo Tech inc.**, société ayant son siège à Longueuil, Québec, Canada (« Hilo Tech », « nous »), collecte, utilise, conserve, communique et protège les renseignements personnels dans le cadre de l'utilisation de l'application **HiloAssistant** (l'« Application »), une application de productivité pour macOS distribuée en marque blanche sous le nom « [Nom du client] Local Assistant ».

Cette Politique s'applique à toute personne qui installe, configure ou utilise l'Application (l'« Utilisateur », « vous »), qu'elle agisse à titre personnel ou pour le compte d'une organisation cliente de Hilo Tech.

Elle est rédigée conformément à la *Loi sur la protection des renseignements personnels dans le secteur privé* du Québec (la « Loi 25 ») et se veut, dans la mesure applicable, compatible avec le *Règlement général sur la protection des données* (RGPD) pour nos clients entreprises situés hors Québec.

---

## 2. Responsable du traitement et responsable de la protection des renseignements personnels

**Responsable du traitement :**
Hilo Tech inc.
Longueuil, Québec, Canada
Courriel : support@hilointelligence.ca

Conformément à l'article 3.1 de la Loi 25, Hilo Tech inc. a désigné un(e) **responsable de la protection des renseignements personnels** (RPRP), qui exerce cette fonction par défaut au titre de plus haute autorité de l'entreprise, sauf délégation formelle.

**Coordonnées du RPRP :**
[Nom du responsable de la protection des renseignements personnels]
Courriel : support@hilointelligence.ca
Objet du courriel : « Protection des renseignements personnels — HiloAssistant »

Toute question, demande d'accès, de rectification ou plainte relative à vos renseignements personnels doit être acheminée à cette adresse.

---

## 3. Fonctionnement général : accès conditionnel à HiloIntelligence

L'Application **ne peut être utilisée sans un compte HiloIntelligence actif**. La connexion s'effectue par authentification OAuth 2.0 / OIDC avec le protocole PKCE, selon un palier d'accès (« Plan 3 ») rattaché à votre compte. Sans authentification valide, l'Application demeure verrouillée et aucune fonctionnalité n'est accessible.

Si votre accès HiloIntelligence est révoqué, suspendu ou expire, l'Application se reverrouille automatiquement et cesse de fonctionner, sans traitement additionnel de vos renseignements personnels au-delà de la vérification du jeton d'accès.

---

## 4. Renseignements collectés, finalités et ce qui reste local

Le tableau ci-dessous présente, pour chaque fonctionnalité de l'Application, la nature des renseignements traités, la finalité poursuivie et si ces renseignements demeurent **localement sur votre appareil** ou sont **transmis** à un tiers.

### 4.1 Identité de compte et authentification

- **Renseignements** : identifiant unique stable (UUID « sub »), adresse courriel, nom, jeton d'accès OAuth et jeton de rafraîchissement.
- **Finalité** : authentifier l'Utilisateur, appliquer le palier d'accès applicable, permettre le fonctionnement de l'Application.
- **Localisation** : le jeton d'accès demeure **en mémoire seulement** (jamais écrit sur disque) ; le jeton de rafraîchissement est stocké dans le **Trousseau macOS**, chiffré par le système d'exploitation. L'identité de compte (UUID, courriel, nom) est fournie et gérée par HiloIntelligence, notre sous-traitant d'authentification.

### 4.2 Actions IA sur texte sélectionné

- **Renseignements** : le texte sélectionné par l'Utilisateur au moment où il déclenche une action d'intelligence artificielle (correction, reformulation, traduction, résumé, etc.).
- **Finalité** : exécuter l'action demandée par l'Utilisateur au moyen d'un modèle de langage.
- **Localisation** : **transmis** par TLS à la passerelle HiloIntelligence (`ai.<domaine>.ca`), accompagné du jeton OAuth de l'Utilisateur. Lorsque le mode « Sans rétention » est activé, un en-tête technique `X-Hilo-Privacy: no-retention` est envoyé afin de demander l'absence de conservation du contenu côté passerelle *(traitement à confirmer par le fournisseur de la passerelle)*.

### 4.3 Dictée vocale

- **Renseignements** : enregistrement audio de la voix de l'Utilisateur.
- **Finalité** : transcrire la parole en texte.
- **Localisation** : par défaut, la reconnaissance s'effectue **entièrement sur l'appareil** (Apple Speech), sans transmission. Si l'Utilisateur active volontairement l'option de transcription infonuagique (**réglage optionnel, désactivé par défaut**), l'audio est **transmis** à un modèle infonuagique tiers aux seules fins de transcription.

### 4.4 Historique du presse-papiers

- **Renseignements** : contenu copié par l'Utilisateur (texte, éléments structurés).
- **Finalité** : offrir un historique consultable du presse-papiers.
- **Localisation** : **stocké localement uniquement**, dans une base SQLite sur l'appareil de l'Utilisateur. Les éléments détectés comme sensibles (mots de passe, clés, numéros de carte) sont masqués à l'affichage. Cette donnée **ne quitte jamais l'appareil**, sauf si l'Utilisateur déclenche lui-même une action IA sur un élément du presse-papiers.

### 4.5 Infos rapides / coffre personnel

- **Renseignements** : renseignements personnels saisis volontairement par l'Utilisateur (adresses, informations de carte structurées sans le CVV, champs libres, etc.).
- **Finalité** : permettre à l'Utilisateur de retrouver et d'insérer rapidement ses propres renseignements dans d'autres applications.
- **Localisation** : **stocké localement uniquement**, chiffré au moyen d'AES-GCM (CryptoKit). Le CVV des cartes n'est **jamais** enregistré. Ces renseignements **ne quittent jamais l'appareil**. *La gestion de mots de passe a été retirée du produit et n'est pas offerte.*

### 4.6 Captures d'écran, vidéo et OCR

- **Renseignements** : images de capture d'écran, enregistrements vidéo, texte extrait par reconnaissance optique de caractères (OCR).
- **Finalité** : permettre à l'Utilisateur de capturer, annoter et réutiliser du contenu visuel.
- **Localisation** : captures, vidéos et OCR sont **traités et stockés localement** (reconnaissance Vision d'Apple, sur l'appareil). Si l'Utilisateur déclenche une action IA sur une capture, l'image ou le texte qui en est extrait est **transmis** à la passerelle HiloIntelligence, uniquement pour cette action précise.

### 4.7 Procédures automatisées (enregistrement de clics)

- **Renseignements** : séquence de clics (libellés d'accessibilité des éléments cliqués) et narration vocale ou textuelle associée.
- **Finalité** : documenter automatiquement une procédure pas à pas.
- **Localisation** : l'enregistrement des clics est **local**. La synthèse de la procédure (mise en forme, résumé) est **transmise** à la passerelle HiloIntelligence lorsque l'Utilisateur demande cette synthèse.

### 4.8 Permissions système macOS

Le fonctionnement de l'Application requiert l'octroi, par l'Utilisateur, des permissions macOS suivantes :
- **Accessibilité** : pour lire le texte sélectionné dans l'application active et y écrire le résultat d'une action.
- **Enregistrement de l'écran** : pour les fonctions de capture d'écran et d'enregistrement vidéo.
- **Micro** : pour la fonction de dictée.

Ces permissions sont gérées par macOS et peuvent être révoquées en tout temps par l'Utilisateur dans les Réglages système.

### 4.9 Télémétrie et rapports d'erreurs

- **Renseignements** : métadonnées techniques relatives à une erreur ou un plantage (jamais le contenu des textes, captures ou renseignements de l'Utilisateur).
- **Finalité** : diagnostiquer et corriger des anomalies techniques.
- **Localisation** : collecte **strictement sur consentement explicite (opt-in)**. Lorsqu'activée, l'information est actuellement transmise **par courriel de rapport** à support@hilointelligence.ca.

---

## 5. Base de traitement et consentement

Hilo Tech traite vos renseignements personnels sur les bases suivantes :

- **Exécution du service demandé** : l'authentification, le fonctionnement du presse-papiers local, du coffre local et des captures locales sont nécessaires à la fourniture de l'Application et ne requièrent pas de consentement distinct au-delà de l'installation et de l'utilisation volontaire de l'Application.
- **Consentement explicite (opt-in)** : la transcription vocale infonuagique et la télémétrie ne sont activées qu'après un geste positif et informé de l'Utilisateur, et peuvent être désactivées en tout temps dans les réglages de l'Application.
- **Action volontaire et ponctuelle de l'Utilisateur** : chaque transmission de texte, d'image ou d'audio à la passerelle HiloIntelligence résulte d'un geste déclenché intentionnellement par l'Utilisateur (sélection d'une action IA). Aucune transmission n'a lieu en arrière-plan à l'insu de l'Utilisateur.

Vous pouvez retirer votre consentement aux traitements optionnels en tout temps, sans affecter les fonctionnalités locales de base de l'Application (voir section 9).

---

## 6. Sous-traitants et destinataires des renseignements personnels

Hilo Tech fait appel aux sous-traitants et tiers suivants :

| Sous-traitant / tiers | Rôle | Renseignements concernés |
|---|---|---|
| **HiloIntelligence** (passerelle IA et service d'authentification, exploités par Hilo Tech inc. ou une entité affiliée) | Authentification OAuth, traitement des requêtes IA sur texte/image/audio transmis volontairement par l'Utilisateur | Identité de compte, jetons OAuth, contenu transmis lors d'une action IA |
| **Fournisseur de modèle de langage** sous-jacent à la passerelle HiloIntelligence | Traitement du contenu transmis afin de générer la réponse IA demandée | Texte, image ou audio transmis lors d'une action IA |
| **Apple inc.** | Fourniture des cadriciels du système d'exploitation (Trousseau macOS, reconnaissance vocale sur l'appareil Apple Speech, reconnaissance optique de caractères Vision) | Selon la fonctionnalité locale utilisée ; ces traitements demeurent sur l'appareil sauf activation explicite d'une option infonuagique |

**Aucun** autre sous-traitant, courtier de données, SDK d'analytique tiers ou traceur publicitaire n'est intégré à l'Application.

Un contrat de sous-traitance conforme à l'article 18.3 de la Loi 25 encadre la relation entre Hilo Tech et HiloIntelligence (et, le cas échéant, le fournisseur de modèle de langage), incluant les mesures de sécurité, les finalités autorisées et l'interdiction d'utiliser les renseignements à d'autres fins.

---

## 7. Transferts hors Québec et hors Canada

Certains sous-traitants mentionnés à la section 6, notamment le fournisseur de modèle de langage sous-jacent à la passerelle HiloIntelligence, peuvent traiter ou héberger des renseignements personnels **à l'extérieur du Québec**, potentiellement à l'extérieur du Canada.

Conformément à l'article 17 de la Loi 25, Hilo Tech effectue, avant tout transfert de cette nature, une **évaluation des facteurs relatifs à la vie privée (ÉFVP)** afin de s'assurer que les renseignements personnels bénéficient, dans le lieu de destination, d'une protection jugée adéquate au regard des principes généralement reconnus en matière de protection des renseignements personnels.

Ces transferts sont encadrés contractuellement (clauses de protection des données, engagements de sécurité et de confidentialité) et limités aux renseignements strictement nécessaires à l'exécution de l'action demandée par l'Utilisateur. Les renseignements identifiés à la section 4 comme demeurant « stockés localement uniquement » (presse-papiers, coffre, captures brutes, procédures brutes) **ne font l'objet d'aucun transfert**, quelle que soit la localisation.

---

## 8. Durées de conservation

| Catégorie de renseignements | Durée de conservation |
|---|---|
| Identité de compte, jetons de rafraîchissement | Pour la durée du compte HiloIntelligence actif ; supprimés à la révocation ou à la suppression du compte |
| Jeton d'accès OAuth | En mémoire uniquement ; effacé à la fermeture de session ou de l'Application |
| Historique du presse-papiers (local) | Conservé localement selon les réglages choisis par l'Utilisateur (durée ou nombre d'éléments configurable), jusqu'à suppression manuelle |
| Coffre / Infos rapides (local, chiffré) | Conservé localement jusqu'à suppression par l'Utilisateur |
| Captures, vidéos, OCR (local) | Conservés localement jusqu'à suppression par l'Utilisateur |
| Contenu transmis lors d'une action IA (texte, image, audio) | Non conservé par Hilo Tech au-delà du traitement de la requête ; conservation par la passerelle/le fournisseur de modèle limitée conformément à leurs politiques et, lorsqu'activé, au mode « Sans rétention » |
| Métadonnées de télémétrie (opt-in) | Conservées le temps nécessaire au diagnostic, puis supprimées ou anonymisées |

À l'expiration des durées de conservation applicables, les renseignements personnels sont détruits, désidentifiés ou anonymisés de façon sécuritaire, conformément à l'article 23 de la Loi 25.

---

## 9. Droits des personnes concernées

Conformément à la Loi 25, vous disposez des droits suivants à l'égard de vos renseignements personnels détenus par Hilo Tech :

- **Droit d'accès** : obtenir confirmation de l'existence de renseignements personnels vous concernant et en obtenir communication.
- **Droit de rectification** : faire corriger tout renseignement inexact, incomplet ou équivoque.
- **Droit de retrait du consentement** : retirer en tout temps votre consentement aux traitements optionnels (transcription vocale infonuagique, télémétrie), sans affecter les fonctionnalités locales de base.
- **Droit à la portabilité** : recevoir, dans un format technologique structuré et couramment utilisé, les renseignements personnels informatisés que vous avez fournis, ou en demander la transmission à un tiers.
- **Droit de désindexation et de suppression** : demander la cessation de la diffusion de renseignements vous causant un préjudice, dans les cas prévus par la loi.
- **Droit de porter plainte** : déposer une plainte auprès de la **Commission d'accès à l'information du Québec (CAI)** si vous estimez que vos droits n'ont pas été respectés.
  Site web : www.cai.gouv.qc.ca

Pour exercer l'un de ces droits, communiquez avec le responsable de la protection des renseignements personnels aux coordonnées indiquées à la section 2. Une réponse vous sera fournie dans les délais prévus par la loi (généralement 30 jours).

---

## 10. Témoins et technologies de suivi

L'Application HiloAssistant est un logiciel de bureau macOS et **n'utilise aucun témoin (cookie), pixel de suivi, identifiant publicitaire ou SDK d'analytique tiers**. Aucune activité de l'Utilisateur n'est partagée à des fins publicitaires ou de profilage commercial.

---

## 11. Renseignements personnels des mineurs

L'Application n'est pas destinée aux personnes mineures et n'est pas conçue pour collecter sciemment des renseignements personnels de mineurs. Si vous croyez qu'un mineur a fourni des renseignements personnels par l'entremise de l'Application, veuillez communiquer avec nous à support@hilointelligence.ca afin que nous puissions y remédier.

---

## 12. Incident de confidentialité

En cas d'incident de confidentialité présentant un **risque de préjudice sérieux** (accès non autorisé, utilisation ou communication non autorisée de renseignements personnels), Hilo Tech s'engage, conformément aux articles 3.5 à 3.8 de la Loi 25, à :

1. Prendre les mesures raisonnables pour réduire le risque qu'un préjudice soit causé et éviter que de nouveaux incidents de même nature ne se produisent ;
2. Aviser la **Commission d'accès à l'information du Québec** ainsi que toute personne concernée par l'incident, dans les meilleurs délais ;
3. Tenir un **registre des incidents de confidentialité**, conformément à la loi.

Si vous soupçonnez un incident de confidentialité touchant vos renseignements, contactez immédiatement support@hilointelligence.ca.

---

## 13. Sécurité des renseignements personnels

Hilo Tech met en œuvre des mesures de sécurité proportionnées à la sensibilité des renseignements traités, notamment :

- Chiffrement en transit (TLS) pour toute communication avec la passerelle HiloIntelligence ;
- Chiffrement au repos (AES-GCM via CryptoKit) pour le coffre / Infos rapides ;
- Stockage du jeton de rafraîchissement dans le **Trousseau macOS**, protégé par le système d'exploitation ;
- Jeton d'accès conservé en mémoire uniquement, jamais persisté sur disque ;
- Masquage à l'affichage des éléments sensibles détectés dans le presse-papiers ;
- Absence de transmission des renseignements stockés localement, sauf action volontaire de l'Utilisateur ;
- Limitation de l'accès aux renseignements aux seules personnes et systèmes qui en ont besoin.

Malgré ces mesures, aucun système n'est absolument sécurisé. Hilo Tech s'engage à réagir promptement à tout incident conformément à la section 12.

---

## 14. Modifications à la présente Politique

Hilo Tech peut modifier la présente Politique en tout temps, notamment pour refléter l'évolution de l'Application, de ses sous-traitants ou du cadre légal applicable. La version en vigueur sera toujours accessible depuis l'Application ou sur demande auprès du responsable de la protection des renseignements personnels. Toute modification substantielle fera l'objet d'un avis raisonnable aux Utilisateurs avant son entrée en vigueur.

---

## 15. Nous joindre

Pour toute question concernant la présente Politique ou le traitement de vos renseignements personnels :

**Hilo Tech inc.**
Responsable de la protection des renseignements personnels
Courriel : support@hilointelligence.ca

Vous pouvez également déposer une plainte auprès de la Commission d'accès à l'information du Québec : www.cai.gouv.qc.ca