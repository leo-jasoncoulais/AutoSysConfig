
# Installation et Configuration Automatisée

Ce script permet d'installer et de configurer divers outils et environnements, notamment l'environnement de bureau Plasma, Oh My Zsh, et GRUB, ainsi que des ajustements pour les systèmes Mac.

## Fonctionnalités du script

1. **Mise à jour et upgrade du système**  
   Le script commence par mettre à jour la liste des paquets disponibles et par installer les mises à jour disponibles :

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Installation de Curl**  
   Curl est installé s'il n'est pas déjà présent :

   ```bash
   sudo apt install curl -y
   ```

3. **Installation et configuration de KDE Plasma**  
   L'environnement de bureau KDE Plasma est installé avec cette commande :

   ```bash
   sudo apt install kde-plasma-desktop -y
   ```

4. **Installation et configuration de Oh My Zsh**  
   Le script installe Zsh et Oh My Zsh, puis modifie le shell par défaut pour Zsh :

   ```bash
   sudo apt install zsh
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   sudo chsh -s $(which zsh)
   ```

   De plus, il copie tous les plugins trouvés dans le répertoire `zsh-plugins` vers le répertoire de plugins Oh My Zsh, et les configure dans le fichier `~/.zshrc` :

   ```bash
   PLUGINS=$(ls zsh-plugins)
   for plugin in $PLUGINS; do
       sudo cp -r zsh-plugins/$plugin ~/.oh-my-zsh/plugins
   done
   sed -i "s/plugins=(.*)/plugins=($PLUGINS)/g" ~/.zshrc
   ```

5. **Configuration de GRUB**  
   Si GRUB est installé, le script trouve le fichier de configuration de GRUB (`grub.cfg`) et effectue une configuration personnalisée :

   - Copie un thème GRUB personnalisé.
   - Modifie `/etc/default/grub` pour y inclure la configuration par défaut spécifiée.
   
   Si le système est un Mac, le script installe et configure `refind` et adapte GRUB pour ce type de système en appliquant des configurations spécifiques.

   ```bash
   sudo refind-install
   sudo cp grub_themes/40_custom /etc/grub.d/40_custom
   read -p "Quel est le disk EFI ? " efi_disk
   sudo sed -i 's/DISK_EFI_TO_CHANGE/$efi_disk/g' /etc/grub.d/40_custom
   ```

   Sinon, il applique une configuration classique :

   ```bash
   sudo cp /usr/share/grub/themes/custom/theme.txt.classic /usr/share/grub/themes/custom/theme.txt
   ```

   Enfin, le script régénère la configuration GRUB :

   ```bash
   for config in $GRUB_CONFIG; do
       sudo grub-mkconfig -o $config;
   done
   ```

6. **Messages d'alerte**  
   Si GRUB n'est pas installé, le script informe l'utilisateur que GRUB n'est pas présent.

## Instructions d'installation

### Prérequis

Avant de lancer ce script, assurez-vous que les outils suivants sont disponibles sur votre machine :

- `sudo` pour les permissions administratives.
- `curl` pour le téléchargement de scripts externes.
- `dmidecode` pour la détection du producteur du système.

### Exécution du script

1. Clonez ou téléchargez ce dépôt sur votre machine.
2. Assurez-vous que le script a les permissions d'exécution :

   ```bash
   chmod +x script.sh
   ```

3. Exécutez le script en tant qu'administrateur :

   ```bash
   sudo ./script.sh
   ```

## Avertissements

- Ce script modifie des configurations système critiques telles que GRUB. Utilisez-le avec précaution, notamment sur les machines Mac où des ajustements EFI sont appliqués.
- Vérifiez que les fichiers et chemins référencés existent bien (par exemple, les thèmes GRUB et les plugins Zsh).

