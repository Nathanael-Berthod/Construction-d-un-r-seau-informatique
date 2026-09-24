Construire un réseau informatique

> **BUT Réseaux & Télécommunications — IUT de Roanne — Semestre 2**
> Auteur : Nathanael Berthod (R&T 1 / B2)

Conception et configuration d'un réseau d'entreprise multi-sites inspiré de l'organisation de l'**Université Jean Monnet** (Carnot, Métare, Roanne, Tréfilerie). Le projet couvre le plan d'adressage VLSM, la segmentation VLAN, le routage dynamique RIPv2, le NAT et la sécurisation par ACL étendues, le tout simulé sous **Cisco Packet Tracer**.

---

## 📁 Contenu du dépôt

| Fichier | Description |
|---|---|
| `Réseau_finale.pkt` | Topologie complète Cisco Packet Tracer (4 LAN + inter-site + WAN) |
| `Compte-rendu_SAE2_01_Berthod_Nathanael.docx` | Rapport complet du projet (plan d'adressage, justifications, tests, annexes) |
| `ACL1.txt` | Configuration des 4 ACL étendues (FTP, ADMIN, TFTP, WEB) |
| `conf_routeur_wan.txt` | Configuration du routeur WAN (NAT + RIPv2 + SSH) |

---

## 🎯 Objectifs

- Concevoir un plan d'adressage IP en **VLSM** sur la plage `172.16.0.0/16`
- Interconnecter **4 sites** géographiques distincts (LAN 1 à 4) via des liaisons point à point
- Segmenter chaque site en **5 VLANs** (Admin, Service, Formation A, Formation B, Enseignants)
- Mettre en œuvre le **routage dynamique RIPv2** entre les routeurs FAI et Entreprise
- Configurer un **NAT dynamique avec surcharge** pour la sortie Internet
- Sécuriser les VLANs par des **ACL étendues**
- Sécuriser l'administration des équipements via **SSH**

---

## 🏗️ Architecture

### Sites et capacités

| LAN | Nb machines | Réseau attribué | CIDR |
|---|---|---|---|
| LAN 3 | 8500 | `172.16.0.0` | /18 |
| LAN 2 | 8000 | `172.16.64.0` | /19 |
| LAN 4 | 2000 | `172.16.96.0` | /21 |
| LAN 1 | 300  | `172.16.104.0` | /23 |

Chaque LAN est composé de **3 bâtiments (A, B, C)** interconnectés en boucle physique — protégée par le protocole **STP** pour éviter les tempêtes de broadcast.

### Segmentation VLAN (identique sur chaque site)

| VLAN | Nom | Usage |
|---|---|---|
| 10  | Admin       | Administration des équipements (SSH) |
| 20  | Service     | Personnel administratif |
| 30  | Formation A | Étudiants formation A |
| 40  | Formation B | Étudiants formation B |
| 100 | Enseignants | Corps enseignant |

### Matériel utilisé

- **Routeurs** : Cisco 1941 + carte d'extension **HWIC-2T** (ports série pour les liaisons inter-site)
- **Switchs** : Cisco 2960 (ports FastEthernet pour PC/serveurs, Gigabit pour interconnexion)
- **Câblage** : Cat 6a pour le cuivre, fibre multimode **1000BASE-SX 50/125** pour les liens > 100 m

---

## ⚙️ Fonctionnalités mises en place

### 🔀 Routage
- **RIPv2** annoncé sur tous les routeurs FAI, Entreprise et WAN
- Réseaux `192.168.1.0/30` pour les liaisons point à point inter-FAI
- Sous-interfaces `gi0/0.10` → `gi0/0.100` pour le routage inter-VLAN sur chaque routeur Entreprise

### 🌍 NAT
- Routeur **WAN** :
  - `gi0/0` : `192.168.1.42/30` (nat **outside**) vers FAI1
  - `gi0/1` : `8.8.8.1/24` (nat **inside**)
- **NAT dynamique avec overload** sur pool public `161.3.36.32/28`
- Tous les LAN privés (`172.16.0.0/16`) accèdent à Internet via des adresses publiques traduites

### 🔒 Sécurisation par ACL
4 ACL étendues configurées sur les routeurs Entreprise :

| ACL | Rôle |
|---|---|
| `ACL1_FTP`  | Autorise le FTP vers le serveur `172.16.102.70` depuis les VLAN Service/Enseignant |
| `ACL2_ADMIN`| Bloque l'accès aux VLAN Admin de chaque site + autorise HTTP/HTTPS/DNS |
| `ACL3_TFTP` | Autorise le TFTP vers les serveurs locaux + ICMP echo/reply |
| `ACL4_WEB`  | Autorise HTTP/HTTPS vers les serveurs web de chaque site |

### 🔐 Administration
- **SSH v2** activé sur tous les switchs (interface VLAN 10) et sur le routeur WAN
- Génération de clé RSA 2048 bits
- Mot de passe **enable secret** + comptes locaux avec privilège 15
- Passwords distincts pour console, VTY et enable sur chaque équipement (voir annexe du rapport)

---

## 🧪 Tests de connectivité validés

- ✅ Ping **inter-VLAN** au sein de chaque LAN (bâtiments A ↔ B ↔ C)
- ✅ Ping **inter-site** (LAN 1 ↔ LAN 2 ↔ LAN 3 ↔ LAN 4)
- ✅ Accès aux **serveurs WEB et TFTP** depuis tous les bâtiments
- ✅ Connexion **SSH** aux switchs depuis le VLAN Admin
- ✅ Ping vers **8.8.8.8** (Internet simulé) validant le NAT WAN

---

## 🚀 Utilisation

1. Ouvrir `Réseau_finale.pkt` avec **Cisco Packet Tracer 8.x** ou plus récent
2. Lancer la simulation et attendre la convergence RIP (~30 s)
3. Tester les pings depuis les PC des différents bâtiments
4. Consulter le compte-rendu `.docx` pour le détail des plans d'adressage et de la logique de configuration
5. Les fichiers `.txt` peuvent être copiés-collés directement en CLI dans les routeurs concernés

---

## 📌 Points d'amélioration

- Ordre et sens d'application (in/out) des ACL à revoir : certaines règles bloquent du trafic légitime malgré les autorisations
- Meilleure organisation des sauvegardes de configurations
- Approfondir la manipulation avancée des ACL et du filtrage

---

## 📚 Compétences mises en œuvre

`VLSM` · `VLAN` · `Trunk 802.1Q` · `STP` · `Inter-VLAN routing` · `RIPv2` · `NAT (PAT)` · `ACL étendues` · `SSH` · `Cisco IOS` · `Packet Tracer`

---

*Projet réalisé dans le cadre du BUT R&T — IUT de Roanne — 2025.*
