# 🟢 TP3 — Java moderne

⏱️ Durée indicative : 2 h

Java a beaucoup évolué depuis Java 8. Ce TP vous fait utiliser ce que l'on trouve dans le code d'aujourd'hui : Streams, `Optional`, `record`, `sealed interface` et `switch` avec *pattern matching*.

## 1. Streams

Remplacez la boucle de recherche par un Stream :

``` java
public Book findBookByTitle(String title) {
    return books.stream()
            .filter(book -> book.getTitle().equals(title))
            .findFirst()
            .orElseThrow(() -> new BookNotFoundException("Livre introuvable : " + title));
}
```

> `books` représente ici votre collection de livres : si vous utilisez une `Map`, partez de `books.values().stream()`.
> `findFirst()` renvoie un `Optional<Book>` : `orElseThrow(...)` conserve l'exception du TP 2.

-   Retourner la liste des livres triée par titre : `List<Book> booksSortedByTitle()`
-   Retourner la liste des livres triée par auteur, puis par titre : `List<Book> booksSortedByAuthor()`
-   Retourner la liste des livres triée par ISBN : `List<Book> booksSortedByIsbn()` (elle servira au TP 4)
-   Utiliser `sorted(...)` avec un [`Comparator`](https://www.baeldung.com/java-comparator-comparable) : `Comparator.comparing(Book::getAuthor).thenComparing(Book::getTitle)`
-   Terminer vos Streams par `.toList()` (liste non modifiable) plutôt que `collect(Collectors.toList())`

## 2. `record`, `sealed interface` et `switch`

Au lieu d'écrire une méthode par type de recherche, on veut **une seule** méthode `search`. Créez les critères de recherche (**un fichier par type**) :

``` java
public sealed interface SearchCriteria permits ByTitle, ByAuthor, ByYearRange {
}

public record ByTitle(String title) implements SearchCriteria { }
public record ByAuthor(String author) implements SearchCriteria { }
public record ByYearRange(int from, int to) implements SearchCriteria { }
```

Puis implémentez dans `Library` :

``` java
public List<Book> search(SearchCriteria criteria) {
    return books.stream()
            .filter(book -> switch (criteria) {
                case ByTitle(String title) -> // TODO
                case ByAuthor(String author) -> // TODO
                case ByYearRange(int from, int to) -> // TODO
            })
            .toList();
}
```

> Le `switch` n'a pas besoin de `default` : l'interface est `sealed`, le compilateur sait que tous les cas sont traités. Ajoutez un quatrième record à la clause `permits` sans le traiter dans le `switch` : que se passe-t-il ?

## Contraintes techniques

-   Tests unitaires pour les méthodes de tri (dont une bibliothèque vide et deux livres de même titre)
-   Un test par critère de recherche (un test paramétré avec `@MethodSource` est bienvenu)
-   `search(new ByTitle(...))` ne tient pas compte de la casse (`equalsIgnoreCase`)

## Bonus

-   `Optional<Book> findOptionalByIsbn(String isbn)` : quand préférer un `Optional` à une exception ? Répondez dans la Pull Request.
-   Regrouper les livres par auteur : `Map<String, List<Book>>` avec `Collectors.groupingBy`
-   Un [Collector personnalisé](https://www.baeldung.com/java-collectors)

## ✅ Terminé quand…

- [ ] Plus aucune boucle `for` dans `Library`
- [ ] `search(...)` gère les trois critères avec un `switch` sans `default`
- [ ] Tous les tests passent dans la CI
