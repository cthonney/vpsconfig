cat << 'EOF' > setup_user.sh
#!/bin/bash

# Nom de l'utilisateur
USERNAME="serial_user"

# Création de l'utilisateur avec home directory et shell bash
sudo useradd -m -s /bin/bash "$USERNAME"

# Définition du mot de passe (interaction requise)
echo "--- Définition du mot de passe pour $USERNAME ---"
sudo passwd "$USERNAME"

# Ajout aux groupes sudo (privilèges) et dialout/tty (accès port série)
sudo usermod -aG sudo,dialout,tty "$USERNAME"

# Configuration du sudo sans mot de passe pour cet utilisateur
echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" | sudo tee "/etc/sudoers.d/$USERNAME" > /dev/null
sudo chmod 0440 "/etc/sudoers.d/$USERNAME"

echo "-----------------------------------------------"
echo "Utilisateur '$USERNAME' créé avec succès."
echo "Tu peux maintenant l'utiliser sur la console Oracle."
echo "-----------------------------------------------"
EOF


--------

# Rendre le script exécutable et le lancer
chmod +x setup_user.sh
sudo ./setup_user.sh


Pour accéder à la console série sur Oracle Cloud Infrastructure (OCI), voici la marche à suivre directement depuis ton tableau de bord :

1. Accès via l'interface OCI
Connecte-toi à ton interface OCI.

Va dans Compute > Instances.

Clique sur le nom de ton instance Ubuntu.

Dans le menu de gauche (sous Resources), clique sur Console Connections.

Clique sur le bouton Launch Cloud Shell connection.

2. Se connecter avec l'utilisateur créé
Une fois la fenêtre du Cloud Shell ouverte :

Appuie sur Entrée pour voir l'invite de login.

Entre le nom d'utilisateur : serial_user (ou celui que tu as choisi).

Entre le mot de passe défini lors de l'exécution du script.

Tu es maintenant connecté. Pour passer en root, tape simplement sudo -i (grâce à la règle NOPASSWD du script, aucun mot de passe ne sera demandé).