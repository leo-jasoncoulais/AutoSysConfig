
# Script d'installation et configuration automatisée

Ce script permet d'installer et de configurer un environnement de bureau KDE Plasma, Oh My Zsh, des logiciels tiers, ainsi que de configurer GRUB avec un thème personnalisé. Il détecte également si la machine est un Mac pour configurer rEFInd en conséquence.

## Prérequis

- Système d'exploitation basé sur Ubuntu/Debian
- Connexion Internet
- Droits sudo

## Fonctionnalités

- Mise à jour du système et installation de dépendances
- Installation et configuration de KDE Plasma
- Installation et configuration d'Oh My Zsh avec des plugins personnalisés
- Installation de logiciels tiers à partir d'un répertoire spécifique
- Configuration de GRUB avec un thème personnalisé
- Configuration spécifique pour les machines Mac avec rEFInd

## Structure du script

### 1. Mise à jour et installation des paquets

Le script commence par mettre à jour le système et installer certains paquets comme `curl` et `zsh`.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl -y
```

### 2. Installation de KDE Plasma

KDE Plasma est installé via la commande suivante :

```bash
sudo apt install kde-plasma-desktop -y
```

### 3. Installation et configuration d'Oh My Zsh

Oh My Zsh est installé et configuré, avec les plugins disponibles dans le dossier `zsh-plugins`.

```bash
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sudo chsh -s $(which zsh)
```

Les plugins sont copiés dans le répertoire d'Oh My Zsh et ajoutés dans le fichier `.zshrc` :

```bash
for plugin in $PLUGINS; do
    cp -r zsh-plugins/$plugin ~/.oh-my-zsh/plugins
done
```

### 4. Installation des logiciels tiers

Les logiciels tiers situés dans le répertoire `applications-install` sont proposés pour installation. L'utilisateur peut choisir d'installer un logiciel spécifique, tous les logiciels, ou continuer sans installation supplémentaire.

```bash
mkdir /tmp/installation-script
```

### 5. Configuration de GRUB

Le script détecte le fichier de configuration GRUB (`grub.cfg`), copie un thème personnalisé dans le répertoire de GRUB et configure GRUB ou rEFInd pour les machines Mac.

```bash
GRUB_CONFIG=$(sudo find /boot -name "grub.cfg")
PRODUCER=$(sudo dmidecode -s system-product-name)
```

Pour les machines Mac, rEFInd est installé et configuré avec des options spécifiques.

```bash
if [[ $PRODUCER == "Mac"* ]]; then
    sudo apt install refind efibootmgr -y
    sudo refind-install
fi
```

### 6. Nettoyage

Une fois toutes les installations et configurations terminées, le répertoire temporaire est supprimé :

```bash
rm -rf /tmp/installation-script
```

## Instructions d'utilisation

1. Cloner ou copier ce script dans un répertoire de travail.
2. Assurez-vous que le répertoire `zsh-plugins` contient les plugins Zsh que vous souhaitez installer.
3. Assurez-vous que le répertoire `applications-install` contient les scripts d'installation des logiciels que vous voulez ajouter.
4. Si vous souhaitez configurer GRUB avec un thème personnalisé, placez les fichiers dans le répertoire `grub_themes`.
5. Exécutez le script avec les droits administrateur :

```bash
sudo ./nom_du_script.sh
```

## Remarques

- Ce script est conçu pour les systèmes basés sur Ubuntu/Debian.
- Pour les utilisateurs de Mac, rEFInd sera installé pour gérer les démarrages.
- Veuillez ajuster les chemins et options si nécessaire selon vos besoins.

## Dépendances

- `apt`
- `curl`
- `zsh`
- `grub`
- `refind` (pour les utilisateurs de Mac)

## Avertissements

- Ce script modifie des fichiers système (GRUB, rEFInd). Utilisez-le à vos risques et périls.
