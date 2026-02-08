# 🔐 Projet : Conception d’un système de communication sécurisée
## Inspiré de WhatsApp (E2EE) et HTTPS/TLS
## 📋 Sommaire

- [🎯 Objectifs du projet](#-objectifs-du-projet)
- [📁 Structure du projet](#-structure-du-projet)
- [🔧 Technologies utilisées](#-technologies-utilisées)
- [📊 Schémas et illustrations](#-schémas-et-illustrations)
- [📖 Explications clés](#-explications-clés)
- [🤔 Questions de réflexion](#-questions-de-réflexion)
- [📌 Conclusion](#-conclusion)

👤 Auteur
## 🎯 Objectifs du projet

Concevoir et analyser un **mini-système de communication sécurisée** intégrant :

- ✅ **Confidentialité** : chiffrement symétrique (AES)
- ✅ **Intégrité et authenticité** : signatures numériques (RSA/SHA-256)
- ✅ **Confiance par certificats** : modèle TLS/HTTPS
- ✅ **Lien réel** : explication des mécanismes WhatsApp (E2EE) et HTTPS (TLS)

## 📁 Structure du projet
### Partie 1 : Confidentialité (WhatsApp – E2EE)

📌 Principe : Chiffrement symétrique avec clé unique par session
├── Explication théorique : AES-256
├── Chiffrement d’un message texte
├── Impact longueur du mot de passe : attaque par bruteforce
└── Réponses :
    • WhatsApp utilise des clés longues auto-générées
    • Clé dérivée par message → confidentialité persistante

### Partie 2 : Intégrité & Authentification (Signatures numériques)
📌 Principe : Paire clé privée/publique
├── Génération clés RSA : openssl genrsa
├── Signature d’un message (SHA-256)
├── Vérification signature
├── Échec après modification → garantie d’intégrité
└── Lien WhatsApp : chaque utilisateur signe ses messages

### Partie 3 : HTTPS & TLS (Sécurisation client-serveur)
📌 Principe : TLS superposé à HTTP
├── Handshake TLS décrit étape par étape
├── Analyse certificat HTTPS (Google)
├── Proxy HTTPS : mécanisme d’interception
└── WhatsApp : TLS pour connexion initiale → puis E2EE

## 🔧 Technologies utilisées

| Outil | Usage |
|-------|-------|
| **OpenSSL** | Génération de clés, signatures, analyse de certificats |
| **AES-256** | Chiffrement symétrique |
| **RSA + SHA-256** | Signatures numériques |
| **TLS 1.3** | Protocole de sécurisation HTTP |
| **Wireshark** | Analyse du trafic réseau (mentionné) |


## 📊 Schémas et illustrations

### Schéma 1 : Architecture globale
[ Alice ] <- E2EE (AES) -> [ Bob ]
      ^                      ^
      v HTTPS/TLS            v
[ Serveur sécurisé ] <- TLS -> [ Client ]

### Schéma 2 : Handshake TLS simplifié
Client                     Serveur
  |----ClientHello------->|
  |<-----ServerHello------|
  |<-----Certificat-------|
  |------Clé de session-->|
  |<-----Finished---------|
  |------Finished-------->|
  |<-> Communication chiffrée <->|


### Schéma 3 : Comparaison WhatsApp vs HTTPS:
WhatsApp :
Connexion → TLS → Établissement → E2EE (AES par message)

HTTPS :
Connexion → TLS → Communication chiffrée (TLS seulement)


## Illustration : Impact longueur mot de passe
Longueur | Sécurité
---------|---------
1-6      | ⚠️ Faible (cassable en secondes)
8-12     | ✅ Moyenne (résiste quelques heures)
14+      | 🛡️ Forte (années de calcul)
## 📖 Explications clés

### 1. Pourquoi WhatsApp n'utilise pas de mots de passe courts ?
- **Vulnérabilité** : Les mots de passe courts sont sensibles aux attaques par dictionnaire/bruteforce
- **Solution WhatsApp** : Utilisation de clés cryptographiques longues (256 bits) générées automatiquement
- **Résultat** : Sécurité renforcée, indépendante du choix de l'utilisateur

### 2. Signature numérique indispensable même avec chiffrement
Chiffrement seul :       Confidentialité ✅
                        Intégrité ❌
                        Authenticité ❌

Chiffrement + Signature : Confidentialité ✅
                         Intégrité ✅
                         Authenticité ✅
                         Non-répudiation ✅

### 3. Différence : Confidentialité vs Intégrité

| Aspect          | Confidentialité                         | Intégrité                               |
|-----------------|-----------------------------------------|-----------------------------------------|
| **Objectif**    | Empêcher la lecture                     | Empêcher la modification                |
| **Mécanisme**   | Chiffrement (AES, RSA, etc.)           | Hachage/Signature (SHA-256, RSA, etc.)  |
| **Exemple**     | Message illisible                       | Signature invalide si message altéré    |
| **Protection**  | Contenu secret                          | Contenu authentique                     |
| **Faille**      | Révélation du contenu                   | Altération non détectée


## 🤔 Questions de réflexion

### 1. Pourquoi la signature est-elle nécessaire même avec chiffrement ?
> **Réponse :** Parce que le chiffrement protège le contenu, mais pas l’identité de l’expéditeur ni l’intégrité du message.

### 2. Comment un proxy HTTPS intercepte-t-il le trafic ?
> **Réponse :** Via un certificat intermédiaire installé sur le client → le proxy joue l'homme du milieu (MITM) avec l'accord de l'utilisateur.

### 3. Pourquoi WhatsApp utilise TLS seulement au début ?
> **Réponse :** TLS sécurise la connexion initiale ; ensuite, E2EE prend le relais pour protéger les messages même contre WhatsApp.

---

## 📌 Conclusion

Ce projet a permis de **modéliser un système de communication sécurisée complet** en intégrant :

- 🔒 **Chiffrement symétrique** pour la confidentialité
- ✍️ **Signatures numériques** pour l'intégrité et l'authenticité
- 🌐 **Certificats TLS** pour la confiance client-serveur

L'analyse comparative avec **WhatsApp (E2EE)** et **HTTPS (TLS)** montre comment ces mécanismes sont utilisés dans des applications réelles pour garantir une sécurité maximale.

## 👤 Auteur

**DIAWANE Ramatoulaye**  
Étudiante en 4ème année d’Ingénierie Informatique & IA  
Projet réalisé le 23 janvier 2026

📘 Note pédagogique : Ce projet illustre des concepts de sécurité réels dans un cadre académique. Les implémentations sont simplifiées et ne doivent pas être déployées en production sans audit.