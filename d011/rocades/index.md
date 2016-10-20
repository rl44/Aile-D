% Rocades ltd001 - ltd011
%
% 2016/09/29 18:06:02


~~~~ [.sh]
$ pandoc --standalone --to html5 --toc <index.md >index.html             
~~~~

----

<style type="text/css" media="all">
    body {
        counter-reset:  h1counter;
    }
    
    body > h1:before {
		content: counter(h1counter) ".\0000a0\0000a0";
		counter-increment: h1counter;
        counter-reset: h2counter;
    }
    
    body > h2:before {
        content: counter(h2counter) ".\0000a0\0000a0";
        counter-increment: h2counter;
        counter-reset: h3counter;
    }
    
    body > h3:before {
        content: counter(h2counter) "." counter(h3counter) ".\0000a0\0000a0";
        counter-increment: h3counter;
    }

</style>

# Interconnexion à la date du 27 septembre 2016

~~~~
From: Patrice Gonnord <patrice.gonnord@univ-nantes.fr>
Subject: =?UTF-8?Q?r=c3=a9seau_vers_D012?=
To: =?UTF-8?Q?R=c3=a9mi_Lehn?= <remi.lehn@univ-nantes.fr>,
 =?UTF-8?Q?Beno=c3=aet_Parrein?= <benoit.parrein@polytech.univ-nantes.fr>,
 Fabien Picarougne <fabien.picarougne@univ-nantes.fr>
Message-ID: <6623099b-47de-044b-5355-b58295777a96@univ-nantes.fr>
Date: Tue, 27 Sep 2016 15:50:33 +0200
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:45.0) Gecko/20100101
 Thunderbird/45.3.0
MIME-Version: 1.0
Content-Type: text/plain; charset=utf-8; format=flowed
Content-Transfer-Encoding: 8bit

Bonjour,
suite à notre discussion d'hier voici la liste des vlans propagés vers la salle D012

vlan 254 - enseignant/recherche - 172.19.32.0/21
vlan 255 - pedagogie - 172.19.64.0/21
vlan 259 - visio
vlan 4041 - TOIP

la répartition sur les ports cuivres
1 - actuellement vlan 254 natif - à changer selon votre usage
2 - actuellement vlan 254 natif + vlan 4041 tag - connecté sur port POE
3 - vlan 254 - 255 - 259 - 4041 tag
4 - vlan 254 - 255 - 259 - 4041 tag

la répartition fibre multimode - SFP-1G
1/2 - vlan 254 - 255 - 259 - 4041 tag

Cordialement,
Patrice 
~~~~
