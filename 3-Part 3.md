# Maintenant on passe à la troisième étape Kafka Consumer To MongoDB

- Double cliquer sur le process group Kafka Consumer to MongoDB

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/d232d9a5-33aa-466c-844d-af32d2659ea0)

- Ajoutez un process Consumekafka_2_0
- Double cliquer sur Consumekafka_2_0 :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/2db2a5b6-6481-45b4-a485-6cc66fdc94e3)

- Aller sur Scheduling ; ajouter 5 sec au Run Scheduler :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/59d8ee46-3d23-434b-ae4e-b7f84cd098b7)

- Aller sur Properties / Ajoutez ```kafka:9092``` au kafka Brokers :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/bc39e139-a35b-4815-9258-a101724531f1)

- Ajoutez ```IBM,AAPL``` au Topic Name :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/e1a9c534-3c5a-4d0b-aa30-dbcb6624e598)

- Ajoutez ```MONGO``` au group id :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/bd66b6ae-3025-4cad-b7a9-f4b4810f0c91)

-Cliquer sur Apply : 

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/b02d153f-18f9-48aa-b5a3-1b7cdb2bcb8a)

- Ajouter un autre process ExtractText :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/fbd506b8-a241-4f36-b731-d506c88b0173)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/34b36af2-7eb5-4f47-86f5-105730b90623)

- Double Cliquer :
- Aller sur Scheduling ; ajouter 5 sec au Run Scheduler :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/59d8ee46-3d23-434b-ae4e-b7f84cd098b7)

- Aller sur Properties / Cliquer sur Add property :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/40b29125-d46c-4af5-a2b4-c64e7b329270)

- Entrez csv comme nom avec la valeur ```[^\n]+\n(.*),(.*),(.*),(.*),(.*),(.*)```

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/e1e7a8ad-58ed-461b-b767-9907ef7893ff)

- Cliquer sur Apply 

- Ajouter une connexion entre Consumekafka_2_0 et ExtractText :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/5bd1cf02-303d-4cf5-854e-65768b13b612)

- Ajouter une Output avec la relation unmatched :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/6ff27668-7dbe-4411-8de3-2128368882d6)

- Ajoutez un nouveau process ReplaceText

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/1c0c8e3f-1733-4e06-85e5-70ef252d65b9)

- Double Cliquer
- Aller sur Scheduling ; ajouter 5 sec au Run Scheduler :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/59d8ee46-3d23-434b-ae4e-b7f84cd098b7)

- Aller sur Properties / Modifiez Replacement Value par ```{"timestamp":"${csv.1}","open":"${csv.2}","high":"${csv.3}","low":"${csv.4}","close":"${csv.5}","volume":"${csv.6}"}```

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/41041381-63db-4063-81b0-e6d2fbe733a5)

- Cliquer sur Apply
- Ajouter une connexion entre ExtractText et ReplaceText de type matched :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/39963cbc-09b4-4828-9c63-a1d0820c5f72)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/cabe6e4f-0dc9-4715-8e0b-af5118d0a29d)


- Ajouter un autre process LogMessage :

- Aller sur Scheduling ; ajouter 5 sec au Run Scheduler :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/59d8ee46-3d23-434b-ae4e-b7f84cd098b7)

-  Aller sur Relationships > cochez terminate > APPLY :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/326e5508-efef-4e76-8cc2-46c203d6bc20)

- Ajouter une connexion de type failure entre ReplaceText et LogMessage :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/24607e7f-a3ee-4e83-b960-e219b39ba91b)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/478f7e3d-80ec-4b29-a9a7-545b2459f0ac)

# Se connecter à MongoDB :

- Se connecter sur votre compte [Mongo DB](https://account.mongodb.com/account/login?_ga=2.84207577.1748310431.1685041583-955503991.1678959535)

- Aller sur Database Deployment , et cliquer sur connect :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/fca6b837-2b53-4613-b533-13bdbecd66bd)

- Cliquer sur Divers :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/e45c1073-7cd7-4adc-a13b-90af0364904c)

- Choisir la version 4.3 or later de java et copier votre connection String :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/f56e3663-b410-4e2c-9f0a-001bcc5fe039)

- Créer une nouvelle Database et Collection :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/f0090e41-146a-42ff-a133-af268b3d5015)

- Ajoutez un autre process PutMongo

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/883d24f0-19dc-490b-9978-b4195ad8c802)

- Double cliquez sur PutMongo
- Aller sur Scheduling ; ajouter 5 sec au Run Scheduler :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/59d8ee46-3d23-434b-ae4e-b7f84cd098b7)

- Aller sur Properties / Copier et coller votre Mongo URI en modifiant <password> par votre mot de passe d'utilisateur Mongo

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/366b18ec-c54c-4606-a8d9-2c3be6238e5c)

- Ajoutez le nom ainsi que la collection de votre MongoDB :
  
![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/2ca86218-1d01-4830-ac38-bbfaf18ed6ee)

- Cliquer sur Apply :
  
- Ajouter une connection entre ReplaceText et PutMongo :
  
![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/4b1e9fb6-bf7e-4177-b316-e2f908d0075f)

- Copier et coller le process Log Message et ajouter une output SUCCESS_MONGO :
 
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/13e5dccf-20b1-4d16-b170-e1b550a3ca30)

- Exécutez votre Workflow

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/bef83b7e-7760-4b85-9fe9-ea396c5c3f74)

  
- Allez à votre MongoDB et rafraîchissez

- Vous devez visualiser vos données sur MongoDB 

 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/3b396c03-55ff-4c35-9985-7a7f82aa9488)

