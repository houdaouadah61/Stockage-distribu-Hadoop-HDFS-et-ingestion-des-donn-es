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
