# Voiture Autonome Virtuelle (VAV) : application de deep Q-Learning


Une application de voiture autonome créée à l'aide du package Kivy et d'un algorithme de deep Q-Learning.



<p align="center">
  <img width="735" height="315" src="https://user-images.githubusercontent.com/57750350/122225830-52474a80-cead-11eb-8b66-56e22aff0fe1.png?raw=true" alt="Sublime's custom image"/>
</p>

## Description de l'application
On a utilisé le package kivy sous Python pour créer un environnement de véhicule autonome.
On commence par une carte vide. Le véhicule est équipé de 3 capteurs (rouge, jaune et bleu). Au début, il circule de manière aléatoire dans la carte.


<p align="center">
  <img width="423" height="332" src="https://user-images.githubusercontent.com/57750350/122221712-756ffb00-cea9-11eb-8527-75a633e078a7.PNG?raw=true" alt="Sublime's custom image"/>
</p>


L'utilisateur peut dessiner du "sable" en utilisant la sourie. La mission du véhicule est de faire des allers-retours du coin supérieur gauche au coin inférieur droit de la carte sans aucune collision, en choisissant à chaque état l’action qui lui permet de maximiser sa récompense. Une collision a lieu quand la voiture touche le sable.


<p align="center">
  <img width="423" height="332" src="https://user-images.githubusercontent.com/57750350/122221894-a18b7c00-cea9-11eb-91a9-73b24e70bdda.png?raw=true" alt="Sublime's custom image"/>
</p>


Après chaque voyage qu'elle effectue, la voiture doit se rendre compte du nombre de pas qu'elle a mis pour arriver à sa destination. Il est préférable que ce nombre de pas soit inférieur à celui du voyage précédent.



## Kivy
Kivy est une bibliothèque de développement d'interface graphique multi-plateforme open source pour Python et peut fonctionner sur iOS, Android, Windows, OS X et GNU / Linux. Il aide à développer des applications qui utilisent une interface utilisateur multi-touch innovante. L'idée fondamentale derrière Kivy est de permettre au développeur de créer une application une fois et de l'utiliser sur tous les appareils, ce qui rend le code réutilisable, permettant une conception d'interaction rapide et facile et un prototypage rapide.


## Apprentissage par renforcement:
### État
Dans cette application, un état est défini par 5 variables :

<ul>
<li>Capteur1 (Rouge)</li>
<li>Capteur2 (Bleu)</li>
<li>Capteur3 (Jaune)</li>
<li>Orientation</li>
<li>-Orientation</li>
</ul>


Les trois premières variables correspondent aux capteurs du véhicule. Chacun d’entre eux détecte le nombre de pixels de sable dans un carré  de surface 400 pixels (20*20) dont le centre coïncide avec celui du capteur.

Les variables « orientation » et « -orientation » mesurent en degrés l’orientation de la voiture respectivement dans le sens direct et dans le sens indirect. L’orientation est nulle si la trajectoire de la voiture est linéaire. La variable « – orientation » est ajoutée pour améliorer la performance.




### Action

A chaque instant, le véhicule peut effectuer l’une des trois actions suivantes :

<ul>
<li>Tourner de 20° dans le sens des aiguilles d'une montre.</li>
<li>Tourner de 20° dans le sens direct.</li>
<li>Ne pas tourner</li>
</ul>

### Récompenses:

<ul>
<li>Se rapprocher de l’objectif : +0.5</li>
<li>S’éloigner de l’objectif : -0.5</li>
<li>Toucher le sable : -2</li>
<li>Trop se rapprocher des bords de la carte : -1</li>
<li>-Arriver à la destination : r=(nombre de pas)<sub>actuel</sub> - (nombre de pas)<sub>prec</sub></li>
</ul>


## Réseau de neurones

### Structure du réseau

<p align="center">
  <img width="444" height="332" src="https://user-images.githubusercontent.com/57750350/122224169-c254d100-ceab-11eb-982d-81dc532aa6c9.png?raw=true" alt="Sublime's custom image"/>
</p>


  Le réseau de neurone adopté pour l'apprentissage par renforcement profond cotient 3 couches:

   <ul>
<li> Une couche d’entrée (input layer) : 5 nœuds
     Dont chacun correspond à une variable d’état (Capteur1, Capteur2, Capteur3, Orientation et -Orientation)</li>
<li>Une couche cachée (hidden layer) : 30 nœuds
  Une couche cachée suffit pour des problèmes aussi simples. D'autres prendraient plus de temps à s'entraîner et n'entraîneraient pas d'améliorations significatives des
  performances. En effet, des études précédentes ont essayé plusieurs modèles de réseau de neurones à plusieurs couches cachées, mais ils n'ont pas amélioré la précision          de manière significative. Par conséquent, on a décidé d'utiliser une seule couche cachée car cela prend également moins de temps de calcul.</li>
<li>Une couche de sortie (output layer) : 3 nœuds
    Dont chacun correspond à une action</li>
</ul>

         
### Fonctions d'activation

Dans ce modèle d'apprentissage profond, deux fonctions d'activations sont appliquées:

<ul>
<li>La fonction ReLU est appliquée aux entrées du réseau, c'est à dire, aux variables d'état.</li>
<li>La fonction Softmax est appliquée au vecteur des sorties du réseau, c'est à dire, celui des Q valeurs estimées de l'état passée entrée et de chacune des actions.</li>
</ul>
         


