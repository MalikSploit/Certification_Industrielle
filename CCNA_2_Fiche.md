# Tâches et Commandes IOS

## Personnalisation et Sécurisation de l'Appareil

### Nommer l'appareil avec Hostname
- Pour définir le nom de l'appareil : `Router(config)# hostname [nom-d'appareil]`
    - Exemple : `Router(config)# hostname MyRouter`

### Créer un Banner
- Pour définir un message de bannière : `Router(config)# banner motd # [message] #`
    - Exemple : `Router(config)# banner motd # Bienvenue sur MyRouter - Accès autorisé uniquement #`

### Activer le mot de passe pour le mode privilégié
- Pour définir un mot de passe pour l'accès au mode privilégié : `Router(config)# enable secret [mot-de-passe]`
    - Exemple : `Router(config)# enable secret mySecretPassword`

### Définir un mot de passe pour le mode de configuration (conf t)
- Pour sécuriser l'accès au mode de configuration : `Router(config)# line con 0`
    - Suivi par : `Router(config-line)# password [mot-de-passe]`
    - Et pour requérir le mot de passe à la connexion : `Router(config-line)# login`
    - Exemple : 
    ```
    Router(config)# line con 0
    Router(config-line)# password confPassword
    Router(config-line)# login
    ```

### Chiffrer les mots de passe dans Running Configuration
- Pour chiffrer les mots de passe affichés dans la configuration en cours : `Router(config)# service password-encryption`
  - Cette commande crypte tous les mots de passe non chiffrés qui apparaissent lorsqu'on fait `show running-config`.

### Copier la Startup Configuration
- Pour sauvegarder la configuration actuelle dans la configuration de démarrage : `Router# copy running-config startup-config`
  - Cela sauvegarde la configuration en cours d'utilisation dans la mémoire non volatile pour le prochain démarrage.

### Voir la Running Configuration
- Pour afficher la configuration actuelle de l'appareil : `Router# show running-config`
  - Cette commande affiche la configuration actuellement active sur l'appareil.


## Ajout d'adresses IP dans un routeur et un commutateur

### Configuration d'une adresse IP sur un routeur
- Entrer en mode de configuration d'interface : `Router(config)# interface [interface-type][interface-number]`
    - Exemple : `Router(config)# interface GigabitEthernet0/0`
- Assigner une adresse IP et un masque de sous-réseau : `Router(config-if)# ip address [ip-address] [subnet-mask]`
    - Exemple : `Router(config-if)# ip address 192.168.1.1 255.255.255.0`
- Activer l'interface : `Router(config-if)# no shutdown`
- Quitter le mode de configuration d'interface : `Router(config-if)# exit`

### Configuration d'une adresse IP sur un commutateur (pour la gestion)
- Entrer en mode de configuration d'interface VLAN (pour la gestion) : `Switch(config)# interface vlan [vlan-id]`
    - Exemple : `Switch(config)# interface vlan 1`
- Assigner une adresse IP et un masque de sous-réseau : `Switch(config-if)# ip address [ip-address] [subnet-mask]`
    - Exemple : `Switch(config-if)# ip address 192.168.1.2 255.255.255.0`
- Activer l'interface VLAN : `Switch(config-if)# no shutdown`
- Définir la passerelle par défaut : `Switch(config)# ip default-gateway [gateway-ip]`
    - Exemple : `Switch(config)# ip default-gateway 192.168.1.1`
- Quitter le mode de configuration d'interface : `Switch(config-if)# exit`


## Faire du routage statique
- Entrer en mode privilégié : `Router> enable`
- Entrer en mode de configuration globale : `Router# configure terminal`
- Configurer une route statique : `Router(config)# ip route [network] [mask] [next-hop-ip or exit-interface]`
  - Exemple : `Router(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.2`

## Faire du routage dynamique en RIP
- Entrer en mode de configuration globale : `Router(config)#`
- Activer le routage RIP : `Router(config)# router rip`
- Configurer la version de RIP : `Router(config-router)# version 2`
- Annoncer les réseaux : `Router(config-router)# network [network-address]`
  - Exemple : `Router(config-router)# network 192.168.1.0`
- Quitter le mode de configuration RIP : `Router(config-router)# exit`

## Faire du routage dynamique en OSPF
- Entrer en mode de configuration globale : `Router(config)#`
- Activer le routage OSPF : `Router(config)# router ospf [process-id]`
- Annoncer les réseaux et définir les aires OSPF : `Router(config-router)# network [network-address] [wildcard-mask] area [area-id]`
  - Exemple : `Router(config-router)# network 192.168.1.0 0.0.0.255 area 0`
