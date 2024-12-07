# medheadDatabase

Les étapes d'installation de la base de donnée: <br>

<ol>
  <li> Télécharger et installer Docker https://www.docker.com </li>
 <li> Récupérer l'image docker de Microsoft SQL Server en utilisant
   <code>docker pull mcr.microsoft.com/mssql/server:2022-latest</code> </li>
<li> Créer un réseau docker qui va contenir les conteneurs de la base de données, la API et le front end en utilisant <code>docker network create —driver bridge --subnet 172.16.0.0/16 <nom_du_reseau></code> </li>
<li> Cloner ce dépôt git qui contient le script SQL des tables de et donées initiales de l'application <code>git pull https://github.com/mhamadalkak22/medheadDatabase.git</code> </li>
<li> Dans le répertoire git récupéré, générer le conteneur docker à partir de l'image sql server. La commande requiert d'indiquer un mot de passe pour l'instance de SQL SERVER. Préciser le réseau, le numéro de port, le nom du conteneur et utiliser le répetoire courant comme volume partagé avec le conteneur sur le chemin /scripts. Il suffit de remplacer les valeurs dans la commande qui suit  
<code>docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=VOTRE_MOT_DE_PASSE" -p 1433:1433 --network NOM_DU_RESEAU --name sql_server --ip 172.16.0.2 -v %cd%:/scripts -d mcr.microsoft.com/mssql/server:2022-latest </code></li>
<li> Accéder à une instance de sqlcmd dans le conteneur docker en utilisant <code>docker exec -it NOM_DU_CONTENEUR /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P MOT_DE_PASSE_SQL</code>  </li>
<li> Dans l'invite de commande qui se lance créer la base de données avec <code>Create database MEDHEAD</code> puis valider  l'aide de  <code>Go</code> </li>
<li> Exécuter le script qui va créer les tables et les remplir avec les données <code>docker exec -it NOM_DU_CONTENEUR /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P MOT_DE_PASSE_SQL -i /scripts/data.sql </code> </li>
<li> Récupérer l'addresse IP du conteneur crée dans le réseau avec <code>Docker inspect NOM_DU_RESEAU </code> </li>
