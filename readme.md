# 🏆 Projet FizzBuzz – Docker, Tests & CI/CD

Ce projet vise à exécuter un programme FizzBuzz en Python à l'aide de Docker et à automatiser les tests avec GitHub Actions et coverage.py.

## 📌 Objectifs du projet
- ✅ Conteneuriser le projet avec Docker
- ✅ Exécuter les tests unitaires avec unittest
- ✅ Vérifier la couverture du code avec coverage.py
- ✅ Automatiser l'exécution des tests avec GitHub Actions
- ✅ Stocker l'image Docker sur Docker Hub et l'utiliser dans les tests

## 🏗 1. Développement du programme FizzBuzz
Nous avons écrit un programme `fizzbuzz.py` qui suit la règle classique du jeu :
- Affiche "Fizz" pour les nombres multiples de 3
- Affiche "Buzz" pour les multiples de 5
- Affiche "FizzBuzz" pour les multiples de 3 et 5
- Affiche le nombre sinon

👉 Nous avons aussi ajouté un fichier `test_fizzbuzz.py` contenant des tests unitaires avec `unittest` pour vérifier son bon fonctionnement.

## 🐳 2. Création du Dockerfile
Nous avons ensuite écrit un Dockerfile pour conteneuriser notre application :

```dockerfile
FROM python:3.9-slim  # Utilisation d'une image légère de Python
WORKDIR /app  # Définition du répertoire de travail
COPY . /app  # Copie du code source dans le conteneur
RUN pip install coverage  # Installation de coverage.py pour les tests
CMD ["python", "fizzbuzz.py"]  # Exécution du programme
```

👉 Ce fichier permet de créer une image Docker qui peut exécuter FizzBuzz et tester la couverture du code.

## 🏗 3. Construction et exécution du conteneur Docker
Nous avons ensuite construit l’image avec la commande :

```bash
docker build -t fizzbuzz-app .
```

Puis, nous avons exécuté le conteneur pour vérifier le programme :

```bash
docker run fizzbuzz-app
```

👉 Le programme a affiché FizzBuzz correctement, donc l’image fonctionnait !

Ensuite, nous avons exécuté les tests unitaires à l’intérieur du conteneur :

```bash
docker run fizzbuzz-app coverage run -m unittest test_fizzbuzz.py
docker run fizzbuzz-app coverage report -m
```

👉 Coverage a bien fonctionné, montrant que tous les tests passaient !

## 🚀 4. Automatisation avec GitHub Actions
Nous avons mis en place un workflow GitHub Actions (`.github/workflows/main.yml`) pour :

- Se connecter à Docker Hub
- Télécharger l’image depuis Docker Hub
- Exécuter les tests et coverage automatiquement

```yaml
name: Run FizzBuzz with Docker

on: [push, pull_request]  # Exécute le workflow à chaque push ou pull request

jobs:
    test:
        runs-on: ubuntu-latest

        steps:
            - name: Checkout repository
                uses: actions/checkout@v3

            - name: Log in to Docker Hub
                run: echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u "${{ secrets.DOCKERHUB_USERNAME }}" --password-stdin

            - name: Pull Docker image from Docker Hub
                run: docker pull saad292/fizzbuzz-app

            - name: Run tests inside Docker
                run: docker run saad292/fizzbuzz-app coverage run -m unittest test_fizzbuzz.py

            - name: Generate coverage report
                run: docker run saad292/fizzbuzz-app coverage report -m
```

👉 Explication :
- ✔ Le workflow se déclenche à chaque push/pull request
- ✔ Il récupère l’image Docker depuis Docker Hub
- ✔ Il exécute les tests unitaires et génère un rapport de couverture

## 🐳 5. Publication de l’image Docker sur Docker Hub
Nous avons configuré Docker Hub pour stocker notre image en ligne. Ensuite, nous avons exécuté :

```bash
docker login -u saad292
docker tag fizzbuzz-app saad292/fizzbuzz-app
docker push saad292/fizzbuzz-app
```

👉 Cela permet à n'importe qui de récupérer notre image avec :

```bash
docker pull saad292/fizzbuzz-app
```

Et de l'exécuter directement sans reconstruire l’image ! 🎉

## 📌 Résumé des étapes réalisées
- ✅ Développement du programme FizzBuzz
- ✅ Ajout des tests unitaires avec unittest
- ✅ Création d'un Dockerfile pour conteneuriser l’application
- ✅ Construction et test du conteneur en local
- ✅ Automatisation des tests et de la couverture avec GitHub Actions
- ✅ Publication de l’image sur Docker Hub

## 🔥 Commandes utiles
1️⃣ Construire et exécuter en local :

```bash
docker build -t fizzbuzz-app .
docker run fizzbuzz-app
```

2️⃣ Lancer les tests et afficher le coverage :

```bash
docker run fizzbuzz-app coverage run -m unittest test_fizzbuzz.py
docker run fizzbuzz-app coverage report -m
```

3️⃣ Récupérer l’image depuis Docker Hub :

```bash
docker pull saad292/fizzbuzz-app
docker run saad292/fizzbuzz-app
```

## 🎯 Conclusion
Ce projet montre comment :
- ✔ Utiliser Docker pour exécuter un projet Python
- ✔ Intégrer les tests unitaires dans un conteneur
- ✔ Générer un rapport de couverture de tests
- ✔ Automatiser le processus avec GitHub Actions
- ✔ Partager l’image sur Docker Hub pour faciliter son utilisation

## 🎯 Prochaine amélioration possible
- Générer un rapport de couverture HTML
- Ajouter des badges dans le README

🚀 Projet terminé avec succès ! 🚀