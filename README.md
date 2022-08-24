## Basic Usage

I used Laravel 8 for example, like this:
```php8
public function getExample(Request $request)
{
    $example = Example::paginate(10);
    return compact('example')
}
```

Register component:
```vue
<script>
// If you are using PurgeCSS, make sure to whitelist the carousel CSS classes
import Pagination from 'vue3-laravel-easy-pagination';

export default {
    name: 'App',
    components: {
        Pagination,
    },
```

Use it like this:
```vue
<template>
    ...
    <Pagination
            :links="linksExample"
            :page-range="2"
            @handler="getExample"
    >
    </Pagination>
    ...
</template>
```



Below you can find basic usage in template, example of data and handler for getting new pages.
Replace `http://127.0.0.1:8000/example` with your link.

```vue
<template>
    ...
    <Pagination
            :links="linksExample"
            :page-range="2"
            @handler="getExample"
    >
    </Pagination>
    ...
</template>
<script>
// If you are using PurgeCSS, make sure to whitelist the carousel CSS classes
import Pagination from 'vue3-laravel-easy-pagination';

export default {
  name: 'App',
  components: {
    Pagination,
  },
  data() {
    return {
      linksExample: {
          "current_page": 1,
          "first_page_url": "http://127.0.0.1:8000/example?page=1",
          "from": 1,
          "last_page": 2,
          "last_page_url": "http://127.0.0.1:8000/example?page=6",
          "links": [
              {
                  "url": null,
                  "label": "&laquo; Previous",
                  "active": false
              },
              {
                  "url": "http://127.0.0.1:8000/example?page=1",
                  "label": "1",
                  "active": true
              },
              {
                  "url": "http://127.0.0.1:8000/example?page=2",
                  "label": "2",
                  "active": false
              },
              {
                  "url": "http://127.0.0.1:8000/example?page=2",
                  "label": "Next &raquo;",
                  "active": false
              }
          ],
          "next_page_url": "http://127.0.0.1:8000/example?page=2",
          "path": "http://127.0.0.1:8000/example",
          "per_page": 1,
          "prev_page_url": null,
          "to": 1,
          "total": 2
        },
      }
    },
  methods: {
      async getExample(page = 1) {
          let params = page > 1 ? `?page=${page}` : ''
          const response = await axios.put(`http://127.0.0.1:8000/example${params}`);
          this.linksExample = response.data.example
      }
  }
}
</script>
```

| Paremeter | Type   | Default | Description                                                                                                              |
|-----------|--------|---------|--------------------------------------------------------------------------------------------------------------------------|
| links     | Object |         | All what you return from laravel                                                                                         |
| pageRange | Number | 1       | How many links do you want near first, last and current (1 means only this links, 2 means this links, 1 prev and 1 next) |
|           |        |         |                                                                                                                          |
