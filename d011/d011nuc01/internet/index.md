% Accès Internet dans les salles d011, d012, c007
%
% jeudi 20 octobre 2016

----

# d011 et d012

## Schéma 

Afin de respecter les termes de la charte Renater, de la réglementation et de
la loi, l'accès Internet se fait à la demande, et en termes d'accès à des
services distants et de manière identifiée.

Le principe est d'établir une liaison _PPP_ entre la machine `d011nuc01` et une
machine distante (`163.172.46.88`), disposant à la fois d'un accès Internet IPv4 et
IPv6 public, ainsi qu'une capacité réseau importante (accès natif en ethernet
1 Gbits/s. directement relié sur un backbone européen).

Un accès distant en protocole _ssh_ est utilisé afin d'initier la session et de
transporter les données correspondant à ce service d'accès. Un rebond via la
machine `bastion-out.univ-nantes.fr` est utilisé pour établir une connexion _ssh_
transportant de manière transparente les trames de la liaison _PPP_.

## Traversée transparente de `bastion-out.univ-nantes.fr`

Ce script permet de définir une redirection via la machine `bastion-out.univ-nantes.fr`
et ajoute une règle de translation d'adresses pour utiliser cette redirection.

~~~~ [.sh .numberLines]
#!/bin/bash

if test "$#" -ne 2
then
    echo "$0" destination port >&2
    exit 1
fi

DEST_IP="$1"
DEST_PORT="$2"
LOCAL_PORT_PREFIX=2     # => 2wxyz
USER=lehn-r		# à remplacer par le nom d'utilisateur LDAP de l'université

function get_local_port() {

        local ip="$1"

        # conversion format adresse A.B.C.D => 00a00b00c00d (nb caractères fixe, chaque
        # octet de l'adresse IP est représenté en décimal sur 3 chiffres remplis de 0 à
        # gauche).
        #

        local fix_ip="$(printf '%03d%03d%03d%03d\n' $(echo "$ip" | tr . ' '))"

        # choix d'un port en prenant 4 chiffres consécutifs dans cette adresse en
        # examinant d'abord la partie droite de celle-ci.

        local i
        for i in $(seq 12 -1 4)
        do
                local port="2$(echo "$fix_ip" | cut -c$((i-3))-$i)"
                if lsof -n -iTCP -sTCP:LISTEN | grep -Eq "TCP.+:$port " # port utilisé ?
                then
                        if test "$i" -eq 4
                        then
                                # plus de port disponible pour la redirection
                                echo "Plus de port local disponible pour $ip:$DEST_PORT" >&2
                                port=''
                                break
                        fi
                        continue
                else
                        break
                fi
        done

        if test -n "$port"
        then
                echo $port
                return 0
        else
                return 1
        fi
}

LOCAL_PORT=$(get_local_port $DEST_IP)

set -v

ssh -f -N -L $LOCAL_PORT:$DEST_IP:$DEST_PORT $USER'@bastion-out.univ-nantes.fr'
sudo iptables -t nat -A OUTPUT -d $DEST_IP -p tcp --dport $DEST_PORT \
        -j REDIRECT --to-port $LOCAL_PORT
~~~~

## Liaison PPP à la demande

La liaison _PPP_ est encapsulée dans une session _ssh_ au moyen de la commande
suivante :

~~~~ [.sh .numberLines]
remi@d011nuc01:~$ sudo /usr/sbin/pppd nodetach noauth nodeflate \
     pty "/usr/bin/ssh remi@163.172.46.88 sudo /usr/sbin/pppd nodetach notty noauth" \
     ipparam vpn 10.0.8.1:10.0.8.2     
Using interface ppp0                                                                     
Connect: ppp0 <--> /dev/pts/21                                                           
remi@163.172.46.88's password:                                                           
BSD-Compress (15) compression enabled                                                    
local  IP address 10.0.8.1                                                               
remote IP address 10.0.8.2                                                               
~~~~

