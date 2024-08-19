# Elasticsearch Query Collection

## Basic CRUD Operations
### Get a Document by ID
get products/_doc/1

### Get the Source of a Document
get products/_source/1

### Get Shards Information for an Index
get _cat/shards/products

### Create or Update a Document (Without Refresh)
put products/_doc/20?refresh=false
{
  "name":"Iphone 11"
}

### Create a New Document (Auto-generated ID)
post products/_doc
{
  "name":"Iphone 11"
}
### Update a Document by ID
put products/_doc/25
{
  "name":"Iphone 11",
  "phonePrice": "15000"
}

### Update Index Settings
put products/_settings
{
  "index":{"refresh_interval":"5s"}
}

### Partially Update a Document
post products/_update/20
{
  "doc":{
    "name":"iphone 12"
  }
}

### Delete a Document by ID
delete products/_doc/25

### Check if a Document Exists
head products/_doc/25

## Search Queries
### Match All Documents
get products/_search
{
  "query":{"match_all":{}}
}

### Multi-Get Documents by IDs
get products/_mget
{
  "ids":[20,"8dkqKJEBwI2OvwcgXNv7"]
}

### Get the Source of a Document by ID
get products/_source/20

### Get Specific Fields from a Document
get products/_doc/25?_source_includes=name,phonePrice

## Index Management
### Define Index with Mappings
PUT products
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "price": {
        "type": "long"
      },
      "stock_no": {
        "type": "keyword"
      },
      "warehouse": {
        "properties": {
          "germany": {
            "type": "integer"
          },
          "turkey": {
            "type": "integer"
          }
        }
      }
    }
  }
}

### Update Index Mappings to Add a New Field
PUT products/_mapping
{
  "properties":{
    "color":{
      "type":"keyword"
    }
  }
}

### Update Field Mappings to Add Multi-Field
PUT products/_mapping
{
  "properties":{
    "name":{
    "type":"text",
    "fields":{
      "keyword":{
        "type":"keyword"
      }
    }
  }
  }
}

### Get Index Mappings
get products/_mapping

### Create a Document with Nested Fields
put product/_doc/1
{
  "name":"kalem 1",
  "price":33,
  "stock_no":3223,
  "warehouse":{
    "germany":123,
    "turkey":123
  }
}

## Search Queries with Filters
### Search for a Specific Term
get product/_search
{
  "query":{
    "term": {
      "name": {
        "value": "kalem 1"
      }
    }
  }
}

### Search for a Term in a Specific Field
get kibana_sample_data_ecommerce/_search
{
  "query":{
    "term": {
      "customer_first_name.keyword": {
        "value": "sonya",
        "case_insensitive":true
      }
    }
  }
}

### Search by Multiple Terms
get kibana_sample_data_ecommerce/_search
{
  "query":{
    "terms": {
      "customer_first_name.keyword": ["Sonya","Yasmine"]
    }
  }
}

### Search by Document IDs
get kibana_sample_data_ecommerce/_search
{
  "query":{
    "ids": {
      "values": ["xtnzJ5EBwI2Ovwcgl8mb","3dnzJ5EBwI2Ovwcgl8mb"]
    }
  }
}

### Prefix Query
get kibana_sample_data_ecommerce/_search
{
  "query":{
    "prefix": {
      "customer_first_name.keyword": {
        "value": "Edd"
      }
    }
  }
}

### Range Query
get kibana_sample_data_ecommerce/_search
{
  "query":{
    "range": {
      "taxful_total_price": {
        "gte": 10,
        "lte": 20
      }
    }
  }
}

### Wildcard Query
get kibana_sample_data_ecommerce/_search
{
  "query":{
    "wildcard": {
      "customer_full_name.keyword": {
        "value": "* Perkins"
      }
    }
    }
}

### Fuzzy Query
get kibana_sample_data_ecommerce/_search
{
  "query":{
    "fuzzy": {
      "customer_first_name.keyword": {
        "value": "ssdie",
        "fuzziness": 2
      }
    }
  }
}

### Fuzzy Query with Pagination
get kibana_sample_data_ecommerce/_search
{
  "from":0,
  "size":3,
  "query":{
    "fuzzy": {
      "customer_first_name.keyword": {
        "value": "ssdie",
        "fuzziness": 2
      }
    }
  }
}

### Select Specific Fields and Fuzzy Query
get kibana_sample_data_ecommerce/_search
{
  "_source":{
    "includes":["category","currency","customer_first_name"]
  },
  "query":{
    "fuzzy": {
      "customer_first_name.keyword": {
        "value": "ssdie",
        "fuzziness": 2
      }
    }
  }
}

### Select Specific Fields, Fuzzy Query, and Sort
get kibana_sample_data_ecommerce/_search
{
  "_source":{
    "includes":["category","taxless_total_price","customer_first_name"]
  },
  "query":{
    "fuzzy": {
      "customer_first_name.keyword": {
        "value": "ssdie",
        "fuzziness": 2
      }
    }
  },
  "sort":{
    "taxless_total_price":{
      "order":"desc"
    }
  }
}

### Match Query for Specific Category
post kibana_sample_data_ecommerce/_search
{
  "query":{
    "match": {
      "category": "Men's"
    }
  }
}

### Match Query with OR Operator
post kibana_sample_data_ecommerce/_search
{
  "query":{
    "match": {
      "customer_full_name": {
        "query":"test goodwin",
        "operator":"or"
      }
    }
  }
}













