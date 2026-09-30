# 🟡 TP4 — Introduction à Spring Boot

⏱️ Durée indicative : 4 h

## 1. Passer le projet sous Spring Boot

1. Sur [start.spring.io](https://start.spring.io), générez un projet : **Gradle - Groovy**, **Java**, la dernière version stable de Spring Boot proposée par défaut, Group `fr`, Artifact `library` (le package devient `fr.library`), **Java 25**, dépendance **Spring Web**. Revoir comment cela a été fait dans [automated-tests](https://github.com/corentinbeuchet/automated-tests).
2. Dans votre dépôt, remplacez `build.gradle` et `settings.gradle` par ceux du projet généré, et ajoutez `src/main/java/fr/library/LibraryApplication.java` et `src/main/resources/application.properties`.
3. Supprimez `Main.java`, qui n'est plus utile, et gardez vos classes et vos tests.
4. `./gradlew bootRun` doit démarrer l'application sur http://localhost:8080.

## 2. Code de départ

### 📄 BookService.java

``` java
@Service
public class BookService {

    private final Library library = new Library();

    // TODO 10 : Retourner la liste des livres
}
```

> Réutilisez votre classe `Library` (et ses tests) plutôt que de recréer une liste dans le service.

### 📄 BookController.java

``` java
@RestController
@RequestMapping("/books")
public class BookController {

    private final BookService bookService;

    public BookController(BookService bookService) {
        this.bookService = bookService;
    }

    // TODO 9 : Créer un endpoint GET /books
}
```

## Reste à faire

### Exigences fonctionnelles

-   Le service retourne la liste des livres triée par titre, par auteur ou par ISBN
-   Le service permet l'ajout d'un livre et la suppression d'un livre par son ISBN
-   Le controller expose :

| Requête | Réponse |
|---|---|
| `GET /books?sort=title` (ou `author`, `isbn`) | `200` + la liste triée |
| `POST /books` (livre en JSON) | `201`, ou `409` si l'ISBN existe déjà |
| `DELETE /books/{isbn}` | `204`, ou `404` si le livre n'existe pas |

### Contraintes techniques

-   Transformer les exceptions métier en codes HTTP avec `@RestControllerAdvice` + `@ExceptionHandler`
-   Renvoyer les erreurs au format `ProblemDetail` : `{ "status": 409, "detail": "Livre déjà présent : 978…" }` (le message de votre exception), avec `ProblemDetail.forStatusAndDetail(HttpStatus.CONFLICT, e.getMessage())`. C'est le contrat qu'attend le front Angular ([angular-pour-les-noobs](https://github.com/corentinbeuchet/angular-pour-les-noobs), TP 4) : il affiche le champ `detail`.
-   Tests d'intégration pour les 3 endpoints, cas d'erreur compris, avec `@WebMvcTest` + `MockMvc` :
    -   dépendance `testImplementation 'org.springframework.boot:spring-boot-starter-webmvc-test'`
    -   depuis Spring Boot 4, `@WebMvcTest` est dans le package `org.springframework.boot.webmvc.test.autoconfigure`
    -   dans ces tests, remplacez le vrai `BookService` par un mock avec `@MockitoBean` (package `org.springframework.test.context.bean.override.mockito`)
-   La CI doit continuer à passer (`./gradlew test`)

## ✅ Terminé quand…

- [ ] `./gradlew bootRun` démarre et `GET /books` répond
- [ ] Chaque ligne du tableau ci-dessus est couverte par un test
- [ ] Les tests unitaires des TP précédents passent toujours
