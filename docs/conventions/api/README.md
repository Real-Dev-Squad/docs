### Query Format used for filtering, sorting and pagination:  

We will be using a query string which will be a string of the following format for filtering and sorting.

##### Filtering:
How to represent a single search value: `?q=key:value`

How to represent a Multiple search values: `?q=key:value1+key:value2`

How to represent a Multiple key value pairs: 
`?q=key1:value1+key1:value2+key2:value1+key2:value2`

To exclude a given value: `?q=-key:value`
##### Sorting:
To sort ascending `?q=key-asc`
To sort descending `?q=key-desc`

##### For pagination:
`size`
`page`
`next`
`prev`
