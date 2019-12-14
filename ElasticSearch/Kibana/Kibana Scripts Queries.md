# Kibana Script - Studies

### Get the month name
Management > Advanced Setting >  Scaled date format > ["P1MT","MMM"] 

``doc['my_field'].value  VS  params['_source']['my_field']``

A script used in the update, update-by-query, or reindex API will have access to the *ctx* variable which exposes:
ctx._source.my_field: Access to the document _source field.

ctx.op: The operation that should be applied to the document: index or delete.

### Query SearchBar
```JSON
{  
   "script":{  
      "script":{  
         "inline":"doc['AdiantamentoSalarioDTO.adiantamentoSalarioStatusDTO.nomeStatus.keyword'].value == params.statusDesembolsado",
         "params":{
             "statusDesembolsado":"Desembolsado"
         }
      }
   }
}
```


### Script Inline
```JSON
"query": {
  "script": {
    "script": {
      "inline": "float sum = 0.0f; for (float v: params['_source'].values()) { sum += v; } return (sum > 50);",
      "lang": "painless"
    }
  }
}
```

### Scripted Field 
``` Java
DateFormat formatter = new SimpleDateFormat("yyyy-MM-dd");
formatter.setTimeZone(TimeZone.getTimeZone("UTC"));

Date today = new Date();
Date vencimento = new Date(doc['AdiantamentoSalarioDTO.dataVencimento'].date.getMillis());

Date todayWithZeroTime = formatter.parse(formatter.format(today));
vencimento = formatter.parse(formatter.format(vencimento));

if (vencimento.getTime() < todayWithZeroTime.getTime() && doc['AdiantamentoSalarioDTO.adiantamentoSalarioStatusDTO.nomeStatus.keyword'].value == "Desembolsado") {
   
         return true;

} else {
    return false;
}
```
### Scripted Field - Count number of days

```
(doc['AdiantamentoSalarioDTO.dataPrevistaDesembolso'].value.getMillis() - doc['AdiantamentoSalarioDTO.dataRequisicao'].value.getMillis()) / 1000 / 60 / 60 / 24
```



### Edit Query DSL 

**EX.: 1**
```JSON
{
    "query": {
        "bool" : {
            "filter" : {
                "script" : {
                    "script" : {
                        "source": "doc['AdiantamentoSalarioDTO.valorDesembolso'].value > 300",
                        "lang": "painless"
                     }
                }
            }
        }
    }
}
```
**EX.: 2 - *constant_score* Wraps a filter query and returns every matching document**
```
{
  "constant_score": {
    "filter": {
      "script": {
        "script": "doc['AdiantamentoSalarioDTO.dataPrevistaDesembolso'].date.getMillis() < doc['AdiantamentoSalarioDTO.dataVencimento'].date.getMillis()"
      }
    }
  }
}
```
**EX.:3 - SQL Version x ElasticDSL Query**
```SQL
SELECT COUNT(*) 
FROM TABLE 
WHERE Message LIKE '%Communication  has failed.%'
  AND [Date] > CONVERT( CHAR(8), GetDate(), 112) + ' 07:40:00'
  AND [Date] < CONVERT( CHAR(8), GetDate(), 112) + ' 22:15:00'

{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "message": "*Communication  has failed*"
          }
        },
        {
          "range": {
            "my_date_field": {
              "gte": "01/01/2018",
              "lt": "01/02/2018",
              "format": "dd/MM/yyyy"
            }
          }
        }
      ]
    }
  },
  "size": 0
}

```
PS: setting a *size* to zero b/c I only want to get back the hits values

