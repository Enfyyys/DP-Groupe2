# Patrons de Structure :

## Adaptateur

### Présentation 
<p>L’Adaptateur est un patron de conception structurel qui permet de faire collaborer des objets ayant des interfaces normalement incompatibles. </p>

<p>L’adaptateur fait fonctionner ensemble des classes qui n'auraient pas pu fonctionner sans lui, à cause d'une incompatibilité d'interfaces. Il permet donc de se faire comprendre par son destinataire, il fait office d'intermédiaire. </p>

<p> Cela peut arriver aussi que l'adaptateur ait la fonction d'adapter dans les 2 sens afin que les 2 partie normalement incompatible puissent se comprendre</p>


### Avantages

<p> Tout d'abord il y a le <strong>principe de Responsabilitée</strong>. Cela indique que grâce au code de l'adaptateur on peut découpler l’interface ou le code de conversion des données (soit l'adaptateur), de la logique métier du programme.</p>

<p>il y a aussi le <strong>Principe ouvert / fermé</strong> permettant d'ajouter sans trop de modification de nouveaux type d'adaptateurs.</p>

### Inconvénient

<p>La compléxité du code augmente car cela nécéssite de créer un ensemble d'interface et de nouvelles classes. Parfois il vaut mieux changer le code pour s'adapter au service.</p>


### Présentation UML

<img src='./UML_Adaptateur.jpg'/>

### Implémentation en C#


<p> Pour la situation, on va imaginer qu'on utilise un service pour tracer des formes géométriques et calculer des aires. On a déjà de quoi faire pour tracer des rectangles et calculer leurs aires dans la classe Rectangle. Cependant le service qu'on utilise contient des méthodes qui sont assez similaire aux nôtre. Pour s'adapter et éviter de modifier la classe Rectangle on va faire un Adaptateur pour intégrer le service ayant la classe forme qui possède les méthodes à implémenter. 

Ci dessous l'UML de la situation donné (pour voir le code qui va avec il faut se reporter sur le fichier Adaptateur.cs) </p>

```C#

 public interface Forme {
    double calculerAire();
    void dessiner();
}

public interface Rectangle {
    double calculerSurface();
    void tracerRectangle();
}


public class Adaptateur implements Forme {
    private Rectangle rectangle;

    public Adaptateur(Rectangle rectangle) {
        this.rectangle = rectangle;
    }

    @Override
    public double calculerAire() {
        return rectangle.calculerSurface();
    }

    @Override
    public void dessiner() {
        rectangle.tracerRectangle();
    }
}
```


## Pont

### Présentation

Le design pattern Bridge est un modèle structurel qui permet de séparer l'abstraction d'une classe de son implémentation, de sorte que les deux puissent évoluer indépendamment. 

### Avantages

Cela permet d'avoir des applications multiplateforme

Avec le principe ouvert / fermé, cela rend facile l'implémentation de nouvelles abstractions et implémentations indépendamment les unes des autres. 

Il est possible de se concentrer sur la logique de haut niveau dans l’abstraction, et sur les détails de la plateforme dans l’implémentation.

### Inconvénient

Cela rajoute une couche supplémentaire d'abstraction et le rend plus difficile à comprendre. Il est souvent plus utile sur les plus gros projets car sur des plus petits ce n'est pas forcément nécéssaire.

Cela peu rajouter beaucoup de classes et d'implémentation dans le code et demande alors une gestion plus complexe.

Avec l'implémentation de classes, cela rend plus difficile la maintenance de celles-ci où il peut être nécessaire de mettre à jour plusieurs classes en même temps.

Pour les développeurs qui ne sont pas familiers avec le pattern Bridge, son utilisation peut nécessiter un apprentissage initial. Il peut être perçu comme moins intuitif que des approches plus simples, ce qui peut entraîner une courbe d'apprentissage.

### Présentation UML

<img src="./UML_Bridge.jpg"/>

### Implémentation en C#

Dans ce contexte, on veux faire des carrés et des cercles qu'on  souhaite afficher avec différentes couleurs. Plutôt que de lier étroitement les formes aux couleurs, elles sont conçus indépendemment afin de pouvoir rajouter facilement de nouvelles classes (formes où couleurs). Ainsi il sera facile d'ajouter au besoin de nouvelles fonctionnalités avec moins d'implémentation et de classes.
```C#
public interface IColoration
{
    void Colorier();
}

public class ColorationRouge : IColoration
{
    public void Colorier()
    {
        Console.WriteLine("Coloration en Rouge");
    }
}

public class ColorationBleu : IColoration
{
    public void Colorier()
    {
        Console.WriteLine("Coloration en Bleu");
    }
}

public abstract class Forme
{
    protected IColoration Coloration;

    protected Forme(IColoration coloration)
    {
        Coloration = coloration;
    }

    public abstract void Afficher();
}

public class Cercle : Forme
{
    public Cercle(IColoration coloration) : base(coloration)
    {
    }

    public override void Afficher()
    {
        Console.Write("Cercle ");
        Coloration.Colorier();
    }
}

public class Carre : Forme
{
    public Carre(IColoration coloration) : base(coloration)
    {
    }

    public override void Afficher()
    {
        Console.Write("Carré ");
        Coloration.Colorier();
    }
}


```

### Composite

### Présentation 

Composite est un patron de conception structurel qui permet d’agencer les objets dans des arborescences afin de pouvoir traiter celles-ci comme des objets individuels. Et permet de concevoir une structure arborescente, par exemple une représentation d'un dossier, ses sous-dossiers et leurs fichiers.

### Avantages

Il permet de traiter à la fois les objets individuels et les compositions d'objets de manière uniforme, ce qui facilite la manipulation des structures arborescentes.

Le design pattern Composite permet d'ajouter de nouveaux types d'objets sans changer le code client existant. Cela rend le système plus flexible et extensible.

 Les composants peuvent être réutilisés à différents niveaux de la hiérarchie, ce qui favorise la réutilisation du code.

 ### Inconvénient

 Dans certains cas, l'utilisation du design pattern Composite peut entraîner une perte de performance en raison de la manipulation d'une structure arborescente plutôt que d'une structure plate.

 La mise en œuvre du design pattern Composite peut introduire une certaine complexité, surtout si la hiérarchie est profonde et complexe.

  Une structure hiérarchique complexe peut rendre la compréhension du code plus difficile pour les développeurs qui n'ont pas été impliqués dans la conception initiale.

  ### Présention UML

<img src = "./UML_Composite.jpg" />

  ### Implémentation en C#

  
Considérons un scénario simple où on doit modéliser une entreprise avec des employés. Chaque employé peut être soit un individu unique, soit un groupe d'employés. On utilisera le design pattern Composite pour représenter cette structure hiérarchique

```C#

public interface IEmploye
{
    string Nom { get; }
    double Salaire { get; }
}

public class EmployeIndividuel : IEmploye
{
    public string Nom { get; }
    public double Salaire { get; }

    public EmployeIndividuel(string nom, double salaire)
    {
        Nom = nom;
        Salaire = salaire;
    }
}

public class GroupeEmployes : IEmploye
{
    private List<IEmploye> membres = new List<IEmploye>();

    public string Nom { get; }
    public double Salaire
    {
        get
        {
            double totalSalaire = 0;
            foreach (var membre in membres)
            {
                totalSalaire += membre.Salaire;
            }
            return totalSalaire;
        }
    }

    public GroupeEmployes(string nom)
    {
        Nom = nom;
    }

    public void AjouterMembre(IEmploye employe)
    {
        membres.Add(employe);
    }

    public void AfficherStructure(int niveau = 0)
    {
        Console.WriteLine(new string('-', niveau) + $"Groupe: {Nom}, Salaire total: {Salaire}");
        foreach (var membre in membres)
        {
            membre.AfficherStructure(niveau + 2);
        }
    }
}

```

### Décorateur



### Présentation 
Décorateur est un patron de conception structurel qui permet d’affecter dynamiquement de nouveaux comportements à des objets en les plaçant dans des emballeurs qui implémentent ces comportements.

### Avantages

Le décorateur permet d'ajouter de nouvelles fonctionnalités à un objet existant sans modifier sa structure de base. Cela rend le code plus flexible et plus facile à étendre.

Le décorateur favorise la composition d'objets plutôt que l'héritage de classes. Cela évite les problèmes liés à une hiérarchie de classes trop profonde et facilite la réutilisation du code.



### Inconvénient

L'utilisation du décorateur peut entraîner une augmentation de la complexité du code, car de multiples couches de décorateurs peuvent être empilées. Cela peut rendre la compréhension du code plus difficile.

Lorsque de nombreuses fonctionnalités sont ajoutées au moyen de décorateurs, cela peut conduire à la création d'un grand nombre de petites classes, ce qui peut être difficile à gérer.


### Présentation UML

<img src='./UML_Decorateur.jpg'/>

### Implémentation en C#

On développe un système de gestion de café, avec différentes options de boissons. On a une classe de base, *CafeSimple*, qui représente une boisson de base. Ensuite, vous avez des décorateurs qui ajoutent des extras à la boisson, tels que le lait, le sucre, ou encore la vanille.

```C#

public abstract class Cafe
{
    public abstract string Description { get; }
    public abstract double Cout();
}

public class CafeSimple : Cafe
{
    public override string Description => "Café simple";

    public override double Cout()
    {
        return 1.0; // Coût de base
    }
}

public abstract class DecorateurCafe : Cafe
{
    protected Cafe cafe;

    public DecorateurCafe(Cafe cafe)
    {
        this.cafe = cafe;
    }
}

public class Lait : DecorateurCafe
{
    public Lait(Cafe cafe) : base(cafe) { }

    public override string Description => cafe.Description + ", Lait";

    public override double Cout()
    {
        return cafe.Cout() + 0.5; // Coût du lait
    }
}

public class Sucre : DecorateurCafe
{
    public Sucre(Cafe cafe) : base(cafe) { }

    public override string Description => cafe.Description + ", Sucre";

    public override double Cout()
    {
        return cafe.Cout() + 0.2; // Coût du sucre
    }
}

```