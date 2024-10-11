
# Configuration GRUB Automatisée

Ce script Bash permet de configurer et personnaliser GRUB et, dans le cas d'une machine Apple, d'installer et configurer `rEFInd` pour la gestion du démarrage. Il s'assure que les thèmes GRUB sont correctement placés et mis à jour, tout en ajustant la configuration selon le fabricant du système.

## Prérequis

Avant de lancer ce script, assurez-vous d'avoir les éléments suivants :
- Accès root (le script utilise `sudo` pour exécuter des commandes nécessitant des privilèges administratifs).
- Un répertoire contenant les fichiers suivants :
  - `grub_themes/custom/`: Répertoire contenant les fichiers du thème GRUB.
  - `grub_themes/default_grub`: Fichier contenant les options GRUB par défaut.
  - `grub_themes/40_custom`: Script personnalisé pour l'entrée GRUB.
  - `grub_themes/custom/theme.txt.mac`: Thème spécifique pour Mac.
  - `grub_themes/custom/theme.txt.classic`: Thème classique pour les autres systèmes.
  
## Fonctionnalités

- **Détection automatique de la configuration GRUB** : Le script localise le fichier `grub.cfg` dans `/boot` pour configurer GRUB.
- **Installation des thèmes GRUB** : Les thèmes personnalisés sont copiés dans `/usr/share/grub/themes`.
- **Ajout d'options par défaut à GRUB** : Les options par défaut sont ajoutées au fichier `/etc/default/grub`.
- **Configuration spécifique aux Mac** :
  - Installation et configuration de `rEFInd` et `efibootmgr` pour la gestion du démarrage EFI.
  - Mise à jour des paramètres de délai (`timeout -1`) et de sélection par défaut dans la configuration de `rEFInd`.
  - Modification du fichier GRUB personnalisé avec le disque EFI spécifié par l'utilisateur.
- **Configuration pour les autres systèmes** : Si l'ordinateur n'est pas un Mac, un thème classique est appliqué.
- **Mise à jour de la configuration GRUB** : Le script exécute `grub-mkconfig` pour chaque fichier `grub.cfg` trouvé.

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
- Copier les fichiers de thème vers le répertoire de thèmes GRUB.
- Appliquer les modifications de configuration GRUB à `/etc/default/grub`.
- S'il s'agit d'un Mac, installer et configurer `rEFInd`, et demander à l'utilisateur de spécifier le disque EFI.
- Générer la nouvelle configuration GRUB.

## Remarques

- Ce script doit être exécuté avec des droits root pour modifier les fichiers système.
- Si GRUB n'est pas installé, le script affichera un message d'erreur.

## Compatibilité

- **Mac** : Le script est compatible avec les systèmes Mac pour lesquels il configure automatiquement `rEFInd` pour la gestion EFI.
- **Autres systèmes** : Il est compatible avec les autres machines où GRUB est installé par défaut, sans avoir besoin de `rEFInd`.

