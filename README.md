Programmation bas niveau (M2101) TP01
=====================================
 
Dans ce TP, nous allons développer des applications en langage C pour découvrir les mécanismes de bas niveau présents dans les ordinateurs. Pour rester assez proche du module du premier semestre (élaboration d'un processeur) nous allons utiliser un micro-contrôleur de la famille Microchip : le PIC 18F452.

L'environnement de développement est l' EDI MPLAB. La simulation matérielle se fera à l'aide de PROTEUS VSM (ISIS).

#Q1
Créer un projet sous MPLAB (Wizard avec 18F452, C18 Toolsuite) avec un fichier source contenant les lignes suivantes.

```

/********************************************/
#define byte unsigned char

int a,b ;
long int c ;
short int d ;
float f;
byte i,j,k ;

void main(void)
{
a = 35 ;
b = 0x56 ;

c = 0x35671122 ;
d = 56 ;
f = 3.56 ;

i = 2 ;
j = 3 ;
k = i + j ;

while(1) ;
} 

/************************************************/
```
   
A quoi sert la première ligne ?

#Q2
 Compiler le projet (build all) . Dans le fichier (Adress Linker Map) généré par la compilation, relever les adresses des différentes variables. Déterminer la taille en octet pour chaque type.

#Q3
Sélectionner le debugger intégré ( MPLAB SIM). Mettre un point d'arrêt à la première ligne du programme (a = 35;) . Ouvrir la fenêtre « file registers ». Executer le programme en pas à pas et vérifier le bon fonctionnement. Préciser l'ordre de stockage en mémoire pour chaque variable. Relever la zone mémoire entre 80 et 90.


 Le micro-contrôleur dispose d'une zone mémoire appelée « file registers » comprise en entre 000 et FFF en hexadécimal. Il dispose d'une unité arithmétique et logique et un accumulateur  noté w. 

Q4 : Modifier le source en ne laissant que les lignes concernant i, j et k. Compiler le programme. Mettre un point d'arrêt sur la première ligne et exécuter. Ouvrir la fenêtre mixte « disassembly listing » qui contient les lignes sources en C et le code machine correspondant généré sous forme de mnémoniques assembleur. 

Exécuter en pas à pas (dans la fenêtre mixte) et expliquer ce que fait chaque instruction machine. On ne tiendra pas compte de l'instruction MOVLB. (aide :http://technology.niagarac.on.ca/staff/mboldin/18F_Instruction_Set). 

Comment est codée en assembleur l'instruction C « while(1) ; ». A quoi sert cette instruction ?

Q5 : Ajouter la ligne « byte *pt » dans la zone de définition des variables globales (avant le main). Compiler et indiquer l'adresse de la variable pt et sa taille. Que va contenir pt ? Quelle est sa valeur actuelle ?

Q6 : Ajouter en première ligne « pt = &i; ». Compiler et exécuter cette première ligne. Quel est le contenu de pt ? Expliquer.

Q7 : Proposer deux solutions pour modifier le contenu de la variable i avec la valeur 7. Donner des noms à ces deux solutions. 

Le micro-contrôleur dispose d'octets de mémoire réservés pour dialoguer avec le mode extérieur appelés « ports d'entrée sortie ». Chaque port d'entrée sorties dispose d'une ou plusieurs adresses mémoire permettant de contrôler son fonctionnement. Dans la suite de ce TP on va utiliser un port d'entrée sortie 8 bits appelé le port B. Les deux variables utilisées seront :
	-  trisb définie à l'adresse 0xF93
	-  portb définie à l'adresse 0xF81


Q8 : Ouvrir avec Proteus VSM le fichier tp1-1.dsn. Modifier le fichier source sous MPLAB avec le code suivant :

/********************************************/
#define byte unsigned char

byte *trisb = 0xf93 ;
byte *portb = 0xf81 ;


void main(void)
{
*trisb = 0 ;
*portb = 0x55 ;
while(1) ;
} 
/********************************************/

Compiler et charger le fichier .hex généré dans le micro-contrôleur sous Proteus. Lancer, vérifier et expliquer le fonctionnement. A quoi sert la variable trisb

Q9 : Charger le fichier tp1-2.dsn sous Proteus, et t saisir le source suivant sous MPLAB

/********************************************/
#define byte unsigned char

byte *trisb = 0xf93 ;
byte *portb = 0xf81 ;


void affiche_7seg(byte val)
{
/* a compléter */
byte tab_7seg[]={0b00111111,0b00000110,0b01011011,0b01001111,.......};

…................................................


}

void main(void)
{
*trisb = 0 ;

/*test de la fonction */
affiche_7seg(2);

while(1) ;
} 
 
/********************************************/

Compléter le code de la fonction affiche_7seg qui permet d'afficher un chiffre de 0 à 9. Tester.



Q10 : Inclure le fichier fourni iq.h (#include « iq.h »)

Modifier le programme pour faire évoluer l'affichage de 0 à 9 avec un changement toutes les secondes. On utilisera la fonction delayms(int).



Q11 : Bonus : reprendre la question Q6 et exécuter en pas à pas en assembleur (fenêtre mixte). Expliquer le fonctionnement de l'adressage indirect en assembleur.
