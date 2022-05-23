# Rôle ansible: wireguard

## Description

Ce rôle est compatible avec LXD et permet de configurer une ou plusieurs interfaces wireguard en mode serveur ou client. La configuration se fait à l'aide des outils wireguard (wireguard-dkms et wireguard-tools). Le rôle a été écrit pour Debian 11.x mais est probablement compatible avec ces dérivés, Ubuntu compris.

## Usage

Définir un fichier de configuration de l'hôte dans `host_vars` pour utiliser ce rôle.
La définition de `ipv4` n'est pas obligatoire si `ipv6` est renseigné et vice-versa.

### Mode client

```yaml
---
instance:
  roles:
    wireguard:
      - name: wg0
        mode: client
        interface:
          ipv4: 192.168.100.1/24
          ipv6: fd00::a100:b001/64
          port: 51820
        peers:
          - comment: <server_fqdn>
            pubkey: <server_public_key>
            ip: <server_ip>
            port: <serve_listen_port>
            allowed_ips: 0.0.0.0/0, ::/0
```

La paire de clés se trouve dans `/srv/wireguard`.

### Mode serveur

Récupérer la clé publique sur les clients pour pouvoir créer la configuration du serveur.

```yaml
---
instance:
  roles:
    wireguard:
      - name: wg0
        mode: server
        listen:
          ipv4: <server_listen_ipv4>
          ipv6: <server_listen_ipv6>
        interface:
          ipv4: <server_vpn_ipv4>
          ipv6: <server_vpn_ipv6>
        peers:
          - comment: <client1_fqdn>
            pubkey: <client1_public_key>
            allowed_ips: 192.168.100.1/32, fd00::a100:b001/128

          - comment: <client2_fqdn>
            pubkey: <client2_public_key>
            allowed_ips: 192.168.100.2/32
```

## Support

Ce rôle a été réalisé dans le cadre du projet [Chaos](https://www.ykn.fr/chaos/) et est donc fourni en l'état. Toute demande de modifications est la bienvenue du moment que celle-ci ne dégrade pas le fonctionnement au sein du projet [Chaos](https://www.ykn.fr/chaos/) (elles seront testées).
