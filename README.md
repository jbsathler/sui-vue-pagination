# sui-vue-pagination

A pagination component for [Semantic-UI-Vue](https://github.com/Semantic-UI-Vue/Semantic-UI-Vue).

## Table of contents

- [Getting started](#getting-started)
- [Usage](#usage)
- [Available Props](#available-props)
- [License](#license)
- [Keywords](#keywords)

## Getting started

Install the npm package:

```bash
npm install sui-vue-pagination
#OR
yarn add sui-vue-pagination
```

## Usage

Use the `<sui-pagination>` component:

```vue
<template>
    <div>
        <sui-pagination
            :totalPages="pageInfo.totalPages"
            :currentPage="pageInfo.currentPage"
            :maxPagesShowed="maxPagesShowed"
            :factor="factor"
            :goPage="getPageData"
        />
    </div>
</template>

<script>
import sui-pagination from "sui-vue-pagination";

export default {
    components: {
        sui-pagination
    },
    data: function() {
        return {
            loading        : false,
            maxPagesShowed : 10,
            factor         : 5,
            items          : [],
            pageInfo       : {
                totalPages   : 1,
                currentPage  : 1,
                itemsPerPage : 10
            }
       };
    },
    computed: {
        get itemsPerPage() {
            return this.pageInfo.itemsPerPage;
        }
    },
    methods: {
        /** 
         * Callback Function to (re)load page data.
         * 
         * This function is REQUIRED and can be named as you like, as long as you pass the 
         * name to component's goPage prop. It will be called whenever a user click a page
         * number on pagination component and will pass the page number clicked as the first
         * and only argument.
         * 
         * The content is just an example of implementation and you should impement the 
         * corresponding way to access the backend api that will populate your page. In the
         * same way, the computed prop itemsPerPage() and the method queryString are just 
         * helpers in this example aproach.
         */
        getPageData = (page) => {
            this.loading = true;
            const query  = this.queryString({ page: page, itemsPerPage: this.itemsPerPage });
            this.$axios.get(`${this.route}${query}`).then(response => {
                this.catalogFields = Object.assign(this.catalogFields, response.data);
                this.loading       = false;
            });
        },
        queryString(params) {
            const queryString = [];
            if (params.page) {
                queryString.push(`page=${params.page}`);
            }
            if (params.itemsPerPage) {
                queryString.push(`limit=${params.itemsPerPage}`);
            }
            return queryString.length > 0 ? `?${queryString.join('&')}` : '';
        }

    }
};
</script>
```

## Available Props

| Prop           | Type     | default | Description                                                                                                                                         |
| -------------- | -------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| totalPages     | Number   | 1       | Total number of pages to be displayed (eventually this will be the number of the last page)                                                         |
| currentPage    | Number   | 1       | Number of actual page selected and showed                                                                                                           |
| maxPagesShowed | Number   | 6       | Maximum number of pages to be showed simultaneously on component. Exceeding pages will be replaced by ellipsis.                                     |
| factor         | Number   | 0       | Number of pages to be skipped by multipage jump buttons (e.g. factor=5 renders -5 and +5 buttons that allow user to jump 5 pages in each direction) |
| goPage         | Function |         | Callback function called when a page number is clicked by user (**required**)                                                                       |

---
## License

[MIT](./LICENSE)

---
## Keywords

sui-vue-pagination, vue-pagination, pagination, vue component, vuejs component, vuejs, vue.js, semantic-ui-vue, semantic-ui
