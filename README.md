# Value Engineering: Technical Interview Question

## Introduction
The purpose of this exercise is to assess your ability to solve a real-world problem using data. 
The exercise is designed to be completed in 2-3 hours, but you are free to spend as much time as you like.

## Task:

For the top 1000 queries by query volume (total number of queries), identify the best and worst performing results for each query.

**To do this you will need to:**
1. Compute the top 1000 queries by query volume
2. For each result in the result set for each query, compute the number of times the result was shown and the number of times the result was clicked
3. Compute the `rp_score` for each result 
   - The `rp_score` is computed as follows:
      - `show_rate = number of times a result was shown / total number of queries`
      - `click_rate = number of times a result was clicked / number of times a result was shown`
      - `rp_score = (show_rate * click_rate) * 1000`
4. For each query, identify the best and worst performing results
   - The best performing result for a query is the result with the highest `rp_score`.
   - The worst performing result for a query is the result with the lowest `rp_score`.

### Guidelines:
- The task can be solved in either SQL or Python.
  - For SQL, [`duckdb`](https://duckdb.org/) is recommended, but you can use any SQL dialect you like. If you a different dialect, please include a `README.md` file with instructions on how to run your solution.
- The data is stored in parquet files, and is available in the `data` directory.
- You should consider how you would validate your solution, and maintain it over time.
- Your solution should be able to handle 100x the amount of data provided.

### Assumptions that can be made:
- If the result is in the result set, we are considering it **shown**

### Appendix:
#### File schemas
##### `queries.parquet`
| column name     | type             | description                           |
|-----------------|------------------|---------------------------------------|
| query_id        | bignumeric       | unique id for the query               |
| query_text_hash | bignumeric       | hash of the query text                |
| query_text      | string           | query text                            |
| result_set      | array\<integer\> | list of product ids in the result set |

##### `events.parquet`
| column name | type       | description                                   |
|-------------|------------|-----------------------------------------------|
| query_id    | bignumeric | unique id for the query                       |
| event_name  | string     | The name of the event                         |
| product_id  | integer    | The id of the product                         |
| position    | integer    | The position of the product in the result set |


#### Output of Top 10 Queries
> [!NOTE]
> The there tend to be a number of results with the same `rp_score` for a given query at the lower end of the `rp_score` spectrum.
> For this reason, the `worst_result_id` that is returned may not be the worst result for the query, but it will be one of the worst results for the query. 

| query_text      | best_result_id | best_result_rp_score | worst_result_id | worst_result_rp_score |
|-----------------|----------------|----------------------|-----------------|-----------------------|
| new balance     | 204275708      | 42.73                | 204738244       | 0.01                  |
| nike            | 203989009      | 6.1                  | 202993400       | 0.01                  |
| converse        | 203645100      | 54.7                 | 204792999       | 0.02                  |
| shorts          | 204168394      | 9.24                 | 204880970       | 0.02                  |
| jeans           | 200549201      | 11.75                | 204080436       | 0.02                  |
| adidas          | 203497397      | 7.19                 | 204849666       | 0.02                  |
| bikini          | 203606817      | 15.35                | 205036639       | 0.02                  |
| crocs           | 202742989      | 48.7                 | 202888372       | 0.02                  |
| new balance 530 | 204470705      | 169.29               | 201097378       | 0.03                  |
| jumpsuit        | 203355304      | 18.85                | 204823721       | 0.03                  |