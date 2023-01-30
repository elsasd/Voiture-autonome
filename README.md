# Projet de Programmation synchrone

Ce répertoire contient le projet du cours de Programmation synchrone du M2
Informatique de l'Université de Paris et de l'École d'Ingénieur Denis Diderot.

Le [sujet](sujet/sujet-projet.pdf) contient tous les détails.


Dans ce dossier, nous devons mettre un rapport de projet en pdf !


# pour compiler : 
taper make et ca produit un fichier ept
scontest -e (-a pour audio flippant)  qui prend des arguments pour lancer la simulation (prend au moins une carte sur laquelle lancer la sim)
affiche : H = angle de la voiture, V = vitesse ? , T = TEMPS PASSE, S = score -> perd si tu depassses limitation de vitesse ou tu ecrases un Nico Le Pieton


# Dans les fichier C donné:
 - L'interface graphique 
 - acces au fichier disque 
 - Calculs trigonometriques, etc 

Lecture pas obligatoire mais si il y a un truc que tu captes aps check la bas.

# Les fichiers ept:
 - Code de simulation de la ville
 - Controleur du vehicule (vide!)
 

# Detail de quelques fichiers :
 - mathext.* -> 3 extentions possibles 
    code ecris en C, et un fichier epi (= opposé a ept, interface de module heptagone): 
    pleins d'outils de maths defini en external(pas ecris en ept mais en c)
 - debug -> pour debugger ecrire un bail du style :
    ```
    node nat() returns (o : int)
    let 
        o = 0 fby (0+1)
        () = Debugd.dbg("Hello\n");
        () = Debug.dbg_int("o = ", o);
    tel
    ```
 - Dans le dernier fichier: type world va nous donner un moyen de forcer l'ordre de debug (pas utile)
    ```
    node nat() returns (o : int)
    var w = world;
    let 
        o = 0 fby (0+1)
        (_) = Debugd.d_string(w, "Hello\n");
        w = Debug.d_int(Debug.d_init(), o);
    tel
    ```
 - Fichier trace aussi pour deboggage
 - Globals.ept : 
    contient plein de def globale utile au sujet : 
    ex: type couleur pour les couleurs des routes 
    aussi plein de constantes vous irez check les bgs
    timestep -> temps de la simulation
 - utiles.ept : 
    Quelques trucs pas necessairement a utiliser mais si besoin c'est la
    Def de noeuds;
    Countdown: compteur qui prend une suite de boool e et ini suite de int;
    Conteur qui a chaque fois que e vaut vrai, decroit
    Comparaison de couleur !

 - Challenge.ept :
  coeur de la simulation : Il a 2 entrées (initial ph et top): initial_ph = etat initial de la simulation
    top = vrai quand simulation lancée, 
    renvoie un masse de truc frerot :'(
    Status: Soit preparé a se lancer, soit lancé, soit arrivé a bon port, soit erreur
    sign : feu de signalisation, 
    Evt :

 - Control.ept : C'est que ca a changer !
  controller : entree : sense type sensor et iti type itielets
                sortir: rspeed : wheels, arriving: bool
    On a acces au roues motrices de la voiture 
        rspeed {left : (sa vitesse), right : (sa vitesse)}
            vitesse en radiant pas sec
        arriving : bool qui dit que t'es arrivé quand tu penses etre arrivé ;)
    On controle que roues arrieres 
    
So quoi les entrées : 
    Sensor et itinetaires :

#Type Itelets :
    Tab du type itielets qui est un tab 
    Une etape est un engeristrement a 2 sens:
        -action : Go |turn|stop
        -param (depend de action -> go + param 20 : ne depasséz pas 20 m.s| Turn : param en degrée pour la rotation de la voiture | Stop: param inutile)
        
# un itineraire:
iti 5 (il y aura 5 trucs):
Go 20
go 15
turn -44, 
go 30 ,
Stop 0;
end 
    
Mais comment on sait quand on passe d'une etape a l'autre ? 
 
##regle de passage d'etape:
Sur la route, votre voiture a des sensor pour savoir des choses sur son environement :
Sous a voiture, on a la couleur de la route parcouru: RC pour road Color situé au centre
Et devant la voiture FC pour Front Color, 
Et sonar: un entié qui detecte si ily a un passant, si c'est fort -> pas de passant 
        
Pour passer une etape, c'est apres avoir vu du vert !
Rouge pour le feu 
Pas de demarcation dur des couleurs -> degradés 
    
Truc rouge devant -> c'est un feu 



#Pour commencer a taff :

1- Carte 00: suivre ligne droite avec angle initial correct, mais en respectant vitesse max;
2- Suivi de route: Corriger un angle initial desaxé sur 00.map puis ensuite sur carte incurvée genre 02.map
3- Tourner sur elle meme d'un angle specifié
4- Detecter les marquage d'etat au sol 
5- Detecter les feux (et les respecter ptdr)
6- Detecter les obstacles au sonar
7- Tester toutes les cartes (10 cartes qui comptent vraiment)