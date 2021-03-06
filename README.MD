# opt-logging
La librairie contient les 2 fichiers de configuration de logback préconisé pour les développements d'application à l'OPT.
Toutes les logs sont dans le même fichier .log (${LOG_FILE}) à l'exception des logs métiers qui se trouvent dans un seul fichier .json (${LOG_FILE_JSON}) si le besoin est exprimé.

## Import de la dépendance

### Gradle : build.gradle
Ajouter la dépendance suivante dans votre build.gradle :

```gradle
compile group: 'nc.opt.core', name: 'opt-logging', version: '1.3.0'
```

### Maven : pom.xml
```gradle
<dependency>
	<groupId>nc.opt.core</groupId>
	<artifactId>opt-logging</artifactId>
	<version>1.3.0</version>
</dependency> 
```

## Configuration du logback

### Inclusion de la configuration par défaut
Contenu du fichier 'src/main/resources/logback-spring.xml'
```logback
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true">
	<property name="LOG_FILE" value="${LOG_FILE}"/>
	<property name="MAX_HISTORY" value="${MAX_HISTORY}"/>
	<property name="MAX_FILE_SIZE" value="${MAX_FILE_SIZE}"/>
	<property name="TOTAL_SIZE_CAP" value="${TOTAL_SIZE_CAP}"/>
	<include resource="nc/opt/core/logging/logs-defaults.xml" />
</configuration>
```

### Inclusion de la configuration pour les logs métiers
```logback
<property name="LOG_FILE_JSON" value="${LOG_FILE_JSON}"/>
<include resource="nc/opt/core/logging/logs-metiers.xml" />
```

## Fonctionnalités

## Logs classiques
Dans la configuration par défaut en version 1.3.0 :
<ul>
<li>La rotation de logs est configurée dans les fichier de la librairie. on utilise [SizeAndTimeBasedRollingPolicy](https://logback.qos.ch/manual/appenders.html).</li>
<li>Avec le profil 'dev' les logs s'affichent uniquement dans la console, les variables d'environnement sont donc facultatives</li>
<li>Avec un profil différent de 'dev' les logs s'affichent uniquement dans les fichiers et il faut définir les variables d'environnement</li>
<li>Le rootLogger est par défaut à INFO</li>
</ul>

## Logs métiers
Dans la configuration des logs métiers en version 1.3.0 :
<ul>
<li>La rotation de logs est configurée dans les fichier de la librairie. on utilise [SizeAndTimeBasedRollingPolicy](https://logback.qos.ch/manual/appenders.html).</li>
<li>Quelque soit le profil les logs sont générés dans le fichier</li>
<li>Le Logger métier est par défaut à INFO</li>
</ul>

## Variables d'environnement
4 variables d'environnement sont nécessaires avec un profil non dev : 
<ul>
<li>LOG_FILE : contient le path vers le fichier des logs classiques.
Les logs classiques sont les fichiers de logs java de base.
la variable doit se terminer par .log</li>
<li>MAX_HISTORY : Nombre de fichiers max à garder pour historique. A savoir que la rotation est effectuée par jour et par taille.</li>
<li>MAX_FILE_SIZE : Taille max d'un fichier pour la rotation. ex : 100MB.</li>
<li>TOTAL_SIZE_CAP : Taille max de la somme des fichiers présents. ex : 5GB</li>
</ul>

1 variable supplémentaire est nécessaire si on veut utiliser le service de log métier :
<ul>
<li>LOG_FILE_JSON : contient le path vers le fichier des logs métiers
Les logs métiers servent à logguer des objets directement en json.
la variable doit se terminer par .json</li>
</ul>

## How to log ?

### Utilisation des logs classiques

#### Déclaration
L'API de log est org.slf4j.slf4j-api
On déclare un logger par classe
Un logger est déclaré de la manière suivante (et non de manière statique : https://wiki.apache.org/commons/Logging/StaticLog):

```java
private final Logger log = LoggerFactory.getLogger(MaClasse.class);
```

#### Format des logs produites
le fichier est un fichier .log avec une ligne par log de la forme suivante :
```
2017-09-19 10:01:27,213 [http-nio-5001-exec-9] [INFO ] nc.opt.kafka.otrs.controller.ProducerOtrsController - Publication d'un message KAFKA depuis OTRS
2017-09-19 10:01:27,273 [StreamThread-1] [INFO ] nc.opt.kafka.wfm.service.impl.WfmServiceImpl - TypeTCRM=THDF | Produit=null | Queue=Telecom::CPMC::IC::RLB | TypeId=10
```
Le fichiers peut contenir des logs multi lignes en cas de stack trace java

### Utilisation du service de log métier

#### Déclaration
```java
@Autowired
private LogMetierService logMetierService;
```

#### Utilisation
```java
logMetierService.logObject("pays", new Pays());
```

#### Format des logs produites
le fichier est un fichier .json avec une ligne par log de la forme suivante :
```
{"@timestamp":"2017-09-01T12:47:15.503+11:00","@version":1,"pays":{\"id\":1,\"libelle\":\"France\"},"logger_name":"jsonLogger","thread_name":"http-nio-8084-exec-1","level":"INFO","level_value":20000}
{"@timestamp":"2017-09-01T12:47:15.503+11:00","@version":1,"commune":{\"id\":1,\"libelle\":\"Nouméa\", \"province\":\"SUD\", \"cp\":\"98800\"},"logger_name":"jsonLogger","thread_name":"http-nio-8084-exec-1","level":"INFO","level_value":20000}
```

