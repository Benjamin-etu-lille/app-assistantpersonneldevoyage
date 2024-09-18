
# SAE S2.02 – Rapport pour la ressource Dev

*SERE Benjamin,  LEGRAND Alexandre,  POUPARD–RAMAUT Rémi*

**Version 1 :**

## Commande pour lancer Application.jar

```sh
java -jar Application.jar
```

## Architecture permettant l’éxecution du projet en dehors d’Application.jar

```
klonk@ordidefou:~/Documents/cours/F5$ tree
.
├── Application.jar
├── bin
│   ├── app
│   │   ├── Main.class
│   │   ├── Plateforme.class
│   │   └── Voyageur.class
│   └── graphes
│       ├── MonLieu.class
│       ├── Troncon.class
│       └── TypeCout.class
├── dev
│   ├── rapport_dev.md
│   └── UML
├── lib
│   ├── jgrapht-core-1.5.1.jar
│   ├── jheaps-0.14.jar
│   ├── junit-platform-console-standalone-1.10.2.jar
│   └── sae_s2_2024.jar
├── src
│   ├── app
│   │   ├── Main.java
│   │   ├── Plateforme.java
│   │   └── Voyageur.java
│   └── graphes
│       ├── MonLieu.java
│       ├── Troncon.java
│       └── TypeCout.java
└── test
    └── app
        └── TestPlateforme.java
```

## Diagramme UML

![image](./UML)

## Diagramme UML en mermaid.js

```mermaid
classDiagram
class Plateforme {
  - trancons : HashMap<Trancon, Double>
  - lieux : HashSet<Lieu>
  - graphePrix : MultiGrapheOrienteValue
  - grapheCO2 : MultiGrapheOrienteValue
  - grapheTemps : MultiGrapheOrienteValue
  - graphe : MultiGrapheOrienteValue
  - voyageur : Voyageur
  - m : ModaliteTransport
  - coutChoisi : TypeCout

  + Plateforme(String[] data)
  + choisirFiltre() void
  + ventilation(String[] data) ArrayList<String[]>
  + extraireLieu(ArrayList<String[]> data) HashSet<Lieu>
  + extraireSommet(HashSet<Lieu> lieu, MultiGrapheOrienteValue graphe) void
  + extraireTroncon(ArrayList<String[]> data, TypeCout cout) HashMap<Trancon, Double>
  + triData(ArrayList<String[]> data) void
  + getTypeCout(int i, ArrayList<String[]> data) Double
  + _getUserInput() String
  + extraireArete(HashMap<Trancon, Double> hashMapTrancon, MultiGrapheOrienteValue graphe) void
  + genererGraphe(ArrayList<String[]> data) void
  + clearMap() void
  + _isDouble(String s) boolean
  + critere() String[]
  + _comparerArrete(Chemin a1, Chemin a2) boolean
  + appliquerCritere(String[] critere) List<Chemin>
  + afficherCritere(TypeCout c) void
  + filtre() TypeCout
  + associerGraphe(TypeCout cout) MultiGrapheOrienteValue
  + choisirVille(String type, HashSet<Lieu> disponibles) Lieu
  + choisirTransport() ModaliteTransport
  + _estDansEnum(String str, Class<T> enumClass) boolean
  + _afficherEnum(Class<T> enumClass) void
  + afficherLieux(HashSet<Lieu> lieux) void
  + afficherTroncons(HashMap<Trancon, Double> trancons) void
  + getLieux() HashSet<Lieu>
  + getTrancons() HashMap<Trancon, Double>
  + getGrapheCO2() MultiGrapheOrienteValue
  + getGraphePrix() MultiGrapheOrienteValue
  + getGrapheTemps() MultiGrapheOrienteValue
  + getGraphe() MultiGrapheOrienteValue
  + getVoyageur() Voyageur
  + setM(ModaliteTransport m) void
  + getM() ModaliteTransport
  + _toString(List<Chemin> l) String
}

class Voyageur {
  - final ID: int
  - arrivee: Lieu
  - depart: Lieu
  - _cpt: int
  + Voyageur(Lieu arrivee, Lieu depart)
  + Voyageur()
  + getArrivee(): Lieu
  + getDepart(): Lieu
  + getID(): int
  + setArrivee(Lieu arrivee): void
  + setDepart(Lieu depart): void
}

class Troncon {
  - depart: Lieu
  - arrivee: Lieu
  - modalite: ModaliteTransport

  + Troncon(Lieu depart, Lieu arrivee, ModaliteTransport modalite)
  + Troncon(String sdepart, String sarrivee, ModaliteTransport modalite)
  + Troncon(String sdepart, String sarrivee, String modalite)
  + getArrivee(): Lieu
  + getDepart(): Lieu
  + getModalite(): ModaliteTransport
  + inversTrancon(): Trancon
  + toString(): String
}

class MonLieu {
  - nom: String
  + MonLieu(String nom)
  + getNom(): String
  + equals(Object obj): boolean
  + toString(): String
}

class Main {
  + _main(String[] args): void
  + _toString(List<Chemin> l): String
}

Plateforme --> "1" Voyageur
Plateforme --> "1" ModaliteTransport : m
Plateforme --> "1" TypeCout : coutChoisi
Main --> Plateforme
Plateforme --> Troncon
Plateforme --> MonLieu
Troncon --> MonLieu : depart
Troncon --> MonLieu : arrivee
Troncon --> ModaliteTransport : modalite

class ModaliteTransport {
  <<enumeration>>
  TRAIN
  BUS
  AVION
}

class TypeCout {
  <<enumeration>>
  CO2
  TEMPS
  PRIX
}
```

