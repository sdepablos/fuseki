# RDF reading

Tutorial: 

## Docker for Apache Jena Fuseki

We're using docker image from https://hub.docker.com/r/stain/jena-fuseki/. To run it execute

```bash
docker run --rm --name fuseki -p 3030:3030 -e ADMIN_PASSWORD=<pass> -v $(pwd)/datasets:/staging stain/jena-fuseki
```

## SPARQL (RDF query language)

There's a good tutorial at https://jena.apache.org/tutorials/sparql.html 

### Some examples

Obtain first hundred triplets 

```sql
SELECT ?subject ?predicate ?object
WHERE {
  ?subject ?predicate ?object
}
LIMIT 100
```

Obtain the identifier and name of businesses (the predicate type for business name is `<http://www.w3.org/2006/vcard/ns#fn>`)

```sql
SELECT ?subject ?object
WHERE {
  ?subject <http://www.w3.org/2006/vcard/ns#fn> ?object
}
LIMIT 100
```

Obtain the identifier and name of businesses from category `c0050202001001`. We're also prefixing vcard to avoid writing it multiple times

```sql
PREFIX vcard: <http://www.w3.org/2006/vcard/ns#>

SELECT ?subject ?object
WHERE {
  ?subject vcard:fn ?object .
  ?subject vcard:category <http://www.bcn.cat/data/asia/categories#c0050202001001> .
}
LIMIT 100
```

```sql
PREFIX vcard: <http://www.w3.org/2006/vcard/ns#>

SELECT ?subject ?object ?code
WHERE {
  ?subject vcard:fn ?object .
  ?subject vcard:category ?code .
  FILTER (
    ?code = <http://www.bcn.cat/data/asia/categories#c0040101007401010060040101007> || 
    ?code = <http://www.bcn.cat/data/asia/categories#XXXXX> 
  )
}
LIMIT 100
```