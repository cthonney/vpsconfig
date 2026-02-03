Une seule commande pour creer le script et l'executer 

```
cat << 'EOF' > setup_swap.sh && chmod +x setup_swap.sh && sudo ./setup_swap.sh
#!/bin/bash
# 1. Création du fichier 1GB
sudo fallocate -l 1G /swapfile || sudo dd if=/dev/zero of=/swapfile bs=1M count=1024
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# 2. Persistence fstab
if ! grep -q "/swapfile" /etc/fstab; then
    echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
fi

# Vérification finale
free -h
EOF

```