% Routeurs Alcatel 7750 SR7
%
% 2016/09/27 10:14:26



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

# Alimentation

![Photo de l'alimentation](http://d3d71ba2asa5oz.cloudfront.net/23000103/images/jv-rya-c-sr7pem2-01.jpg)

## Alimentation -48V DC (courant continu)

Le **-**48V n'est pas une coquille (cf. p7 du _Data sheet_), c'est assez
original, le **-** est en haut et le **+** en bas, ce dernier semblant
être la tension de référence (masse).

La tolérance de l'alimentation va jusqu'à 80V DC, 60A. Le câblage
électrique doit donc être en conséquence 
([minimum 2,5mm² (AWG 14), idéalement 4mm² (AWG 12) voire plus...](http://www.solar-wind.co.uk/cable-sizing-DC-cables.html)
).

# Console

Port console, série asynchrone (v24/rs232c), 9600 bauds, 8 bits, sans
parité, 1 bit stop.

~~~~ [.sh]
$ sudo chmod 666 /dev/ttyUSB0
$ cu --line /dev/ttyUSB0 --speed 9600 # cu -l /dev/ttyUSB0 fonctionne aussi
Connected.

Login:   
Login: 
Login: ~[d012nuc01].   # Touche ~ , attendre 1 sec., touche .
Disconnected.
~~~~

# Références techniques

[7750 SR OS Basic System Configuration Guide](https://infoproducts.alcatel-lucent.com/cgi-bin/dbaccessfilename.cgi/9300700701_V1_7750%20SR%20OS%20Basic%20System%20Configuration%20Guide%208.0r1.pdf)

[Quick specs](https://www.hpe.com/h20195/v2/GetDocument.aspx?docname=c04640654&doctype=quickspecs&doclang=EN_US&searchquery=&cc=us&lc=en)

[Data sheet](https://www.hpe.com/h20195/v2/GetDocument.aspx?docname=4AA5-9070ENW&doctype=data%20sheet&doclang=EN_US&searchquery=&cc=us&lc=en)

[Modular Ethernet Routers
Alcatel-Lucent 7750-SR7 Switch Fabric and Control Processor Module DC Power Chassis Starter Bundle
(JL136A)](http://www8.hp.com/us/en/products/networking-routers/product-detail.html?oid=8148014)

[Alcatel-Lucent 7750 SR7 Switch Fabric and Control Processor Module DC Power Chassis Starter Bundle (JL136A) - Product documentation](https://www.hpe.com/h20195/v2/default.aspx?cc=us&lc=en&oid=8148014)

[Alcatel Unleashed](http://www.alcatelunleashed.com/)

[Shodan.io](https://www.shodan.io)

