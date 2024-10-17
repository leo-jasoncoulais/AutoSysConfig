
# Installation et Configuration Automatique sous Linux

## Description

Ce script Bash est conçu pour installer et configurer l'environnement Plasma, le gestionnaire de shell Oh My Zsh, ainsi que d'autres logiciels tiers sur des systèmes Linux basés sur différents gestionnaires de paquets (apt, pacman, dnf). Il inclut également la configuration du gestionnaire de démarrage GRUB et du bootloader rEFInd pour les systèmes Mac.

### Fonctionnalités
- Mise à jour du système et installation de `curl`
- Installation et configuration de l'environnement de bureau KDE Plasma
- Installation et configuration de `Oh My Zsh` avec des plugins personnalisés
- Installation de logiciels tiers à partir de scripts fournis
- Configuration de GRUB et rEFInd pour Mac si applicable

## Prérequis

- Systèmes Linux compatibles utilisant l'un des gestionnaires de paquets suivants : `apt`, `pacman`, ou `dnf`
- Accès superutilisateur (`sudo`) pour l'installation de logiciels
- Connexion internet pour télécharger les outils nécessaires

## Utilisation

### 1. Exécution du script

Exécutez le script avec les droits superutilisateur :

```bash
sudo ./config-system
```

### 2. Mise à jour et installation des paquets requis

Le script détecte automatiquement votre gestionnaire de paquets (`apt`, `pacman`, ou `dnf`) et met à jour le système avant d'installer les paquets nécessaires comme `curl`.

### 3. Installation et configuration de KDE Plasma

- Si vous souhaitez installer l'environnement de bureau KDE Plasma, le script l'installera automatiquement en fonction de votre gestionnaire de paquets.

### 4. Installation et configuration de Oh My Zsh

- Le script installe `zsh` et configure automatiquement `Oh My Zsh`. Il copie également les plugins personnalisés disponibles dans le répertoire `zsh-plugins` vers le répertoire des plugins de `Oh My Zsh`.
- Il met ensuite à jour le fichier de configuration `~/.zshrc` pour activer les plugins et inclure le plugin `git`.

### 5. Installation des logiciels tiers

- Vous pouvez sélectionner les logiciels que vous souhaitez installer parmi ceux présents dans le répertoire `applications-install`. Le script vous propose une interface interactive où vous pouvez :
  - Sélectionner un logiciel à installer en choisissant son numéro
  - Installer tous les logiciels d'un coup en entrant "a"
  - Continuer l'installation après avoir choisi les logiciels souhaités en entrant "c"

### 6. Configuration de GRUB et rEFInd (pour Mac)

- Le script configure GRUB en copiant les thèmes et en modifiant les fichiers de configuration.
- Pour les systèmes Mac, il installe et configure `rEFInd` pour gérer le démarrage EFI. Vous devrez fournir le nom du disque EFI lors de la configuration.
- **Note importante** : Pensez à changer l'ordre de démarrage avec `efibootmgr` après l'installation de `rEFInd`.

## Structure du projet

- `applications-install/` : Contient les scripts d'installation des logiciels tiers.
- `grub_themes/` : Contient les thèmes GRUB personnalisés.
- `zsh-plugins/` : Contient les plugins personnalisés pour `Oh My Zsh`.

## Remarques

- **Compatibilité OS** : Si votre système d'exploitation ne dispose pas de `apt`, `pacman`, ou `dnf`, le script ne pourra pas fonctionner et affichera un message d'erreur.
- **Systèmes Mac** : Le script effectue des configurations spécifiques pour les systèmes Mac avec rEFInd. Assurez-vous de suivre les instructions concernant `efibootmgr`.
