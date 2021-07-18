# Home-Assistant-Config

Voici ma configuration concernant l'ensemble des éléments de domotique installés dans ma demeure. J'ai commencé à m'intéresser à la domotique particulièrement à l'époque du X10. Mais à l'époque ce système avait trop de désavantages et j'ai rapidement abandonné. Au mois de mai 2017, je me suis procuré un Google Home Mini, afin de voir les possibilités de ce gadget. J'ai beaucoup aimé, mais après avoir acheté d'autres bidules de domotique (interrupteurs, lumières) que je me suis aperçu qu'il était difficile d'harmoniser et de faire collaborer différents éléments de différentes compagnies. C'est en cherchant une solution pour connecter mes appareils à Siri que je suis tombé sur Home Assistant. C'est ce logiciel qui m'a redonné le goût de me relancer dans la domotique grand public.

J'ai été programmeur dans mon ancienne vie, à l'époque du VisualBasic, TurboPascal, etc. Les chemins m'ont fait prendre plutôt la route de l'architecture de système Windows. Je n'ai donc pas la compétence de certains gourous de Home Assitant qui développent et améliorent diverses pièces du logiciel. Mais j'appends actuellement le Python et j'espère faire prochainement mon premier statut "Hello World"! Je suis d'ailleurs en apprentissage de GitHub! Beaucoup d'apprentissages en même temps. 

## Composantes 
### Home Assistant

Mon HA a jusqu'en juillet 2019 roulé sur Raspberry Pi 3 Model A+ (1.4GHz 64bit quad-core). Avec le système d'exploitation HASSIO qui est inclus dans l'image, c'est la façon la plus simple et efficace. Par contre, j'ai commencé avec des cartes SD assez vielles et assez "made in China". Une base de données est créée sur celle-ci lors de l'installation de HA et le taux d'écriture et de lecture est assez élevé. Puisqu'une carte SD est relativement lente pour l'écriture et que le nombre d'écritures sur ces types de cartes est assez limité, j'ai eu beaucoup de problèmes de corruption. Après une petite recherche sur internet, je me suis procuré une carte Samsung EVO Plus 64GB microSDXC qui a été beaucoup plus fonctionnelle. De plus, j'ai déplacé la BD (Marina)SQL sur mon NAS au mois de mai 2019 et la différence a été notable en termes de stabilité et de performance au niveau de la page des historiques dans HA.

Puis en août 2019, j'ai déplacé le HA sur mon NAS (Asustor AS5104T, Intel Celeron @ 2GHz, 4Gb RAM) à l'intérieur d'un container Docker. Lorsque l'on installe HA sur autre chose que sur un Raspberry avec HASSIO, il faut savoir que l'on perd certaines fonctionnalités disponibles par l'interface graphique (mise à jour, add-on, etc.) Des étapes plus manuelles via la ligne de commande seront alors nécessaires. Je suggère que vous passiez par un Rasberry pour vous familiariser avec les configurations du HA. D'ailleurs, une de mes prochaines étapes est d'installé une clé USB Z-Wave à HA pour éliminer la nécessité du chez-soi Smarthings que j'utilise pour mes appareils Z-Wave. Je ne sais pas encore si ça sera possible.

** UPDATE **
En mai 2020, je suis retourné sur un Raspberry PI 4, mais en laissant la BD sur le NAS.  J'ai utilisé une carte SD plus adapté aux application (SanDisk Extreme Pro MicroSDXC UHS-I U3 A2 V30 64GB) et jusqu'à présent tout fonctionnement bien.

### Philips Hue (https://www.philips-hue.com/)

* 2 x Color Lightstrip dans la chambre à coucher
* 1 x Color Candle Hub E14 dans la chambre à coucher
* 1 x White A19 pour un garde-robe
* 2 x White GU10 dans la cuisine
* 1 x Hue Motion Sensor
* 1 x Hue Dimmer switch
* 1 x Hue Bridge
* 1 x Hue Button

Les lumières et le détecteur de mouvements sont intégrés à HA avec le module d'intégration standard.
La Hue Dimmer switch et le Hue Button ne peuvent être utilisé par HA.


### TP-Link (https://www.tp-link.com/fr-ca/home-networking/smart-plug/)

* 1 x HS110
* 4 x HS105