- Quitter le mode de configuration OSPF : `Router(config-router)# exit`

## Mettre SSH sur un routeur
- Vérifier le support SSH : `S1# show ip ssh`
- Configurer le domaine IP : `S1(config)# ip domain-name cisco.com`
- Générer des paires de clés RSA : `S1(config)# crypto key generate rsa`
- Configurer l'authentification des utilisateurs : `S1(config)# username root secret gonfle!`
- Configurer les lignes de VTY pour SSH :
```
S1(config)# line vty 0 15
S1(config-line)# transport input ssh
S1(config-line)# login local
S1(config-line)# exit
```
- Activer SSH version 2 : `S1(config)# ip ssh version 2`

## Créer des VLAN différents
- Créer un VLAN avec un numéro d'identité valide :
    - Commande : `Switch(config)# vlan vlan-id`
    - Exemple : `Switch(config)# vlan 10`

- Indiquer un nom unique pour identifier le VLAN :
    - Commande : `Switch(config-vlan)# name vlan-name`
    - Exemple : `Switch(config-vlan)# name SalesDept`

- Passer en mode de configuration d'interface :
    - Commande : `Switch(config)# interface interface-id`
    - Exemple : `Switch(config)# interface GigabitEthernet0/1`

- Définir le port en mode d'accès :
    - Commande : `Switch(config-if)# switchport mode access`

- Affecter le port à un VLAN :
    - Commande : `Switch(config-if)# switchport access vlan vlan-id`
    - Exemple : `Switch(config-if)# switchport access vlan 10`

- Afficher les informations des VLAN :
    - Commande : `S1# show vlan brief`
    - Exemple : `S1# show vlan brief`

- Activer le trunking sur un port :
    - Commande : `S1(config-if)# switchport mode trunk`
    - Exemple : `S1(config-if)# switchport mode trunk`

- Désactiver la négociation DTP (pas nécessaire si configuration manuelle) :
      - Commande : `S1(config-if)# switchport nonegotiate`

- Réactiver le protocole DTP dynamique (pas nécessaire si configuration manuelle) :
      - Commande : `S1(config-if)# switchport mode dynamic auto`

- Pour configurer manuellement les VLANs autorisés sur un lien trunk, utiliser :
      - Commande : `S1(config-if)# switchport trunk allowed vlan vlan-id1, vlan-id2`
      - Exemple : `S1(config-if)# switchport trunk allowed vlan 10,20`

## Configurations de VLAN et de trunk de commutateur S2
```
S2(config)# vlan 10
S2(config-vlan)# name LAN10
S2(config-vlan)# exit
S2(config)# vlan 20
S2(config-vlan)# name LAN20
S2(config-vlan)# exit
S2(config)# vlan 99
S2(config-vlan)# name Management
S2(config-vlan)# exit
S2(config)# interface vlan 99
S2(config-if)# ip add 192.168.99.3 255.255.255.0
S2(config-if)# no shut
S2(config-if)# exit
S2(config)# ip default-gateway 192.168.99.1
S2(config)# interface fa0/18
S2(config-if)# switchport mode access
S2(config-if)# switchport access vlan 20
S2(config-if)# no shut
S2(config-if)# exit
S2(config)# interface fa0/1
S2(config-if)# switchport mode trunk
S2(config-if)# no shut
S2(config-if)# exit
S2(config)# end
```

## Routage entre les différents VLANs avec un Routeur

### Configuration du Routeur pour le Routage Inter-VLAN
1. **Création des sous-interfaces** : Chaque sous-interface sur le routeur correspondra à un VLAN sur le commutateur. Cela permet au routeur de router le trafic entre ces VLANs.
   
   - Commande : `Router(config)# interface [interface-type][interface-number].[subinterface-number]`
   - Exemple : `Router(config)# interface GigabitEthernet0/0.10`

2. **Attribution d'une adresse IP et association avec un VLAN** : Définir une adresse IP pour chaque sous-interface et spécifier le VLAN auquel elle appartient.
   
     - Commande : `Router(config-subif)# encapsulation dot1Q [vlan-id]`
     - Commande : `Router(config-subif)# ip address [ip-address] [subnet-mask]`
     - Exemple pour VLAN 10 :
     ```
     Router(config-subif)# encapsulation dot1Q 10
     Router(config-subif)# ip address 192.168.10.1 255.255.255.0
     ```

3. **Activation des sous-interfaces** : Activer chaque sous-interface pour qu'elle soit opérationnelle.
   
     - Commande : `Router(config-subif)# no shutdown`
     - Exemple : `Router(config-subif)# no shutdown`