## Explication de l’architecture

Les programmes java sont séparés en deux packages :

- **graphe** :
  - contient l’enum `TypeCout` qui liste les différents critères
  - contient les classes `MonLieu` et `Troncon` qui implémentent respectivement les interfaces `Lieu` et `Trancon` qui sont fournies dans l’archive jar `sae_s2_2024.jar`.
  
- **app** :
  - contient les classes `Main`, `Plateforme` et `Voyageur`. Elles utilisent les classes présentes dans `graphes`. `Voyageur` sert à représenter l’utilisateur auprès de `Plateforme`. `Plateforme` créé les graphes en fonction des input et des données de l’utilisateur. `Main` se lance au démarrage de `Application.jar` et appelle successivement les fonctions permettant à l’utilisateur d’interagir avec la classe `Plateforme`.

## TestPlateforme.java

La classe de test `TestPlateforme.java` implémente 10 tests différents sur la classe `Plateforme`.

**Version 2 :**

## Commande pour lancer Application.jar

```bash
java -jar Application.jar
```

## Architecture permettant l’exécution du projet en dehors d’Application.jar

```bash
klonk@ordidefou:~/Documents/F5$ tree
.
├── Application.jar
├── dev
│   ├── rapport.pdf
│   ├── UML
│   └── UML_v2.png
├── graphes
│   ├── exempleGraphe.png
│   ├── exempleGrapheV2.png
│   └── rapport_graphes.md
├── ihm
│   └── IHM_merged.pdf
├── lib
│   ├── jgrapht-core-1.5.1.jar
│   ├── jheaps-0.14.jar
│   ├── junit-platform-console-standalone-1.10.2.jar
│   └── sae_s2_2024.jar
├── README.md
├── res
│   ├── correspondance.csv
│   └── data.csv
├── src
│   ├── app
│   │   ├── Correspondance.java
│   │   ├── Graphes.java
│   │   ├── Main.java
│   │   ├── Plateforme.java
│   │   └── Voyageur.java
│   ├── exception
│   │   ├── CSVFormatException.java
│   │   ├── RoadNotFoundException.java
│   │   └── SameCityException.java
│   └── graphes
│       ├── MonLieu.java
│       ├── Troncon.java
│       └── TypeCout.java
└── test
    └── app
        └── TestPlateforme.java

12 directories, 27 files
```

## Diagramme UML

![image](./UML_v2.png)

## Diagramme UML en mermaid.js

