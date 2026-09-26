# 🟢 TP1 — Java fondamental guidé

⏱️ Durée indicative : 2 h

## Point de départ

Le projet compile déjà. Les trois classes se trouvent dans `src/main/java/fr/library/` :

```text
src/main/java/fr/library/
├── Main.java      # programme de démonstration (ne pas modifier au début)
├── Book.java      # à compléter (TODO 1 à 3)
└── Library.java   # à compléter (TODO 4 à 6)
```

Lancez `./gradlew run` : le programme s'arrête sur le premier `TODO`. Remplacez chaque `throw new UnsupportedOperationException(...)` par votre code, jusqu'à ce que `Main` affiche les deux livres.

## Reste à faire

### Exigences fonctionnelles

-   Rajouter l'identifiant [ISBN](https://fr.wikipedia.org/wiki/International_Standard_Book_Number) (ISBN-13 : 13 chiffres, sans tirets). Le constructeur devient :
    ```java
    public Book(String isbn, String title, String author, int year)
    ```
    Mettez ensuite `Main` à jour avec les ISBN : `9780132350884` (Clean Code) et `9780134685991` (Effective Java).
-   Refuser un ISBN ou un titre `null` ou vide (`IllegalArgumentException`)
-   Ajouter et supprimer des livres
-   Rechercher par titre, par auteur et par ISBN
-   Garantir l'unicité de l'ISBN dans la bibliothèque
-   L'ISBN doit être immuable après création

La classe `Library` doit exposer au minimum ces méthodes (elles seront utilisées par les tests du TP 2) :

| Méthode | Rôle |
|---|---|
| `void addBook(Book book)` | ajoute un livre |
| `void removeBook(String isbn)` | supprime un livre |
| `Book findByIsbn(String isbn)` | recherche par ISBN |
| `Book findBookByTitle(String title)` | recherche par titre |
| `List<Book> findByAuthor(String author)` | tous les livres d'un auteur |
| `boolean containsIsbn(String isbn)` | l'ISBN est-il présent ? |
| `int size()` | nombre de livres |
| `void displayBooks()` | affiche tous les livres |

### Contraintes techniques

-   Encapsulation stricte (aucun champ public)
-   Implémentation correcte de `equals()` et `hashCode()` : sur quel(s) champ(s) ? Justifiez votre choix dans la Pull Request.
-   `toString()` pertinent
-   Protéger la branche `main` de votre dépôt GitHub : passage obligatoire par une Pull Request (pas besoin de review, c'est un TP individuel)

### Bonus

-   Vérifier la [clé de contrôle](https://fr.wikipedia.org/wiki/International_Standard_Book_Number#Cl%C3%A9_de_contr%C3%B4le) de l'ISBN-13

## ✅ Terminé quand…

- [ ] `./gradlew run` affiche les livres sans erreur
- [ ] Un ISBN déjà présent ne peut pas être ajouté une deuxième fois
- [ ] Aucun champ n'est `public`, et l'ISBN n'a pas de setter
- [ ] Votre travail est arrivé sur `main` par une Pull Request
