Ce TP consiste à déployer une infrastructure web hautement disponible avec Docker Compose, en équilibrant la charge entre les services Apache, 
MySQL et Neo4j répartis sur deux serveurs virtuels, avec une gestion de la persistance des données.

Services Mis en Place:

        Le TP met en œuvre trois services principaux, chacun déployé dans des conteneurs Docker distincts :
        Apache : Serveur web pour gérer les requêtes HTTP.
        Deux instances : apache1 et apache2, exposant respectivement les ports 8081 et 8082.
        MySQL : Base de données relationnelle pour la gestion des données applicatives.
        Deux instances : mysql1 (port 3306) et mysql2 (port 3307).
        Neo4j : Base de données orientée graphe pour des cas d'usage spécifiques.
        Deux instances : neo4j1 (ports 7474/7687) et neo4j2 (ports 7475/7688).
        Chaque service est configuré avec des volumes persistants pour stocker les données sur le disque local, garantissant leur persistance entre les redémarrages.

Haute Disponibilité (HA)

      La haute disponibilité est assurée grâce à :
      Réplication des Services :
      Chaque service est déployé en double (apache1/apache2, mysql1/mysql2, neo4j1/neo4j2) pour garantir une redondance.
      Si une instance tombe en panne, l'autre peut continuer à répondre aux requêtes.
      Isolation Réseau :
      Les services sont connectés à des réseaux Docker personnalisés (apache_network, backend_network, ha_network) pour contrôler les communications
      et limiter les interférences.


Load Balancing:

    Le load balancing est implémenté avec Nginx, configuré comme équilibreur de charge pour répartir les requêtes entre 
    les différentes instances de chaque service.
    
        Fonctionnement :
        Les requêtes sont dirigées vers un groupe de serveurs défini dans la directive upstream.
        Nginx utilise un algorithme d'équilibrage (par défaut, round-robin) pour distribuer les requêtes de manière uniforme.
        
        Configuration :
        Le fichier Nginx (nginx.conf) contient des directives pour gérer le load balancing :
        Apache : Répartition entre apache1:8081 et apache2:8082.
        MySQL : Répartition entre mysql1:3306 et mysql2:3307.
        Neo4j :
        Interface HTTP : Répartition entre neo4j1:7474 et neo4j2:7475.
        Protocole Bolt : Répartition entre neo4j1:7687 et neo4j2:7688.



















      
