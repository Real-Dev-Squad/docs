## RDS Query language(RQL) for Filtering, Sorting, and Pagination:

We will be using a query string with a custom format for filtering and sorting, which closely following Github's search formats. You can read more about it [here](https://docs.github.com/en/search-github).

#### Filtering:
- Single search value: `?q=key:value`
- Multiple search values: `?q=key:value1+key:value2`
- Multiple key-value pairs: `?q=key1:value1+key1:value2+key2:value1+key2:value2`
- Exclude a given value: `?q=-key:value`

#### Sorting:
- Ascending order: `?q=sort:key-asc`
- Descending order: `?q=sort:key-desc`

#### Pagination:
- `size`: Number of items per page.
- `page`: Navigate to a specific page.
- `next`: Retrieve the next page.
- `prev`: Retrieve the previous page.

#### Combined Example:
- Filtering, sorting, and pagination together: 
  - `?q=key:value1+key2:value2+-key3:value3+sort:key4-asc&page=2&size=20`
    - This example filters by key1 and key2, excludes key3, sorts by key4 in ascending order, and displays the results on the second page with 20 items per page.
