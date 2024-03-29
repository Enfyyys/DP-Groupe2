## Mediateur

### Présentation

Le mediateur permet de supprimer les communications directes entre classes et de mettre en place un intermédiaire qui se chargera d'effectuer ces communications.

### Avantages

Permet de découpler le code et de diminuer les responsabilités de chaque classe.
Respecte mieux certains principes SOLID (notamment le SRP et le open/closed principle) : responsabilité unique des classes, et les classes sont fermées à la modification mais ouvertes à l'extension.

### Inconvénients

Etant donné que l'on injecte un "object" dans la méthode Notify(), on peut rapidement se retrouver avec un "God Object", qui est un objet qui peut "tout faire". Il faut donc utiliser ce pattern avec parcimonie. 

### Présentation UML

![Design PAttern sauvegarde-Page-4 drawio](https://github.com/Enfyyys/DP-Groupe1/assets/105907677/88d1e208-7f00-433d-868c-111781ddf0d0)

### Implémentation en C#

```C#
// Ici le médiateur sera le serveur d'un bar : les "components" seront le client et le cuisinier

var cuisinier = new Cuisinier();
var client = new Client();
var serveur = new Serveur(cuisinier, client);

client.SetMediator(serveur);
cuisinier.SetMediator(serveur);

client.Commande();

public interface IMediator
{
    void Notify(object sender, string ev);
}

class Serveur : IMediator
{
    private Cuisinier _cuisinier;

    private Client _client;

    public Serveur(Cuisinier cuisinier, Client client)
    {
        this._cuisinier = cuisinier;
        this._cuisinier.SetMediator(this);
        this._client = client;
        this._client.SetMediator(this);
    }

    public void Notify(object sender, string ev)
    {
        if (ev == "Commande")
        {
            Console.WriteLine("Le serveur a reçu la commande, il l'emmene en cuisine");
            this._cuisinier.Cuisine();
        }

        if (ev == "Renvoie")
        {
            Console.WriteLine("Le serveur amène la commande au client");
            this._client.Mange();
        }
    }
}

class Personne
{
    protected IMediator _mediator;

    public Personne(IMediator mediator = null)
    {
        this._mediator = mediator;
    }

    public void SetMediator(IMediator mediator)
    {
        this._mediator = mediator;
    }
}

class Cuisinier : Personne
{
    public void Cuisine()
    {
        Console.WriteLine("Le cuisinier cuisine et renvoie la commande");
        Thread.Sleep(2000);
        Console.WriteLine("Ding, c'est prêt");

        this._mediator.Notify(this, "Renvoie");
    }
}

class Client : Personne
{
    public void Commande()
    {
        Console.WriteLine("J'AI FAIM !!!");

        this._mediator.Notify(this, "Commande");
    }

    public void Mange()
    {
        Console.WriteLine("Miam");
    }
}
```
