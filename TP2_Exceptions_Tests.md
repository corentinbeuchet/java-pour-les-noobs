# 🟢 TP2 — Exceptions & tests

⏱️ Durée indicative : 3 h

## 1. Ajouter JUnit 6

Dans `build.gradle`, remplacez le bloc `dependencies` et ajoutez la configuration de la tâche `test` :

``` groovy
dependencies {
    testImplementation platform('org.junit:junit-bom:6.1.3')
    testImplementation 'org.junit.jupiter:junit-jupiter'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

> Sans `useJUnitPlatform()`, Gradle ne trouve pas vos tests. Les tests se placent dans `src/test/java/fr/library/`.

## 2. Test fourni

Créez `src/test/java/fr/library/LibraryTest.java` :

``` java
package fr.library;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class LibraryTest {

    @Test
    void shouldFindBookByTitle() {
        Library library = new Library();
        library.addBook(new Book("9780132350884", "Clean Code", "Robert C. Martin", 2008));

        Book book = library.findBookByTitle("Clean Code");

        assertEquals("9780132350884", book.getIsbn());
    }
}
```

Lancez `./gradlew test` : le test doit passer.

## 3. Reste à faire

### Exigences fonctionnelles

-   Remplacer `List` par `Set` / `Map` lorsque c'est pertinent (les tests doivent rester verts : c'est tout l'intérêt des tests)
-   Implémenter une hiérarchie d'exceptions **non vérifiées** :
    -   `LibraryException extends RuntimeException`
    -   `BookNotFoundException extends LibraryException` : livre absent (recherche, suppression)
    -   `DuplicateBookException extends LibraryException` : ISBN déjà présent

### Contraintes techniques

-   JUnit 6 obligatoire
-   Des tests unitaires pertinents pour chaque méthode de `Library` et pour le constructeur de `Book`
-   Au moins un test paramétré : [documentation JUnit](https://docs.junit.org/current/writing-tests/parameterized-classes-and-tests.html)
-   Tests des cas limites indispensables : ISBN `null`, vide ou fait d'espaces, doublon, livre absent, bibliothèque vide…
-   Pas d'import `*` (`import static org.junit.jupiter.api.Assertions.*;`) : importez ce que vous utilisez
-   Ajouter la CI pour lancer les tests automatiquement : créez `.github/workflows/ci.yml` :

```yaml
name: CI

on:
  pull_request:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-26.04
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-java@v6
        with:
          distribution: temurin
          java-version: '25'
          cache: gradle
      - run: ./gradlew test
```

-   Bloquer le merge si la CI ne passe pas : ajoutez le check `test` (le nom du job) aux règles de protection de `main`

> ⚠️ Si la CI échoue avec `./gradlew: Permission denied` (fréquent quand le fichier a été commité depuis Windows), `gradlew` a perdu son droit d'exécution. Corrigez-le avec `git update-index --chmod=+x gradlew`, puis commitez.

## ✅ Terminé quand…

- [ ] `./gradlew test` passe en local **et** dans la CI
- [ ] Chaque exception de la hiérarchie est couverte par au moins un test
- [ ] Au moins un `@ParameterizedTest` teste plusieurs ISBN invalides
- [ ] Une Pull Request dont la CI est rouge ne peut pas être mergée
