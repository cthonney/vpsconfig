Ne pas oublier d'ouvrir les ports sur l'interface oracle avant

Console OCI > Compute > Instances > Sélectionne ta VM.

Bas de page : Attached VNICs > Clique sur le Subnet.

Clique sur la Security List (ex: Default Security List...).

2. Ajouter les règles (Ingress Rules)
Clique sur Add Ingress Rules et configure comme suit

Source Type	CIDR
Source CIDR	0.0.0.0/0
Protocol	TCP (pour 80, 443, SSH) ou UDP (Pangolin)
Port Range	80, 443, 22230, 21820, 51820


--------

```
cat << 'EOF' > setup_firewall.sh
#!/bin/bash

# --- 1. RESET IPTABLES ---
echo "Nettoyage d'iptables..."
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X
sudo iptables -t mangle -F
sudo iptables -t mangle -X

# --- 2. CONFIGURATION UFW ---
echo "Configuration de UFW..."
sudo apt update && sudo apt install ufw -y

# Reset et politiques par défaut
sudo ufw --force reset
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Autorisations
sudo ufw limit 22/tcp          # SSH standard + Rate limit
sudo ufw limit 22230/tcp       # SSH Custom + Rate limit
sudo ufw allow 80/tcp          # HTTP
sudo ufw allow 443/tcp         # HTTPS
sudo ufw allow 21820/udp       # Pangolin/VPN
sudo ufw allow 51820/udp       # Wireguard/Pangolin

# Activation
sudo ufw --force enable

echo "--------------------------"
sudo ufw status verbose
echo "--------------------------"
echo "Check terminé. Iptables est clean et UFW est actif."
EOF
```

------

# Rendre le script exécutable et le lancere :
```
chmod +x setup_firewall.sh
./setup_firewall.sh
```