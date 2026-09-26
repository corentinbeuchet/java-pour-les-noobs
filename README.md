# java-pour-les-noobs

Dans cette série de TP, vous allez construire pas à pas une petite application de gestion de bibliothèque en Java : d'abord en Java pur, puis avec des tests automatisés, et enfin avec Spring Boot et une base de données.

## Ce que vous devez comprendre et savoir faire

- Écrire des classes Java propres (encapsulation, `equals()` / `hashCode()`, `toString()`)
- Gérer les erreurs avec une hiérarchie d'exceptions
- Écrire des tests unitaires utiles avec JUnit 6 (cas nominaux, cas limites, tests paramétrés)
- Utiliser les Streams, les `record`, les `sealed interface` et le `switch` moderne
- Exposer une API REST avec Spring Boot et la tester
- Persister les données avec Spring Data JPA, Liquibase et Testcontainers

## Prérequis

| Outil | Version |
|---|---|
| Java (JDK) | 25 (LTS), par exemple [Eclipse Temurin](https://adoptium.net/) |
| Gradle | 9.8.0, fourni par le wrapper `./gradlew` (rien à installer) |
| Spring Boot | la dernière version stable proposée par défaut sur [start.spring.io](https://start.spring.io) (à partir du TP 4) |
| IDE | IntelliJ IDEA (ou celui de votre choix) |
| Docker | nécessaire au TP 5 pour Testcontainers |

## Démarrer

1. Créez un dépôt **vide** sur votre compte GitHub (sans README).
2. Récupérez ce projet et poussez-le sur votre dépôt :

```bash
git clone https://github.com/corentinbeuchet/java-pour-les-noobs.git
cd java-pour-les-noobs
git remote set-url origin https://github.com/<votre-compte>/java-pour-les-noobs.git
git push -u origin main
```

3. Vérifiez que tout fonctionne :

```bash
./gradlew run        # sous Windows : gradlew.bat run
```

Le programme s'arrête sur `UnsupportedOperationException: TODO 4 : addBook` : c'est normal, c'est à vous de jouer !

## Contenu

| TP | Sujet | Niveau | Durée indicative |
|---|---|---|---|
| [TP 1](TP1_Java_Fondamental.md) | Java fondamental | 🟢 | 2 h |
| [TP 2](TP2_Exceptions_Tests.md) | Exceptions & tests | 🟢 | 3 h |
| [TP 3](TP3_Java_Moderne.md) | Java moderne | 🟢 | 2 h |
| [TP 4](TP4_SpringBoot_Introduction.md) | Introduction à Spring Boot | 🟡 | 4 h |
| [TP 5](TP5_JPA_Database.md) | Base de données avec JPA | 🔴 | 4 h |

Chaque TP repart du code du TP précédent : travaillez dans **un seul dépôt** et ouvrez **une Pull Request par TP**.

## Structure du projet

```text
.
├── build.gradle               # configuration Gradle
├── gradlew / gradlew.bat      # wrapper Gradle (pas besoin d'installer Gradle)
├── src/main/java/fr/library/  # votre code
└── ressources-tp5/            # fichiers à copier au TP 5 (ne pas toucher avant)
```
