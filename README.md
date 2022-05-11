# SalonsMigration
ressources/réflexions sur la migration de la base Salons de l'EPMO


## Wikidata

```sparql

SELECT DISTINCT ?item ?itemLabel ?itemDescription ?idTMS ?idBddSalons ?img (CONCAT("https://www.musee-orsay.fr/fr/oeuvres/", STR(?idTMS )) as ?URLbddOrsay) (CONCAT("http://salons.musee-orsay.fr/index/notice/", STR(?idBddSalons )) as ?URLbddSalons)  WHERE {
      {?item wdt:P31/wdt:P279* wd:Q3305213} UNION {?item wdt:P31/wdt:P279* wd:Q860861} # classe et sous-classes de peinture et sculptures
      ?item wdt:P195/wdt:P361* ?collection . # qui font partie de musées et de tous ses départements si existant
    FILTER ( ?collection = wd:Q23402 ) # du musée d'Orsay
      ?item wdt:P4659 ?idTMS.
      ?item wdt:P6007 ?idBddSalons. #qui ont un id dans la base Salons
    OPTIONAL {
      ?item wdt:P18 ?img.     
}
    SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]" }
}
```
Accéder à l'export CSV du 11 mai 2022 de la requête SPARQL via l'interface de Flat Viewer : https://flatgithub.com/antoinecourtin/SalonsMigration?filename=query%20%281%29.csv&filters=&sha=b889c9c7c2661e364d368494879c959b8ade3220&sort=img%2Casc&stickyColumnName=itemDescription
