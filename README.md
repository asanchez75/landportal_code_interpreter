# landportal_code_interpreter

* [State and challenges of the land around the world](https://htmlpreview.github.io/?https://raw.githubusercontent.com/asanchez75/landportal_code_interpreter/main/Land%20Indicators%20-%20EDA%20for%20%20state%20and%20challenges%20of%20the%20land%20.html)

* [Indicators that can be used to support the Sol Index and its challenges](https://htmlpreview.github.io/?https://raw.githubusercontent.com/asanchez75/landportal_code_interpreter/main/SOLIndex%20and%20Challenges.html)
  
* [5 questions about indicators](https://htmlpreview.github.io/?https://raw.githubusercontent.com/asanchez75/landportal_code_interpreter/main/5%20questions%20about%20indicators.html)

* [Formulation of hypothesis to be tested through data analysis.html](https://htmlpreview.github.io/?https://raw.githubusercontent.com/asanchez75/landportal_code_interpreter/main/Formulation%20of%20hypothesis%20to%20be%20tested%20through%20data%20analysis.html)


## SPARQL query to extract data into CSV format


```sparql
PREFIX cex: <http://purl.org/weso/ontology/computex#> 
PREFIX time: <http://www.w3.org/2006/time#> 
PREFIX qb: <http://purl.org/linked-data/cube#> 
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> 
PREFIX dct: <http://purl.org/dc/terms/> 
PREFIX sdmx-attribute: <http://purl.org/linked-data/sdmx/2009/attribute#>  
SELECT ?indicator ?indicatorName ?country ?countryName ?time ?value ?note 
FROM <http://data.landportal.info> 
FROM <http://countries.landportal.info> 
FROM <http://indicators.landportal.info> 
WHERE { 
?obs cex:ref-indicator ?bindicator; 
cex:ref-area ?bcountry; 
cex:ref-time ?btime; 
cex:value ?bvalue . 
?bindicator rdfs:label ?revindicatorName. 
?bcountry rdfs:label ?revcountryName 
OPTIONAL { ?obs rdfs:comment ?bnote } 
BIND(REPLACE(STR(?bindicator), 'http://data.landportal.info/indicator/', '') AS ?indicator)
BIND(STR(?revindicatorName) AS ?indicatorName)
BIND(REPLACE(STR(?bcountry), 'http://data.landportal.info/geo/', '') AS ?country) 
BIND(STR(?revcountryName) AS ?countryName) 
BIND(REPLACE(STR(?btime), 'http://data.landportal.info/time/', '') AS ?time) 
BIND(STR(IF(DATATYPE(?bvalue)=xsd:string, STR(?bvalue), ?bvalue)) AS ?value) 
BIND(STR(IF(DATATYPE(?bnote)=xsd:string, STR(?bnote), ?bnote)) AS ?note) 
{
select ?bindicator where { 
    ?bindicator ?p <http://purl.org/weso/ontology/computex#Indicator> .
} limit 100 
}
} 
ORDER BY ?time
```
