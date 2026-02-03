1. Modification de la configuration
sudo nano /etc/ssh/sshd_config
Note : Cherche la ligne #Port 22, décommente-la et remplace par ton port (ex: Port 22230).

2. Application des changements
Sur Ubuntu, si ssh.socket est actif, un simple restart ssh ne suffit pas toujours. Voici la séquence complète :


# 1. Recharger la configuration de systemd
sudo systemctl daemon-reload

# 2. Redémarrer le socket (prioritaire pour la gestion du port)
sudo systemctl restart ssh.socket

# 3. Redémarrer le service SSH
sudo systemctl restart ssh.service
3. Vérification immédiate
Vérifie que le service écoute bien sur le nouveau port avant de fermer ta session actuelle


# Vérifier le statut
sudo systemctl status ssh

# Vérifier l'écoute du port
ss -tulpn | grep ssh

# Tester avec un nouvelle connexion SSH avant de fermer l'actuelle