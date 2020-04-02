# Query and filter context

## Relevance scoresedit

By default, Elasticsearch sorts matching search results by **relevance score**, which measures how well each document matches a query.

The relevance score is a positive floating point number, returned in the `_score` meta-field of the search API. The higher the `_score`, the more relevant the document. While each query type can calculate relevance scores differently, score calculation also depends on whether the query clause is run in a **query** or **filter** context.

## Query context

In the query context, a query clause answers the question “How well does this document match this query clause?” Besides deciding whether or not the document matches, the query clause also calculates a relevance score in the `_score` meta-field.

Query context is in effect whenever a `query` clause is passed to a query parameter, such as the `query` parameter in the search API.

## Filter context

In a filter context, a query clause answers the question “Does this document match this query clause?” The answer is a simple Yes or No — no scores are calculated. Filter context is mostly used for filtering structured data, e.g.

- Does this `timestamp` fall into the range 2015 to 2016?
- Is the `status` field set to `"published"`?

Frequently used filters will be cached automatically by Elasticsearch, to speed up performance.

Filter context is in effect whenever a query clause is passed to a `filter` parameter, such as the `filter` or `must_not` parameters in the `bool` query, the `filter` parameter in the `constant_score` query, or the `filter` aggregation.

## Example of query and filter contextsedit

Below is an example of query clauses being used in query and filter context in the `search` API. This query will match documents where all of the following conditions are met:

- The `title` field contains the word `search`.
- The `content` field contains the word `elasticsearch`.
- The `status` field contains the exact word `published`.
- The `publish_date` field contains a date from 1 Jan 2015 onwards.

```JSON
GET /_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "Search"
          }
        },
        {
          "match": {
            "content": "Elasticsearch"
          }
        }
      ],
      "filter": [
        {
          "term": {
            "status": "published"
          }
        },
        {
          "range": {
            "publish_date": {
              "gte": "2015-01-01"
            }
          }
        }
      ]
    }
  }
}
```

- The `query` parameter indicates query context.
- The `bool` and two `match` clauses are used in query context, which means that they are used to score how well each document matches.
- The `filter` parameter indicates `filter` context. Its `term` and `range` clauses are used in `filter` context. They will filter out documents which do not match, but they will not affect the score for matching documents.

>
Scores calculated for queries in query context are represented as single precision floating point numbers; they have only 24 bits for significand’s precision. Score calculations that exceed the significand’s precision will be converted to floats with loss of precision.

>
Use query clauses in query context for conditions which should affect the score of matching documents (i.e. how well does the document match), and use all other query clauses in filter context.
