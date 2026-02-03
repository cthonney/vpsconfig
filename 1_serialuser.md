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