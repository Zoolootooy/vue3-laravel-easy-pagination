<template>
    <div>
        <ul class="pagination" v-if="links.links.length > 3">
            <li class="page-item" @click="choosePage(links.first_page_url)">
                <a class="page-link cp" v-html="'&laquo;'"></a>
            </li>
            <li v-for="page in pagesData" class="page-item" :class="{active: page.active}" @click="choosePage(page.url)">
                <a class="page-link" :class="{cp: page.url}" v-html="clearPaginateText(page.label)"></a>
            </li>
            <li class="page-item" @click="choosePage(links.last_page_url)">
                <a class="page-link cp" v-html="'&raquo;'"></a>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Pagination',
    props: {
        'links': Object,
        'pageRange': {
            type: Number,
            default: 1,
        },
    },
    data() {
        return {
            pagesData: []
        }
    },
    mounted() {
        this.updatePages()
    },
    methods: {
        updatePages() {
            if (this.links.links.length - 2 > this.pageRange + 1) {
                let newVal = [];
                let activePageNum = this.links.links.current_page
                for (let i = 0; i < this.links.links.length; i++) {
                    if (this.isRequiredPage(this.links.links[i])) {
                        newVal.push(this.links.links[i])
                    } else if (newVal.slice(-1)[0].label != '...') {
                        newVal.push({'label': '...'})
                    }
                }
                this.pagesData = newVal
            } else {
                this.pagesData = this.links.links
            }
        },
        choosePage(link) {
            if (link) {
                let number = this.clearPaginateNumber(link)
                this.updatePages()
                this.$emit('handler', number)
            }
        },

        isOutRange() {
            if (this.links.links.length - 2 > this.pageRange + 1) {
                this.links.links = this.links.links.splice(this.pageRange + 1, this.links.links.length - 3, {
                    'label': '...'
                })
            }
        },
        clearPaginateText(text) {
            return text.replace('&laquo; Previous', '&lsaquo;').replace('Next &raquo;', '&rsaquo;')
        },
        clearPaginateNumber(link) {
            return link.match(/\?page=\d+/i)[0]?.replace('?page=', '') || 1
        },
        isRequiredPage(page) {
            let result = false
            if (page.label.search('Previous') >= 0 || page.label.search('Next') >= 0) {
                result = true
            } else {
                if (
                        page.label == 1 ||
                        page.label == this.links.last_page ||
                        page.label - this.pageRange <= 0 ||
                        page.label > this.links.last_page - this.pageRange ||
                        page.active ||
                        (page.label >= this.links.current_page - this.pageRange + 1 && page.label <= this.links.current_page + this.pageRange - 1)
                ) {
                    result = true
                }
            }
            return result
        }
    },
    watch: {
        links() {
            this.updatePages()
        }
    }
}
</script>


<style scoped>
.cp {
    cursor: pointer;
}
.pagination {
    display: flex;
    list-style: none;
}

.page-link {
    padding: 0.375rem 0.75rem;
    position: relative;
    display: block;
    color: #0d6efd;
    text-decoration: none;
    background-color: #fff;
    border: 1px solid #dee2e6;
    transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.page-link:hover {
     z-index: 2;
     color: #0a58ca;
     background-color: #e9ecef;
     border-color: #dee2e6;
}

.page-link:focus {
     z-index: 3;
     color: #0a58ca;
     background-color: #e9ecef;
     outline: 0;
     box-shadow: 0 0 0 0.25rem rgb(13 110 253 / 25%);
}

.page-item:first-child .page-link {
    border-top-left-radius: 0.375rem;
    border-bottom-left-radius: 0.375rem;
}

.page-item:last-child .page-link {
    border-top-right-radius: 0.375rem;
    border-bottom-right-radius: 0.375rem;
}

.page-item:not(:first-child) .page-link {
    margin-left: -1px;
}


.page-item.active .page-link {
     z-index: 3;
     color: #fff;
     background-color: #0d6efd;
     border-color: #0d6efd;
 }

.page-item.disabled .page-link {
     color: #6c757d;
     pointer-events: none;
     background-color: #fff;
     border-color: #dee2e6;
}

</style>
