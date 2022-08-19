<template>
    <div>
        <ul class="pagination" v-if="pages.length > 3">
        <span v-for="page in pagesData">{{page.label}}</span>
            <li class="page-item" @click="choosePage(firstPageUrl)">
                <a class="page-link cp" v-html="'&laquo;'"></a>
            </li>
            <li v-for="page in pagesData" class="page-item" :class="{active: page.active}" @click="choosePage(page.url)">
                <a class="page-link" :class="{cp: page.url}" v-html="clearPaginateText(page.label)"></a>
            </li>
            <li class="page-item" @click="choosePage(lastPageUrl)">
                <a class="page-link cp" v-html="'&raquo;'"></a>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Pagination',
    props: {
        'pages': Array,
        'pageRange': Number,
        'firstPageUrl': String,
        'lastPageUrl': String,
    },
    data() {
        return {
            pagesData: []
        }
    },
    mounted() {
        if (this.pages.length - 2 > this.pageRange + 1) {
            let newVal = this.pages.slice(0, this.pageRange + 1)
            let start = this.pageRange + 1
            let end = this.pages.length - 3
            for (let i = start; i <= end; i++) {
                if (this.pages[i].active) {
                    newVal.push(this.pages[i])
                } else {
                    if (newVal[i-1]?.url !== undefined) {
                        newVal.push({'label': '...'})
                    }
                }
            }
            newVal = newVal.concat(this.pages.slice(end + 1, this.pages.length))
            this.pagesData = newVal
        } else {
            this.pagesData = this.pages
        }
    },
    methods: {
        choosePage(link) {
            if (link) {
                this.$emit('handler', this.clearPaginateNumber(link))
            }
        },

        isOutRange() {
            if (this.pages.length - 2 > this.pageRange + 1) {
                this.pages = this.pages.splice(this.pageRange + 1, this.pages.length - 3, {
                    'label': '...'
                })
            }
        },
        clearPaginateText(text) {
            return text.replace('&laquo; Previous', '&lsaquo;').replace('Next &raquo;', '&rsaquo;')
        },
        clearPaginateNumber(link) {
            return link.match(/\?page=\d+/i)[0]?.replace('?page=', '') || 1
        }
    },
    watch: {

    }
}
</script>

<style scoped>
.cp {
    cursor: pointer;
}
.pagination {
  --cui-pagination-padding-x: 0.75rem;
  --cui-pagination-padding-y: 0.375rem;
  --cui-pagination-font-size: 1rem;
  --cui-pagination-color: var(--cui-link-color);
  --cui-pagination-bg: #fff;
  --cui-pagination-border-width: 1px;
  --cui-pagination-border-color: #c4c9d0;
  --cui-pagination-border-radius: 0.375rem;
  --cui-pagination-hover-color: var(--cui-link-hover-color);
  --cui-pagination-hover-bg: #d8dbe0;
  --cui-pagination-hover-border-color: #c4c9d0;
  --cui-pagination-focus-color: var(--cui-link-hover-color);
  --cui-pagination-focus-bg: #d8dbe0;
  --cui-pagination-focus-box-shadow: 0 0 0 0.25rem rgba(50, 31, 219, 0.25);
  --cui-pagination-active-color: rgba(255, 255, 255, 0.87);
  --cui-pagination-active-bg: #321fdb;
  --cui-pagination-active-border-color: #321fdb;
  --cui-pagination-disabled-color: #8a93a2;
  --cui-pagination-disabled-bg: #fff;
  --cui-pagination-disabled-border-color: #c4c9d0;
  display: flex;
  list-style: none;
}
html:not([dir=rtl]) .pagination {
  padding-left: 0;
}
*[dir=rtl] .pagination {
  padding-right: 0;
}
.page-link {
  position: relative;
  display: block;
  padding: var(--cui-pagination-padding-y) var(--cui-pagination-padding-x);
  font-size: var(--cui-pagination-font-size);
  color: var(--cui-pagination-color);
  text-decoration: none;
  background-color: var(--cui-pagination-bg);
  border: var(--cui-pagination-border-width) solid var(--cui-pagination-border-color);
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
@media (prefers-reduced-motion: reduce) {
.page-link {
    transition: none;
}
}
.page-link:hover {
  z-index: 2;
  color: var(--cui-pagination-hover-color);
  background-color: var(--cui-pagination-hover-bg);
  border-color: var(--cui-pagination-hover-border-color);
}
.page-link:focus {
  z-index: 3;
  color: var(--cui-pagination-focus-color);
  background-color: var(--cui-pagination-focus-bg);
  outline: 0;
  box-shadow: var(--cui-pagination-focus-box-shadow);
}
.page-link.active, .active > .page-link {
  z-index: 3;
  color: var(--cui-pagination-active-color);
  background-color: var(--cui-pagination-active-bg);
  border-color: var(--cui-pagination-active-border-color);
}
.page-link.disabled, .disabled > .page-link {
  color: var(--cui-pagination-disabled-color);
  pointer-events: none;
  background-color: var(--cui-pagination-disabled-bg);
  border-color: var(--cui-pagination-disabled-border-color);
}
html:not([dir=rtl]) .page-item:not(:first-child) .page-link {
  margin-left: -1px;
}
*[dir=rtl] .page-item:not(:first-child) .page-link {
  margin-right: -1px;
}
html:not([dir=rtl]) .page-item:first-child .page-link {
  border-top-left-radius: var(--cui-pagination-border-radius);
}
*[dir=rtl] .page-item:first-child .page-link {
  border-top-right-radius: var(--cui-pagination-border-radius);
}
html:not([dir=rtl]) .page-item:first-child .page-link {
  border-bottom-left-radius: var(--cui-pagination-border-radius);
}
*[dir=rtl] .page-item:first-child .page-link {
  border-bottom-right-radius: var(--cui-pagination-border-radius);
}
html:not([dir=rtl]) .page-item:last-child .page-link {
  border-top-right-radius: var(--cui-pagination-border-radius);
}
*[dir=rtl] .page-item:last-child .page-link {
  border-top-left-radius: var(--cui-pagination-border-radius);
}
html:not([dir=rtl]) .page-item:last-child .page-link {
  border-bottom-right-radius: var(--cui-pagination-border-radius);
}
*[dir=rtl] .page-item:last-child .page-link {
  border-bottom-left-radius: var(--cui-pagination-border-radius);
}
.pagination-lg {
  --cui-pagination-padding-x: 1.5rem;
  --cui-pagination-padding-y: 0.75rem;
  --cui-pagination-font-size: 1.25rem;
  --cui-pagination-border-radius: 0.5rem;
}
.pagination-sm {
  --cui-pagination-padding-x: 0.5rem;
  --cui-pagination-padding-y: 0.25rem;
  --cui-pagination-font-size: 0.875rem;
  --cui-pagination-border-radius: 0.25rem;
}
</style>
