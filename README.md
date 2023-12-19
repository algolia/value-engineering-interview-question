# Value Engineering: Technical Interview Question

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
- The data is stored in parquet files, and is available in the `data` directory.
- We are not looking for a complete solution, we are interested in seeing how you approach the problem and what you are able to accomplish in the time given.

### Assumptions that can be made:
- If the result is in the result set, we are considering it **shown**

### Appendix:
#### File schemas
##### `queries.parquet`
| column name     | type             |
|-----------------|------------------|
| query_id        | bignumeric       |
| query_text_hash | bignumeric       |
| query_text      | string           |
| result_set      | array\<integer\> |

##### `events.parquet`
| column name | type       |
|-------------|------------|
| query_id    | bignumeric |
| event_name  | string     |
| product_id  | integer    |
| position    | integer    |
