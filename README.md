# SalonsMigration
ressources/réflexions sur la migration de la base Salons de l'EPMO


## Wikidata

#### Peintures et sculptures (et ss-classe) ayant un idTMS et un id de la base Salon
```sparql

SELECT DISTINCT ?item ?itemLabel ?itemDescription ?idTMS ?idBddSalons ?img (CONCAT("https://www.musee-orsay.fr/fr/oeuvres/", STR(?idTMS )) as ?URLbddOrsay) (CONCAT("http://salons.musee-orsay.fr/index/notice/", STR(?idBddSalons )) as ?URLbddSalons) # utilisaiton de CONCAT pour créer tout de suite des URLs

WHERE {
      {?item wdt:P31/wdt:P279* wd:Q3305213} UNION {?item wdt:P31/wdt:P279* wd:Q860861} # classe et sous-classes de peinture et sculptures
      ?item wdt:P195/wdt:P361* ?collection . # qui font partie de musées et de tous ses départements si existant
      ?item wdt:P4659 ?idTMS.
      ?item wdt:P6007 ?idBddSalons. #qui ont un id dans la base Salons
    OPTIONAL {
      ?item wdt:P18 ?img.     
}
    SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]" }
}
```
Accéder à l'export CSV du 11 mai 2022 de la requête SPARQL via l'interface de Flat Viewer : https://flatgithub.com/antoinecourtin/SalonsMigration?filename=query%20%281%29.csv&filters=&sha=b889c9c7c2661e364d368494879c959b8ade3220&sort=img%2Casc&stickyColumnName=itemDescription

#### Peintures et sculptures (et ss-classe) conservées au Louvre et ayant un id de la base Salons
```sparql
SELECT DISTINCT ?item ?itemLabel ?itemDescription ?img ?idLouvre (CONCAT("https://collections.louvre.fr/ark:/53355/cl", STR(?idLouvre )) as ?URLbddLouvre) (CONCAT("http://salons.musee-orsay.fr/index/notice/", STR(?idBddSalons )) as ?URLbddSalons)
WHERE {
      {?item wdt:P31/wdt:P279* wd:Q3305213} UNION {?item wdt:P31/wdt:P279* wd:Q860861} # classe et sous-classes de peinture et sculptures
      ?item wdt:P195/wdt:P361* ?collection . # qui font partie de musées et de tous ses départements si existant
   
    FILTER ( ?collection = wd:Q19675 ) # du musée Louvre
  
  ?item wdt:P9394 ?idLouvre.
      ?item wdt:P6007 ?idBddSalons. #qui ont un id dans la base Salons
    OPTIONAL {
      ?item wdt:P18 ?img.     
}
    SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]" }
}
```


#### Peintures et sculptures (et ss-classe) ayant un id de la base Salon  et récupération des métadonnées descriptives de 1er niveau
```sparql
SELECT DISTINCT ?item ?itemLabel  ?itemDescription  ?creatorLabel ?locationLabel ?datecrea ?idBddSalons ?img # utilisaiton de CONCAT pour créer tout de suite des URLs
WHERE {
      {?item wdt:P31/wdt:P279* wd:Q3305213} UNION {?item wdt:P31/wdt:P279* wd:Q860861} # classe et sous-classes de peinture et sculptures
      ?item wdt:P195/wdt:P361* ?collection . # qui font partie de musées et de tous ses départements si existant
      ?item wdt:P6007 ?idBddSalons. #qui ont un id dans la base Salons
    OPTIONAL {
      ?item wdt:P18 ?img.     
      ?item wdt:P276 ?location.
      ?item wdt:P571 ?datecrea.
      ?item wdt:P170 ?creator.
   
}
    SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]" }
}
```



##### Autres idées/ressources

* [requête dans les collections en ligne d'Orsay qui ont le mot "salon" dans l'indexation iconographique de type "oeuvres représentée"](https://www.musee-orsay.fr/fr/collections/recherche?search=&domain_kind_checkboxes%5B276579%5D=276579&index_artwork_has_picture=1&sort_by=search_api_relevance&items_per_page=15&search_type=advanced_search&display_type=grid&union_artist_names=&artist_nationality=&union_artwork_titles=&artwork_materials=&artwork_start_date=&artwork_conservation_name=&artwork_inventory_number=&artwork_history_title=&artwork_acq_date=&artwork_expositions=&artwork_icono_subject=&artwork_icono_place=&artwork_icono_figure=&artwork_icono_artwork=%22salon+de%22)
