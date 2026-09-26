# 🔴 TP5 — Base de données avec JPA

⏱️ Durée indicative : 4 h

## 1. Dépendances

Complétez le bloc `dependencies` de `build.gradle` (JPA, Liquibase, PostgreSQL, Testcontainers) :

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-webmvc'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-liquibase'
    runtimeOnly 'org.postgresql:postgresql'

    testImplementation 'org.springframework.boot:spring-boot-starter-webmvc-test'
    testImplementation 'org.springframework.boot:spring-boot-starter-data-jpa-test'
    testImplementation 'org.springframework.boot:spring-boot-testcontainers'
    testImplementation 'org.testcontainers:testcontainers-junit-jupiter'
    testImplementation 'org.testcontainers:testcontainers-postgresql'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}
```

> Spring Boot 4 : `spring-boot-starter-web` s'appelle désormais `spring-boot-starter-webmvc`, Liquibase a son propre starter et les starters de test sont découpés par technologie. Les versions sont gérées par Spring Boot : ne les écrivez pas.

## 2. Configuration et migrations

-   Copiez le contenu de [`ressources-tp5/src`](ressources-tp5/src) dans votre dossier `src` : configuration (`application.yaml`, `application-dev.yml`) et migrations Liquibase (création de la table `books` et dix livres d'exemple).
-   Supprimez `src/main/resources/application.properties` généré par start.spring.io : la configuration est désormais dans `application.yaml`.
-   Pour lancer l'application en local, démarrez une base PostgreSQL :

```bash
docker run -d --name library-db -e POSTGRES_DB=library_db -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres:18
```

-   N'hésitez pas à regarder le projet [test-driven-development](https://github.com/corentinbeuchet/test-driven-development) et à vous inspirer de son code.

## 3. Code de départ

### 📄 Book.java

``` java
@Entity
@Table(name = "books")
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true, updatable = false)
    private String isbn;
    private String title;
    private String author;
    private int year;

    protected Book() {
        // requis par JPA
    }
}
```

> `@Table(name = "books")` est indispensable : la migration crée la table `books`, et `ddl-auto: validate` refuse de démarrer si l'entité pointe vers une table `book`.

### 📄 BookRepository.java

``` java
public interface BookRepository extends JpaRepository<Book, Long> {

    Optional<Book> findByIsbn(String isbn);
}
```

### 📄 BookService.java

``` java
// TODO 11 : Utiliser BookRepository au lieu de la classe Library en mémoire
```

## Reste à faire

### Exigences fonctionnelles

-   Le projet fonctionne avec une base de données PostgreSQL
-   Les endpoints du TP 4 se comportent exactement comme avant (mêmes codes HTTP)

### Contraintes techniques

-   Utiliser Spring Data JPA pour accéder aux données
-   Tests d'intégration sur une vraie base grâce à Testcontainers (`@ServiceConnection`, image `postgres:18`), comme dans le projet [test-driven-development](https://github.com/corentinbeuchet/test-driven-development)
-   Ne jamais committer de vrai mot de passe : le mot de passe de la base est lu dans la variable d'environnement `DB_PASSWORD`

## ✅ Terminé quand…

- [ ] `./gradlew bootRun` démarre avec PostgreSQL et `GET /books` renvoie les dix livres d'exemple
- [ ] Les tests d'intégration tournent sur PostgreSQL via Testcontainers, en local et dans la CI
- [ ] Les tests des TP 2 à 4 passent toujours
