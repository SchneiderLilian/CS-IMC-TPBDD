# Préconfiguration de l'environnement

## Environnement Python

L'environnement tourne dans un GitHub Codespace, donc un conteneur Docker préconfiguré. Le fichier `.devcontainer/Dockerfile` part d'une image Microsoft avec .NET 8.0 et rajoute tout ce qu'il faut pour le TP :

- Python 3 avec pip et venv
- Les drivers ODBC 18 pour se connecter à SQL Server (`msodbcsql18`)
- Les outils mssql en ligne de commande
- Les libs ODBC et Kerberos nécessaires

Un environnement virtuel Python est créé automatiquement dans `/home/vscode/venv` avec les packages du `requirements.txt` :
- `pyodbc` pour SQL Server
- `py2neo` pour Neo4j
- `python-dotenv` pour charger les variables d'environnement

Le fichier `devcontainer.json` configure VS Code pour utiliser automatiquement cet environnement virtuel et installe les extensions utiles (Python, Pylance, Neo4j, Docker, etc.). Du coup pas besoin de faire `source venv/bin/activate` manuellement à chaque fois.

## Connexions aux bases de données

Toutes les infos de connexion sont dans un fichier `.env` qu'on doit récupérer auprès de l'enseignant. Ce fichier n'est pas versionné sur Git pour des raisons de sécurité.

Il contient :
- Les credentials SQL Server (serveur, db, user, password, driver ODBC)
- Les credentials Neo4j Sandbox (URL bolt, user, password)

Dans le code Python, on charge ces variables avec `dotenv.load_dotenv()` puis on y accède via `os.environ["NOM_VARIABLE"]`. Ça permet de séparer la config sensible du code.
