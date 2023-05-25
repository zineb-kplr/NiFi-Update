# Maintenant on passe à l'étape 2 : Push To Kafka

- Naviguer vers la page des process group en cliquant sur cette icône :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/a4731d07-95eb-44df-9da6-3ddb0027172f)

- Double cliquez sur le process groupe PushToKafka

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/50c87bcb-71d1-4cad-a182-5519015acc63)

- Ajoutez un nouveau Processor GetFile

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/f6e58219-60fa-44a5-ada5-0a4108459b58)

- Cliquer sur ADD 

- Double cliquer sur GetFile :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/a134e24e-c4ff-436e-aab4-45b7a7a4e83a)

- Ajoutez data/input en InputDirectory :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/9f87228b-9b7a-42c5-b19a-d50e5ae3edbd)

- Ajoutez  ```.*\.csv``` en file filter :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/3f9fb48a-66c3-430a-abbb-eee4a93e0616)

- Cliquer sur APPLY :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/5e8a8914-28eb-4773-a3fa-adf0d6324259)

- Ajoutez un autre process UpdateAttribute

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/725b0ce8-7265-4ae0-a088-0b37bf162352)

- Cliquer sur ADD : 

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/e3aff856-abf1-4a64-908c-64cb21d7b7c4)

- Double cliquez sur UpdateAttribute

- Ajouter une nouvelle property topic > OK:

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/cbee1c6d-93a9-49c1-8601-f5522e19efc7)

- Ajoutez ```{filename:substringBeforeLast("_"):substringAfterLast("_")}``` au topic`:

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/e53c2a60-9379-4788-b768-5bf242762f7f)

- Ajouter une nouvelle property schema.name> OK:

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/55feca07-d357-4006-beab-352b0a7b2941)

- Ajoutez stock à schema.name :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/41d1f6cf-ef1f-415f-924a-1ceadd07a4ae)

- Cliquer sur Apply :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/b5827755-2674-45db-a61c-b94a6f23b85a)

- Ajouter une connexion entre GetFile et UpdateAttribute :  
- Cliquer sur ADD :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/03a5349c-d109-4801-b49f-d10618e34fb2)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/ce84da74-c297-4143-a963-a8c56c567082)

- Ajoutez un nouveau Process PublishKafkaRecord_2_0 :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/ca978897-aa8a-4dd6-9c7f-916530eb0ef6)

- Cliquer sur ADD 

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/2069b47a-4ed6-45a7-baa0-f08b8987676e)

- Double cliquez sur PublishKafkaRecord

- Ajouter ```kafka:9092``` à kafka Brokers :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/7e678a28-4007-4768-a79f-6e5d790b52c1)


- Ajouter ```${topic}``` à topic name

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/5452d978-dfcb-41ce-91b6-5fd06763c197)

- Record Reader > Reference Parameter > create new service > CSVReader 1.21.0 > Create :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/32a98697-4f1a-4b9f-a4a5-d0202389d820)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/16a775c5-2dfb-4829-b93d-2f26debc9230)

- Record Writer > Reference Parameter > create new service > CSVRecordSetWriter 1.21.0 > Create :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/820ffbef-a71c-4513-9266-5fac44034546)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/2170a4c2-8f52-4bbb-bfb0-983f58379ff0)

- Cliquez sur la flèche de Record Reader :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/6d08dd3b-94df-4286-8cd0-8ba8f2dd1da5)

- Cliquez sur le petit icon paramètre à la droite

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/45242414-ddfc-401f-8f72-c5eb8077f6e3)

- Sélectionner Infer Schema > Use 'Schema Text' Property :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/62928a96-7d05-4a02-a6b0-7737c0588e84)


- Ajoutez schema text :

Ajoutez schema text :
```
{
  "name": "alpha",
  "type": "record",
  "fields": [
    { "name": "timestamp", "type": "string" },
    { "name": "open", "type": "double" },
    { "name": "high", "type": "double" },
    { "name": "low", "type": "double" },
    { "name": "close", "type": "double" },
    { "name": "volume", "type": "double" }
    ]
}
```

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/6908d5a5-52c2-4646-9382-f2385e1cb1f7)

- Modifier Treat First Line as Header > True

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/28595cb6-2534-4178-96a6-0827ccbe645c)

- Cliquer sur Ok :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/3597827e-b834-4fc0-9e60-b24a65f9e1f0)

- Cliquer sur Apply 
- Maintenant vous allez cliquez sur l'icon enable :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/62a04fb1-4c57-4087-89f9-2fbe8ced1468)

- Maintenant , créez une relation entre le Process GetFile et Update Attribute de type success

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/7b2db4ac-7a97-4c30-89ef-6f75563cc69d)

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/244b589b-f920-47e9-9aae-50b8934b3275)

- On ajoute deux output

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/1ddfe301-6b18-47fc-80fd-24c893eadd42)

- Un avec le nom SUCCESS :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/2609c2e4-712e-4f01-a765-758078071ea1)

- Et l'autre avec le nom FAILURE :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/b6ebf0c8-7e44-48eb-9c40-3ea1d6b40cb9)

- Après vous liez ces deux output avec PublishKafkaRecord , une avec la relation Success et l'autre avec la relation failure :

![image](https://github.com/zineb-kplr/NiFi-Update/assets/123749462/3978934d-9608-4735-a6e7-109552205646)

- Executer ensuite les 3 process :