```mermaid
classDiagram
 class Correspondance {
 -correspondanceMinute HashMap<String[], Integer>
 -correspondanceCO2 HashMap<String[], Double>
 -correspondancePrix HashMap<String[], Integer>
 +Correspondance(String)
 +extraireLieuCorrespondance(Set<Lieu>)
 +getCorrespondance() HashSet<Lieu>
 }

 class Graphes {
 -trancons HashMap<Trancon, Double>
 -lieux HashSet<Lieu>
 +Graphes(String, String)
 +Graphes(String)
 +Graphes(String[])
 +ventilation(String) ArrayList<String[]>
 +ventilation(String[]) ArrayList<String[]>
 +areTranconsValid(List<String[]>) boolean
 +areCorrespondancesValid(List<String[]>) boolean
 +extraireLieu(ArrayList<String[]>) HashSet<Lieu>
 +extraireSommet(HashSet<Lieu>, MultiGrapheOrienteValue)
 +extraireTroncon(ArrayList<String[]>, TypeCout) HashMap<Trancon, Double>
 +extraireTroncon(ArrayList<String[]>, ArrayList<String[]>, TypeCout) HashMap<Trancon, Double>
 +triData(ArrayList<String[]>)
 +getTypeCout(int, ArrayList<String[]>) Double
 +extraireArete(HashMap<Trancon, Double>, MultiGrapheOrienteValue)
 +genererGraphe(ArrayList<String[]>)
 +genererGraphe(ArrayList<String[]>, ArrayList<String[]>)
 +clearMap()
 +associerGraphe(TypeCout) MultiGrapheOrienteValue
 +afficherLieux(HashSet<Lieu>)
 +getLieux() HashSet<Lieu>
 +getTroncons() HashMap<Trancon, Double>
 +getGrapheCO2() MultiGrapheOrienteValue
 +getGraphePrix() MultiGrapheOrienteValue
 +getGrapheTemps() MultiGrapheOrienteValue
 +getCoutChoisi() TypeCout
 +setM(ModaliteTransport)
 +setCoutChoisi(TypeCout)
 +getCorrespondance() Correspondance
 +getTrancons() HashMap<Trancon, Double>
 +getM() ModaliteTransport
 }

 Graphes --> MultiGrapheOrienteValue : graphePrix, grapheCO2, grapheTemps
 Graphes --> ModaliteTransport : m
 Graphes --> TypeCout : coutChoisi
 Graphes --> Correspondance : correspondance

 %%class Main {
 %% -SEPARATOR String
 %% -FILENAME String
 %% -FILENAMEC String
 %% +main(String[])
 %% +toString(List<Chemin>, TypeCout) String
 %%}

 class Voyageur {
 -ID int
 -cpt int
 +Voyageur(Lieu, Lieu)
 +Voyageur()
 +getArrivee() Lieu
 +getDepart() Lieu
 +getID() int
 +setArrivee(Lieu) void
 +setDepart(Lieu) void
 }

 Voyageur --> Lieu : depart, arrive


 class Plateforme{
 +Plateforme(Graphes)
 +choisirFiltre()
 +getUserInuput() String
 +isDouble(String) boolean
 +critere() String[]
 +comparerArete(Chemin, Chemin) boolean
 +comparerArete(List<Trancon>, List<Trancon>) boolean
 +appliquerCritere(String[]) List<Chemin>
 +calculKPCC(MultiGrapheOrienteValue, Voyageur) List<Chemin>
 +afficherCritere(TypeCout)
 +choisirVille(String, HashSet<Lieu>) Lieu
 +choisirTransport() ModaliteTransport
 +estDansTypeCout(String) boolean
 +estDansModaliteTransport(String) boolean
 +afficherTypeCout()
 +filtre() TypeCout
 +afficherTroncons(HashMap<Trancon, Double>)
 +getGrapheFinal() MultiGrapheOrienteValue
 +getVoyageur() Voyageur
 +setGraphe(MultiGrapheOrienteValue)
 +getGraphes() Graphes
 }

 Plateforme --> Voyageur : voyageur
 Plateforme --> MultiGrapheOrienteValue : grapheFinal
 Plateforme --> Graphes : graphes

 class TypeCout{
 <<enumeration>>
 CO2
 TEMPS
 PRIX
 +afficherEquivalent() String
 }

 class ModaliteTransport{
 <<enumeration>>
 }

 class MonLieu{
 -nom String
 +MonLieu(String)
 +getNom() String
 + equals(Object ) boolean
 + toString() String
 +cleanName() String 
 }

 MonLieu --> Lieu

 class Troncon{
 +Troncon(Lieu , Lieu , ModaliteTransport )
 +Troncon(String , String , ModaliteTransport )
 +Troncon(String , String , String )
 +getArrivee() Lieu
 +getDepart() Lieu
 +getModalite() ModaliteTransport
 +inversTrancon() Trancon
 +isSameCity() boolean
 +toString() String
 }

 Troncon --> Lieu : depart, arrivee
 Troncon --> ModaliteTransport : modalite
 Troncon --> Trancon
```

