## GET - Search in document

```shell
curl https://sheetdb.io/api/v1/58f61be4dda40/search?name=Steve&age=22&casesensitive=true
```

```php
<?php
$options = array(
  'http' => array(
    'method'  => 'GET'
  )
);

$result = json_decode(
  file_get_contents('https://sheetdb.io/api/v1/58f61be4dda40/search?name=Steve&age=22&casesensitive=true', false, stream_context_create($options))
);
```

```html--javascript
<script src="//cdnjs.cloudflare.com/ajax/libs/axios/0.18.0/axios.js"></script>
<script>
    axios.get('https://sheetdb.io/api/v1/58f61be4dda40/search?name=Steve&age=22&casesensitive=true')
    .then( response => {
        console.log(response.data);
    });
</script>
```

```javascript--node
const sheetdb = require("sheetdb-node");
const client = sheetdb({ address: '58f61be4dda40' });

// Get all rows where column 'id' is 'foo' and column 'value' is 'bar'
client.read({ search: { id: "1", name: "Tom" } }).then(function(data) {
  console.log(data);
}, function(err){
  console.log(err);
});

// Get first row where column 'player' is 'Smith',
// column 'score' is '41' from sheet named "Sheet2"
client.read({
  limit: 1,
  search: { 'player': 'Smith', 'score': 41 },
  sheet: 'Sheet2'
}).then(function(data) {
  console.log(data);
}, function(err){
  console.log(err);
});
```

> Example response:

```json
[
  {
    "id": "61",
    "name": "Steve",
    "age": "22",
    "comment": "special"
  }
]
```

`GET https://sheetdb.io/api/v1/58f61be4dda40/search?name=Steve&age=22&casesensitive=true`

`GET https://sheetdb.io/api/v1/58f61be4dda40/search_or?name=Steve&age=22&casesensitive=true`

Returns an array of all rows matching parameters. If any query fails, the result **will NOT** be listed. If you want to check if any parameter is true, use the search_or endpoint (second URL above).

You can search using wildcards. Asterisk (`*`) can represent any string.
Wildcard work only when READ and SEARCH permissions are both enabled, if only SEARCH peremission is enabled, wildcard will not work for security reasons.

If you want to exclude rows from the search results, use an exclamation mark before the value. For example, if you want all rows without name=Tom use this url: [https://sheetdb.io/api/v1/58f61be4dda40/search?name=**!Tom**](https://sheetdb.io/api/v1/58f61be4dda40/search?name=!Tom)

You can use relational operators to determine if a value is greater than or less than a given number. Just start with `<`, `>`, `<=`, `>=` operator. Example urls:

- Greater than 3: [https://sheetdb.io/api/v1/58f61be4dda40/search?id=>3](https://sheetdb.io/api/v1/58f61be4dda40/search?id=>3)
- Less than 3: [https://sheetdb.io/api/v1/58f61be4dda40/search?id=<3](https://sheetdb.io/api/v1/58f61be4dda40/search?id=<3)
- Greater than or equal to 3: [https://sheetdb.io/api/v1/58f61be4dda40/search?id=>=3](https://sheetdb.io/api/v1/58f61be4dda40/search?id=>=3)
- Less than or equal to 3: [https://sheetdb.io/api/v1/58f61be4dda40/search?id=<=3](https://sheetdb.io/api/v1/58f61be4dda40/search?id=<=3)

You can use multiple queries for the same column, but you must use array notation (`[]` at the end of the variable name), for example: [https://sheetdb.io/api/v1/58f61be4dda40/search?name[]=!Tom&name[]=!Steve](https://sheetdb.io/api/v1/58f61be4dda40/search?name[]=!Tom&name[]=!Steve)

If you want to search for a string with a space, just repace space with `%20`
If you want to include & symbol in your query, replace it with `%26`

You can use optional parameters:

* `limit` - the number of rows that should be returned
* `offset` - row from which it should start (how many rows to skip)
* `sort_by` - the column you want to sort by
* `sort_order` - sort in `asc` or `desc` order
* `sort_method` - if you want to sort by date, set this parameter to `date`, it will automatically detect the date format
* `cast_numbers` - if you want to cast the value to a number, add column names separated by commas. Example: [https://sheetdb.io/api/v1/58f61be4dda40?cast_numbers=id,age](https://sheetdb.io/api/v1/58f61be4dda40?cast_numbers=id,age)
* `single_object` - if you want to get ony one item as an object (not in the array), set this parameter to `true`
* `casesensitive` - by default search is not case sensitive, to make it case sensitive set this parameter to `true`

For more examples, check out this blog post: [Search feature in SheetDB API](https://blog.sheetdb.io/using-the-search-feature-in-sheetdb-api-to-filter-results-from-google-sheets-b2e5f1ae4b) or this youtube video: [Using SheetDB search API to filter results from Google Sheets](https://www.youtube.com/watch?v=hQmmhVxG0UI&ab_channel=SheetDB).
