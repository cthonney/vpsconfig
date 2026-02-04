###Sécuriser UFW pour OCI

```
sudo nano /etc/ufw/before.rules
```

Repère la ligne 
```#
End required lines. 
```

Colle ce bloc juste après.

```

# ==========================================================
# SERVICES CRITIQUES ORACLE CLOUD (OCI)
# ==========================================================

# 1. Autoriser iSCSI (Volumes Block Storage) pour ROOT uniquement
-A ufw-before-output -d 169.254.0.2/32 -p tcp --dport 3260 -m owner --uid-owner 0 -j ACCEPT
-A ufw-before-output -d 169.254.2.0/24 -p tcp --dport 3260 -m owner --uid-owner 0 -j ACCEPT
-A ufw-before-output -d 169.254.4.0/24 -p tcp --dport 3260 -m owner --uid-owner 0 -j ACCEPT
-A ufw-before-output -d 169.254.5.0/24 -p tcp --dport 3260 -m owner --uid-owner 0 -j ACCEPT

# 2. DNS Interne Oracle
-A ufw-before-output -d 169.254.169.254/32 -p udp --dport 53 -j ACCEPT
-A ufw-before-output -d 169.254.169.254/32 -p tcp --dport 53 -j ACCEPT

# 3. Metadata Service (ID Instance, Clés SSH, Cloud-Init)
-A ufw-before-output -d 169.254.169.254/32 -p tcp --dport 80 -j ACCEPT

# 4. DHCP, TFTP et NTP (Sync réseau et horaire)
-A ufw-before-output -d 169.254.169.254/32 -p udp --dport 67:69 -j ACCEPT
-A ufw-before-output -d 169.254.169.254/32 -p udp --dport 123 -j ACCEPT

# 5. Sécurité : Rejet propre du reste du trafic Link-Local
-A ufw-before-output -d 169.254.0.0/16 -p tcp -j REJECT --reject-with tcp-reset
-A ufw-before-output -d 169.254.0.0/16 -p udp -j REJECT --reject-with icmp-port-unreachable

# ==========================================================

```


```
sudo ufw reload
```
