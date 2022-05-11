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
Utiliser