Ces petites prises sont superbes. Facile d'intégration via l'interface de HA, elles sont rapides puisqu'elle sont accédées sans passer par le cloud. Et bien abordables. Les prises HS105 me permettent d'allumer et de fermer mon ordi, deux lumières et un Lectrofan (oui, une machine a bruit!). La prise HS110 offre aussi du "power monitoring". Attachée à un air climatisé, elle m'indique si l'air climatisé est en fonction. Ceci, même si quelqu'un a fermé celui-ci directement ou via à manette.

### Withings (https://www.withings.com/)

* 2 x Sleep (Capteur de sommeil)
![enter image description here](https://raw.githubusercontent.com/eplantequebec/readme-images/master/withingsSleep.png)

Bien avant d'avoir mon 1er Google Home Mini, j'avais déjà ces deux produits en possession. Il n'y a pas d'intégration possible avec HA directement, mais j'ai trouvé amusant de connecté ceux-ci à IFTTT, puis d'envoyer un Webhook à HA à chaque levé ou couché du lit. J'ai ainsi deux entités (boolean) dans HA qui détermine si je suis couché ou non. Ainsi l'alarme ne part que si je suis au lit et si elle démarre, elle se ferme dès que je me lève! :)

*** UPDATE ***
Puisque maintenant IFTTT n'autorise que 3, je ne l'utilise plus.  Je cherche donc une façon de détecter ma présence au lit.

### Xiaomi

* Roborock Vaccum

