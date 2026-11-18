# TP JOUR 2 - RETAIL INTELLIGENCE PLATFORM

## Environnement
- Ubuntu 26.04.1
- Hadoop 3.5.0
- Java Hadoop 17
- Java NiFi 21
- HDFS mono-noeud
- Replication factor : 1

## Data Lake
Racine : /retail-lake

Zones créées :
- raw
- bronze
- silver
- gold
- quality
- logs

## Données TPCx-BB
Données initiales :
- store.dat
- item.dat
- store_sales.dat

Donnée refresh :
- store_sales.dat

Source : /mnt/hgfs/retail_project/

## Ingestion NiFi
Flux : GetFile -> PutHDFS
Gestion des échecs : LogAttribute

Source locale : /home/houda/nifi-input
Destination : /retail-lake/raw/nifi

Le fichier store.dat a été transféré par NiFi.
Il a ensuite été rangé dans la zone initial du Data Lake.

## Tests
- Création des répertoires HDFS
- Dépôt et lecture des données
- Vérification des tailles
- Contrôle SHA-256
- Test d'un dossier source inexistant dans NiFi
- Consultation de la provenance NiFi

## Limites
Environnement mono-noeud.
Les fichiers CSV de démonstration sont distincts des données TPCx-BB.
## Reproduction du TP

### 1. Démarrage de Hadoop HDFS

Démarrer les services HDFS :

    start-dfs.sh

Vérifier les processus :

    jps

Les processus attendus sont :
- NameNode
- DataNode
- SecondaryNameNode

### 2. Vérification du Data Lake

Afficher les répertoires :

    hdfs dfs -ls /retail-lake

Vérifier les fichiers TPCx-BB :

    hdfs dfs -ls -R /retail-lake/raw/tpcxbb

### 3. Démarrage d'Apache NiFi

Utiliser Java 21 :

    export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64

Démarrer NiFi :

    /opt/nifi/bin/nifi.sh start

Adresse de l'interface :

    https://localhost:8443/nifi/

### 4. Fonctionnement du flux NiFi

Processeurs utilisés :
- GetFile : récupère les fichiers dans le dossier local.
- PutHDFS : envoie les fichiers vers HDFS.
- LogAttribute : permet la consultation des attributs
  dans le traitement des échecs.

Dossier source :
    /home/houda/nifi-input

Destination HDFS :
    /retail-lake/raw/nifi

Vérification du dépôt :

    hdfs dfs -ls -h /retail-lake/raw/nifi

### 5. Gestion des erreurs

Un test a été réalisé avec un dossier source inexistant.
NiFi a signalé l'erreur de configuration.
Le chemin correct a ensuite été rétabli.

Voir le fichier test_erreur_nifi.txt.

### 6. Provenance et intégrité

La provenance du FlowFile store.dat a été vérifiée
dans NiFi.

Les contrôles SHA-256 ont permis de comparer
les fichiers locaux et les fichiers HDFS.

Voir :
- provenance_nifi.txt
- controle_integrite_donnees.txt
- preuve_integrite_nifi.txt

### 7. Arrêt des services

Arrêter NiFi :

    /opt/nifi/bin/nifi.sh stop

Arrêter Hadoop HDFS :

    stop-dfs.sh

Ne jamais reformater le NameNode pour redémarrer HDFS.