EX.: 4 -  Converting the expression A=a1 AND (B=b1 OR C=c1) in ElasticDSL Query
```JSON
{
   "query": {
    "filtered": {
      "query": {
        "query_string": {
          "query": "something to search"
        }
      },
      "filter": {
        "and": [
          {
            "term": { "A": "a1" }
          },
          {
            "or": [
              {
                "term": { "B": "b1 }
              },
              {
                "term": { "C": "c1 }
              }
            ]
          }
        ]
      }
    }
  }
}
```
**EX.: 5** -  Get all documents that have AdiantamentoSalarioDTO.dataRequisicao within certain range or the field is null (does not exist)
```JSON
{
  "query": {
    "bool": {
      "filter": [
        {
          "match_all": {}
        },
        {
          "bool": {
            "should": [
              {
                "bool": {
                  "must_not": {
                    "exists": {
                      "field": "AdiantamentoSalarioDTO.dataRequisicao"
                    }
                  }
                }
              },
              {
                "bool": {
                  "must": {
                    "exists": {
                      "field": "AdiantamentoSalarioDTO.dataRequisicao"
                    }
                  }
                }
              },
              {
                "range": {
                    "AdiantamentoSalarioDTO.dataRequisicao": {
                    "gte": "23/09/2019",
                    "lt": "now",
                    "format": "dd/MM/yyyy"
                    }
                }
              }              
            ]
          }
        }
      ]
    }
  },
  "size": 0
}
```
### Simple Sort  Query
```JSON
POST notify-xerpa-advanced-index/_search
{
   "query" : {
      "term" : { "AdiantamentoSalarioDTO.adiantamentoSalarioStatusDTO.nomeStatus.keyword" : "Desembolsado" }
   },
   "sort" : [
      {"AdiantamentoSalarioDTO.valorDesembolso" : {"order" : "asc", "mode" : "avg"}}
   ]
}
```
### Query to Update Document 

####  Inserting Field
```JSON
POST notify-xerpa-advanced-index/_update_by_query
{ 
   "script":{ 
      "inline":"ctx._source.AdiantamentoSalarioDTO.adiantamentoSalarioStatusDTO.nomeStatus222 = \"okay\"",
      "lang":"painless"
   },
   "query":{ 
      "match":{ 
         "_id":"A345"
      }
   }
}
```
#### Update Fields Value

```json
POST notify-xerpa-advanced-index/_update_by_query
{
  "script": {
    "inline": "ctx._source.AdiantamentoSalarioDTO.adiantamentoSalarioStatusDTO.nomeStatus = \"Cancelado\";  ctx._source.AdiantamentoSalarioDTO.adiantamentoSalarioStatusDTO.siglaStatus = \"CA\"",
    "lang": "painless"
  },
  "query": {
    "match": {
      "_id": "A123"
    }
  }
}


```
#### Updade field - II
```JSON
POST notify-xerpa-advanced-index/_delete_by_query
{ 
   "query":{ 
      "match":{ 
         "_id":"CCBA004/20190924"
      }
   }
}
```
#### Deleting field 
```JSON
POST notify-xerpa-advanced-index/_update_by_query
{ 
   "script":{ 
      "inline":"ctx._source.AdiantamentoSalarioDTO.tarifa = 9",
      "lang":"painless"
   },
   "query":{ 
      "match":{ 
         "_id":"CCBA003/20191014"
      }
   }
}

```


## References
- [How to Script Painless-ly in Elasticsearch](https://www.compose.com/articles/how-to-script-painless-ly-in-elasticsearch/)
- [Script Fields](https://qbox.io/blog/how-to-painless-scripting-in-elasticsearch?utm_source=qbox.io&utm_medium=article&utm_campaign=advanced-methods-for-painless-scripting-with-elasticsearch-part-2%20https://qbox.io/blog/advanced-methods-painless-scripting-elasticsearch-syntax-params?utm_source=qbox.io&utm_medium=article&utm_campaign=advanced-methods-for-painless-scripting-with-elasticsearch-part-2%20https://qbox.io/blog/advanced-methods-for-painless-scripting-with-elasticsearch-part-2?utm_source=qbox.io&utm_medium=article&utm_campaign=painless-scripting-elasticsearch-kibana-scripted-fields%20https://qbox.io/blog/painless-scripting-elasticsearch-kibana-scripted-fields)



