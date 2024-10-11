
# Script d'installation de Plasma et de configuration GRUB

Ce script Bash permet d'installer l'environnement de bureau KDE Plasma, de configurer GRUB avec un thème personnalisé, et, dans le cas d'une machine Apple, d'installer et configurer `rEFInd` pour la gestion du démarrage EFI.

## Prérequis

Avant d'exécuter ce script, assurez-vous d'avoir :
- Accès root ou sudo (le script nécessite des privilèges administratifs).
- Un répertoire contenant les fichiers suivants :
  - `grub_themes/custom/`: Répertoire contenant les fichiers du thème GRUB.
  - `grub_themes/default_grub`: Fichier contenant les options GRUB par défaut.
  - `grub_themes/40_custom`: Script personnalisé pour une entrée GRUB.
  - `grub_themes/custom/theme.txt.mac`: Thème pour les systèmes Mac.
  - `grub_themes/custom/theme.txt.classic`: Thème pour les autres systèmes.

## Fonctionnalités

1. **Mise à jour du système** :
   - Le script exécute `sudo apt update && sudo apt upgrade -y` pour mettre à jour les paquets du système.
   
2. **Installation de l'environnement KDE Plasma** :
   - Plasma est installé via la commande `sudo apt install kde-plasma-desktop -y`.

3. **Configuration GRUB** :
   - Le script détecte la présence de fichiers `grub.cfg` dans `/boot`.
   - Il copie les fichiers de thème vers le répertoire `/usr/share/grub/themes`.
   - Les options par défaut sont ajoutées au fichier `/etc/default/grub`.

4. **Configuration spécifique aux Mac** :
   - Si le système est un Mac (détecté via `dmidecode`), le script installe et configure `rEFInd` pour gérer le démarrage EFI.
   - Il modifie le fichier de configuration `refind.conf` pour désactiver le timeout et changer la sélection par défaut.
   - L'utilisateur doit spécifier le disque EFI pour modifier le script personnalisé GRUB.
   
5. **Configuration pour les autres systèmes** :
   - Si la machine n'est pas un Mac, le thème classique est appliqué.
   
6. **Mise à jour de la configuration GRUB** :
   - Le script génère la configuration GRUB avec `grub-mkconfig` pour chaque fichier `grub.cfg` trouvé.

## Instructions

1. **Cloner ou copier le répertoire contenant le script et les fichiers requis**.
2. **Donner les droits d'exécution au script** :
   ```bash
   chmod +x nom_du_script.sh
   ```
3. **Exécuter le script** :
   ```bash
   ./nom_du_script.sh
   ```

Le script va :
- Mettre à jour le système et installer KDE Plasma.
- Copier les fichiers de thème dans le répertoire des thèmes GRUB.
- Appliquer des modifications de configuration à `/etc/default/grub`.
- S'il s'agit d'un Mac, installer et configurer `rEFInd` pour gérer EFI.
- Demander à l'utilisateur de spécifier le disque EFI.
- Générer la nouvelle configuration GRUB.

## Remarques

- Le script doit être exécuté avec des droits administratifs (`sudo`).
- Si GRUB n'est pas installé, un message d'erreur sera affiché.
- Sur Mac, le disque EFI doit être spécifié manuellement lorsque demandé.

## Compatibilité

- **Mac** : Ce script est compatible avec les systèmes Mac et utilise `rEFInd` pour gérer le démarrage EFI.
- **Autres systèmes** : Compatible avec les machines où GRUB est installé et KDE Plasma peut être utilisé comme environnement de bureau.