![enter image description here](https://raw.githubusercontent.com/eplantequebec/readme-images/master/XiaomiVacuum.png)

L'intégration à HA se fait via l'édition du fichier de configuration. Le plus difficile est de rapatrier le Token requis pour communiquer avec les serveurs de Xiaomi.

* Xiomi Boutons
https://fr.aliexpress.com/item/32824750261.html?albpd=en32824750261&acnt=708-803-3821&aff_platform=aaf&albpg=743612850714&netw=u&albcp=7386552844&sk=UneMJZVf&trgt=743612850714&terminal_id=96fb4c02a36244ec8551b4a63271ce2d&tmLog=new_Detail&needSmbHouyi=false&albbt=Google_7_shopping&src=google&crea=en32824750261&aff_fcid=4467bb12100241c8983b60a78c8b6270-1626651562999-08717-UneMJZVf&gclid=CjwKCAjwos-HBhB3EiwAe4xM9yy0WejzgaaPungtuXAUqBAlJug35i0e1OCe7sTs8yVbkbwYZgkykxoCbLUQAvD_BwE&albag=80241711349&aff_fsk=UneMJZVf&albch=shopping&albagn=888888&isSmbAutoCall=false&aff_trace_key=4467bb12100241c8983b60a78c8b6270-1626651562999-08717-UneMJZVf&device=c&gclsrc=aw.ds

### MyQ

* Contrôleur de porte de Garage MyQ (MYQ-G0201C)

L'intégration à HA se fait via l'édition du fichier de configuration, simplement en indiquant les informations nécessaires pour se connecter au cloud de MyQ. J'ai essayé à l'époque le très prometteur Garadget (https://www.garadget.com), mais à l'époque il était incompatible avec les Chamberlain. Je crois qu'ils ont réglé le problème maintenant. De toute façon, les deux utilisent le cloud.

### Aoetec (https://aeotec.com/)

* 1 x Nano Dual Switch ZW132 (with power monitoring)
* 1 x Nano Dual Switch ZW139 (without power monitoring)
* 1 x Nano Dimmer ZW111-A (with power monitoring)
* 1 x Aeotec Sensor 6

Ces nano interrupteurs sont vraiment intéressant. Elles sont compatibles Z-Wave et elles m'ont permis de rendre des lumières intelligentes, sans remplacer mes interrupteurs, qui sont non standard, mais que nous aimons bien. (Interrupteurs Adorme de Legrand). Elles détectent sans problème les changements d'état des interrupteurs et sont assez rapides malgré le fait que HA doit passé par le hub SmartThings qui gère les appareils Z-Wave. Le seul désavantage est le prix qui est trop élevé. 

Malgré le fait que je n'ai pas de neutre pour la lumière de la cuisine, j'ai pu utiliser la switch ZW111. Lorsque le gradateur était au maximum, la lumière commençait à clignoter. Avec HA, j'ai mis une règle qui, lorsque la prise est placée à 90%, la puissance est redescendue automatiquement à 90%, niveau où il n'y a plus de clignotement. 

La switch ZW132 est branché sur deux lumières de la salle de bain. Or, une seule se rend à HA. Dans l'application SmartThings, les deux apparaissent sans problème. Je crois que le problème est entre SmartThings et HA. Faudra que j'approfondisse la question un jour. Pour l'instant, je contourne le problème en créant des scènes (On et Off) pour la seconde lumière et j'appelle la scène à partir de HA.

Le Sensor 6 pour la porte d'entrée fonctionne bien. Encore ici, le seul problème est le prix. Les Neo Coolcam sont bien aussi à moitié du prix.

### Logitech

Le Hub Harmony s'intègre très bien avec HA. Par contre, il a deux gros défauts. Le prix et le fait que l'on doit passer par le cloud (comme la majorité des autres hubs d'ailleurs). Il est facile à configurer et l'ensemble des appareils avec l'ensemble des commandes seront importés dans HA. Si vous avez besoin plus d'un Hub, la facture grossira en conséquence. Je suggère plutôt les Broadband Mini IR, qui sont une fraction du prix.

*** UPDATE ***
Je me suis débarrasé de mon hub logitech.  Je préfère maintenant avoir des Broadlink IR Mini 3.  Ils sont très très moins cher et me permettent une plus grande souplesse.

### MagicLight Controler

1 x SuperNight WiFi Wireless LED Smart Controller
1 x MagicLight WiFi RGB LED Controller 

En fait c'est deux fois le même produit.  Un est sûrement un "rebranding" de l'autre produit.  Quoi qu'il en soit, le produit fonctionne très bien.  Le tout est en Wifi.  Après avoir ajouté les adresses IP dans la configuration de HA, celui-ci communique efficacement directement avec l'appareil, sans passer par le cloud.

### SmartThings 

1 x Hub SmathThings
3 x SmartThings Button 

J'ai aussi essayé Vera et Wink, mais j'ai finalement opté pour le SmartThings, parce que c'était le moins pire. Il sert surtout pour les composantes qui sont Z-Wave, ZigBee (éventuellement) ou tout autre appareil qui seraient compatibles SmarthThings, mais pas HA directement. J'espère un jour bancher un concentrateur Z-Wave directement sur HA via le USB, mais maintenant que celui-ci fonctionne sur mon NAS, ça pourrait être difficile, voir impossible. Sinon, j'aimerais bien redonner une chance au hub Vera. Celui-ci fonctionne localement pour le contrôle des appareils.

Les bouton SmartThings, sont très utiles.  Ils ont trois statut qu'ils envoie au hub de Samsung et qui sont ensuite relayé à HA.  Un clique, un double-clique ou un longue-pression peut-être alors identifier dans HA lors de la configuration d'un déclancher dans une automation.

*** UPDATE ***
J'ai abandonné SmartThings au profit de Vera.  Puisque les boutons étaient compatible seulement avec SmartThings, j'ai dû abandonné ceux-ci.  De toute façon, ils commençaient à agacé grandement la conjointe, car ceux-ci perdaient souvent la communication avec le HUB.

### VERA

Au début de ma découverte de HA, j'ai essayé 3 hubs (SmartThings, Vera et Wink).  Je n'ai pas aimé Wink et mon préféré fût SmartThings.  Afin de diminuer ma dépendance au cloud, j'ai décidé en décembre 2019 de refaire un essai avec Vera.   Celui-ci est donc maintenant utilisé pour mes device Z-Wave et jusqu'à présent tout va bien.  

*** UPDATE ***
J'ai aussi abandonnée le VERA.  Je gère maintenant directement le ZigBee et le Z-Wave à partir du Raspberry Pi.   Pour le Z-wave, j'utilise le Aeotec Z-Stick Gen5 Plus.  Pour le ZigBee, j'utilise le RASPBEE II ZigBee.

### Broadlink

2 x Broadlink RM Mini 3
2 x Broadlink A1,Smart Home Sensor

J'adore les Broadlink RM Mini 3. HA peut envoyer pratiquement n'importe quel code infrarouge et le champ d'action me semble plus grand que l'Harmony de Logitech. L'apprentissage est un peu ardu, puisque vous devez rapatrier le code pour chaque touche de la télécommande afin de les intégrer à HA. Mais le temps-réponse est superbe puisque tout est local. L'appareil s'alimente simplement par une prise USB. Et le coût est d'environ $30 CND. 

L'apprentissage est maintenant grandement simplifié dans Home Assistant.  Celui-ci permet maintenant l'apprentissage dans Home Assistant.

Les deux Broadlink A1 me servent surtout pour comme hygromètre. Il est aussi utilisable pour la température. Par contre, pour la qualité de l'air, le senseur de mouvement et de niveau sonore, il est trop peu précis pour être utilisé. De toute façon le produit est discontinué. Si vous avez besoin simplement d'un thermomètre, vous pouvez utiliser les SmarthThings Button qui ont un senseur intégré. 

*** UPDATE ***
Je me suis débarrassé de ces éléments, ils sont un peu trop voyant.  J'ai mis des détecteurs d'humidité/température Sonoff.

### Prise électrique Enerwave (Z-Wave)

https://www.amazon.ca/Enerwave-ZW15RM-PLUS-App-Controlled-Automation-Interchangeable/dp/B07991PBH9

### EventGhost
Pour controler l'ordinateur via des Webhook.   Ce qui permet à HA d'appeller ce logiciel pour qu'il (exemple) ferme l'écran ou le son la nuit.

### IFTTT
Ce service est un service de dernier recours pour HA.  Je l'utilise principalement pour déclencher des scripts HA suite à une commande vocale à Google.  Exemple, la commande vocale "Ok Google, prépare le dodo." execute un script HA qui éteint tout dans la maison et prépare le dodo.

### Home Assistant Companion.
L'application IOS indispensable! 


### Kevo Smart Lock
Je n'ai pas rafolé de ce verrou.  Le déverrouillage prenait souvent plusieurs "touches" afin de s'executer.  Le Kevo plus doit être acheté à part ce qui fait monter la facture.  Et même avec ce module, l'intégration avec HA était pénible.  L'achat d'un verrou August fût une bénédiction.

### August
Facile à intégrer dans HA, Il est Z-Wave et se déverouille dès qu'on approche de la maison.  C'est un coup de coeur.  Beaucoup plus efficace que Kevo.

### Sinope Thermostat
![enter image description here](https://raw.githubusercontent.com/eplantequebec/readme-images/master/Sinope.png)

Produit du Québec que j'ai acheté il y a 5 ans.  Le hub a une intégration HA via HACS.  Fonctionne admirablement bien sauf pour un convecteurs.

### Sonoff - Contrôleur pour ventilateur de plafond
![enter image description here](https://raw.githubusercontent.com/eplantequebec/readme-images/master/sonoffFan.png)

Lorsque j'ai acheté mes deux ventilateur de plafond, je voulais un système controlable via un iPhone.  Ça fonctionnait correctement, mais lorsque j'ai eu mon Google Home, la première chose que je voulais controller avec celui-ci était mes ventilateurs.  J'ai acheté un Sonoff iFan2.  Malheureusement, quelques semaines après je decouvrait Home Assistant et celui-ci n'était pas intégrable à celui-ci. 

La plupart des modules Sonoff ont la particularité de pouvoir changer le Firmware et d'installer un firmware opensource développé pour les modules ESP8266.  Pendant longtemps, des compétence en soudure étaient requises pour changer se Firmware.  Il y a eu aussi certain modules qui pouvaient être changer via Wifi (OTA), mais c'était complexe et j'ai finalement laissé tombé après avoir essayé une fois.

Mais en juillet 2020, je découvrait l'intégration d'AlexxIT via un vidéo de DrZzs.   Le module disponible via HACS fonctionne à merveille et permet non seulement l'intégration de la plupart des modules Sonoff, mais permets à HA de communiquer localement avec ces modules.  Plus de cloud.

### Sonoff - Senseur

J'ai intégré des senseurs de température et d'humidité Sonoff SNZB-02 qui me https://sonoff.tech/product/smart-home-security/snzb-02/.  Peu cher et efficace.  Ils me permettent de régler la vitesse des ventilateur de plafond en fonction de la température.


<hr>

### Optimisé pour iPhone
<img src="https://raw.githubusercontent.com/eplantequebec/readme-images/master/screnshot.jpg" >

xiomi card from:
https://github.com/sharetheloveio/sharethelove.io/blob/master/picture-elements/xiaomi-vacuum-card.md