## Nouveau package

Ajout du package 'exception' avec les exceptions : 
  
  - `CSVFormatException.java` qui est renvoyée si une erreur est détectée dans le CSV.
  - `RoadNotFoundException.java` qui est renvoyée si aucune route n'est trouvée entre le lieu de départ et d'arrivée.
  - `SameCityException.java` qui est renvoyée si l'utilisateur entre la même ville pour le départ et pour l'arrivée.

## Correspondance.java

Pour gérer les correspondances lors d'un itinéraire et éviter de lister tout les arrêts qui se font lors d'un voyage, nous avons décidé de créer une classe `Correspondance.java` qui à elle seule importe le fichier CSV contenant les correspondances possible (par exemple passer du train à l'avion dans la villeC). Elle est ensuite utilisée par la classe [Graphes.java](#graphesjava)

## Graphes.java

La classe Graphe a été ajoutée afin de réduire la quantité de code présente dans la classe Plateforme. Désormais, la Plateforme possède un attribut graphe qui va contenir tout les chemins possible et un attribut grapheFinal qui correspond au graphe auquel on a appliqué le critère choisi par l'utilisateur.

## Quelques fonctions ajoutées

- ### Graphes.java 
  - Surcharge des méthodes ventilation, extraireTroncon et genererGraphe afin de conserver la rétrocompatibilité.
  - Ajout de `areTranconsValid(List) : boolean` qui vérifie la validité des tronçons présent dans le data.csv, celui-ci renvoie un booléen false si une erreur est détectée.
  - Ajout de `areCorrespondancesValid(List) : boolean` qui vérifie la validité des correspondances inscrites dans correspondances.csv, cette méthode renvoie également un booléen false si une erreur est détectée.
  - Modification de `afficherLieux(HashSet) : void`, après en cette méthode n'affiche plus que les lieux dit, c'est à dire qu'elle n'affichera pas "villeC, Avion" par exemple.

- ### Plateforme.java
  - Ajout de `calculKPCC(MultiGrapheOrienteValue, Voyageur) : List`, cette méthode calcule les plus courts chemins dans le graphe passé en paramètre avec le voyageur correspondant. Cette méthode permet de calculer au mieux un trajet qui commencerait par une ville avec une correspondance ou bien qui irait vers une ville avec une correspondance.
  - Modification de `appliquerCritere(String[]) : List`, étant donnée que nous avons fait une méthode pour calculer les KPCC alors nous avons modifié cette méthode afin d'y ajouter l'appel à `calculKPCC(MultiGrapheOrienteValue, Voyageur) : List`.

- ### TypeCout.java
  - Ajout de `afficherEquivalent() : String` qui renvoie une String plus représentative du cout choisi, elle renverra par exemple "min" si le temps est choisi.

- ### MonLieu.java
  - Ajout de `cleanName() : String` qui renvoie une String à partir d'un `MonLieu`, cette méthode va le convertir en une String avant de récuperer uniquement la première partie de celui-ci avant la 1ère vigule. Elle renvoie donc "villeC" si l'objet courant se nomme "villeC, AVION".

- ### Troncon.java
  - Ajout de `isSameCity() : boolean` qui regarde l'égalité entre l'arrivé et le départ, en sachant que ceux-ci sont comparés avec leurs noms qui sont passés par `cleanName() : String`. Ainsi, cette méthode renverra true si un tronçon a "villeC" en départ et "villeC, AVION" en arrivé.

## TestPlateforme.java

Enfin nous avons modifié notre classe de test afin de couvrir l'ajout des correspondances. Nous avons également rajouté un test qui couvre une situation possible d'un plus court chemin entre villeA et villeD.