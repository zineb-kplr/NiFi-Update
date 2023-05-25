# Nous allons fractionner le traitement en trois lots :

  1-Récupération des données Alpha Vantage.
  
  2-Découpage des données et envoi vers un broker Kafka.
  
  3-Récupération des données et stockage dans une base de données MongoDB.
  
# Réalisation UI 1ère Etape:	

- Une fois connecté à votre interface vous devez créer 3 différents process group

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/e3859632-e8c9-4fec-85ed-c2242d6a312f)

- Insérez l'icône du Process Group et faites-la glisser pour créer un nouveau Process Group:
- Insérez l'icône du Process Group et faites-la glisser pour créer un nouveau Process Group:

![process_group](https://user-images.githubusercontent.com/78825764/193155523-9dc14871-1799-4aaf-8bef-cc0463eaefb7.PNG)

- Donner pour chaque Process Group un nom (1-AlphaVantage Download ; 2-push to kafka ; 3-kafka Consumer to MongoDb)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/7df288df-d4e2-4ab5-91fe-39fd49e91b5a)

- Dans cette première partie on va créer notre premier Workflow au niveau du premier process group 1-AlphaVantage Download
- Double Cliquer sur Le Process Group 1-AlphaVantage Download 

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/36bb7ebe-724f-4fc2-b36a-ac2d419d2277)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/e1076952-b193-4396-b0bc-bf7f369c7594)

- Ensuite on va glisser l'icone de processor 

![b](https://user-images.githubusercontent.com/78825764/193159262-452364ad-5ba5-4667-a881-cf14bee04724.png)

- et on va créer le premier prossecor nommé GetHTTP :
- Cliquer sur ADD :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/af973344-d4f3-42fc-be16-7ebf74f382e5)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/72fe0cfc-640d-451c-818b-399869b4ebe3)

- Double Cliquer sur le Processor GetHTTP ; puis renommer votre Processor GetHTTP : IBM Stock :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/2807fa0a-c770-4d70-9d4c-8e39a5ecac70)

- Sur Scheduling ajouter 5 sec au Run Schedule :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/cef99f0e-0ec3-4d3f-aca6-ce30b5728eb7)

- Aller sur Properties :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/9a285395-023b-4762-9d5b-39a0ea92fb7a)

- Pour l'URL ; il nous faut une API afin de pouvoir accéder aux données d'AlphaVantage 

- Aller sur https://www.alphavantage.co/support/#api-key ; esssayer de remplir ces champs puis cliquer sur GET FREE API KEY :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/526d3d73-aa74-4691-a0ff-3973633fbdaa)

-  Copier l'API 
  
- Maintenant que you avez votre API on va entrer l'URL https://www.alphavantage.co/query?function=TIME_SERIES_INTRADAY&symbol=IBM&interval=1min&apikey=VOTREAPI&datatype=csv en modifiant VOTREAPI par l'API que vous avez copier 
  
- Ensuite entrez intraday_5min_IBM.csv dans le champ filename :
  
![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/a818fadc-6f0c-48b1-94e3-25d8beefdd17)

- Ensuite appuyez sur SSL Context Service > Create new service > Compatible Controller Services >StandardSSLContextService 1.21.0 > create
  
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/089303ca-4256-4dcd-9b90-c3afb5ca35c1)
   
 - Cliquer sur la flèche :
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/8cff6d1d-d3ac-429f-9344-a131cb1ac279)
   
 - Cliquez sur l'icone de paramètre :
   
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/f93f7054-e4a7-48a8-8110-f68591364198)
    
 - Aller sur Properties :
    
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/27f2ff4c-4d17-4793-a77b-005f71a09199)
    
 - Modifiez le paramètre Truststore type no value > JKS
 - Modifier le paramètre Truststore Filename : ```/opt/nifi/nifi-current/conf/cacerts.jks```
 - Modifier le paramètre Truststore Password : ```changeit```
 - Cliquer sur Apply : 
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/87ceea1e-2631-4b33-beec-7f542449ccd8)
 - Cliquer sur Enable :
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/f4322168-5645-4337-8d53-4101784d0891)
 - De la meme facon on va créer un autre process GetHTTP : Apple Stock On va juste changer l'URL https://www.alphavantage.co/query?function=TIME_SERIES_INTRADAY&symbol=AAPL&interval=1min&apikey=YOURAPIKEY&datatype=csv et  le Truststore Filename intraday_5min_AAPL.csv(Vous pouvez faire un copier coller)
  
 ![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/df682200-02c6-4fdd-976c-4390be95b36c)
   
- Ensuite on va créer un autre process UpdateAttribute :
   
![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/05d1e9d8-fa88-4909-9396-fd493a0d115a)
  
- Aller sur UpdateAttribute > Properties > +
   
![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/1f8c3249-69b8-4bb4-a45a-1a3aa6719b80)

- Ajouter filename > OK :
 
![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/5a7be08a-f165-47a0-916d-4027fd577fc6)
- Ajouter la valeur ```${filename:substringBeforeLast('.csv')}_${now():toNumber()}${literal('.csv')}``` au filename 
- Cliquer sur OK  > Apply :
![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/d7535bb6-a4cf-4c29-8b87-40965c6a213f)
- Maintenant on va ajouter des relations entre GetHTTP :IBM Stock et UpdateAttributs et aussi entre GetHTTP : Apple Stock et UpdateAttributs
![F](https://user-images.githubusercontent.com/78825764/193239963-202238ce-dfbb-4cd1-a998-a87ee118251c.png)

- Appuyez sur ADD

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/812ccc1e-bd4b-4772-9c11-2ebf473b9976)

- Maintenant on va ajouter un autre process PutFile
