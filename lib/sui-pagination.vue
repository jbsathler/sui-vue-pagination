<template lang="html">
    <sui-menu class="left floated tiny" pagination size="small"><!-- Para omitir paginação se houver apenas 1 página, incluir: v-show="parseInt(totalPages) > 1" -->
        <a is="sui-menu-item" size="small" icon primary :class="{disabled:currentPage == 1}" @click.native="goPage(1)">
            <sui-icon name="left angle double" />
        </a>
        <a is="sui-menu-item" size="small" icon primary :class="{disabled:currentPage == 1}" @click.native="goPage(pageMinusFactor)" v-if="parseInt(factor) > 0">-{{factor}}</a>
        <a is="sui-menu-item" size="small" icon primary :class="{disabled:currentPage == 1}" @click.native="goPage(parseInt(currentPage) - 1)">
            <sui-icon name="left angle" />
        </a>
        <template v-for="pageNumber in pages">
            <a is="sui-menu-item" size="small" v-if="isNaN(pageNumber)" class="disabled" :key="`ellipsis-${pageNumber}`">...</a>
            <a is="sui-menu-item" size="small" v-else @click.native="goPage(pageNumber)" :active="currentPage == pageNumber" :key="pageNumber">{{ pageNumber }}</a>
        </template>
        <a is="sui-menu-item" size="small" icon primary :class="{disabled:currentPage == totalPages}" @click="goPage(parseInt(currentPage) + 1)">
            <sui-icon name="right angle" />
        </a>
        <a is="sui-menu-item" size="small" icon primary :class="{disabled:currentPage == totalPages}" @click.native="goPage(pagePlusFactor)" v-if="parseInt(factor) > 0">+{{factor}}</a>
        <a is="sui-menu-item" size="small" icon primary :class="{disabled:currentPage == totalPages}" @click="goPage(totalPages)">
            <sui-icon name="right angle double" />
        </a>
    </sui-menu>
</template>

<script>
export default {
    name     : "sui-pagination",
    props    : {
        totalPages     : {
            type     : Number,
            default  : 1,
        },
        currentPage    : {
            type     : Number,
            default  : 1,
        },
        maxPagesShowed : {
            type     : Number,
            default  : 6,
        },
        factor         : {
            type     : Number,
            default  : 0,
        },
        goPage         : {
            type     : Function,
            required : true,
        },
    },
    methods  : {
        get pagesShowedWithEllipsis() {
            return this.maxPagesShowed - 1;
        },
        get pagePlusFactor() {
            return (
                (parseInt(`${this.currentPage}`) + parseInt(`${this.factor}`)) > parseInt(`${this.totalPages}`) ? 
                parseInt(`${this.totalPages}`) : 
                (parseInt(`${this.currentPage}`) + parseInt(`${this.factor}`))
            );
        },
        get pageMinusFactor() {
            return (
                (parseInt(`${this.currentPage}`) - parseInt(`${this.factor}`)) < 1 ? 
                1 : 
                (parseInt(`${this.currentPage}`) - parseInt(`${this.factor}`))
            );
        }
    },
    computed : {
        get pages() {
            const pages = [];
            if (parseInt(`${this.totalPages}`) >= parseInt(`${this.maxPagesShowed}`)) {
                if (parseInt(`${this.currentPage}`) < this.pagesShowedWithEllipsis) {
                    for (let page = 1; page <= this.pagesShowedWithEllipsis; page++) {
                        pages.push(page);
                    }
                    pages.push('right');
                } else {
                    pages.push('left');
                    if (parseInt(`${this.currentPage}`) > parseInt(`${this.totalPages}`) - this.pagesShowedWithEllipsis) {
                        for (let page = parseInt(`${this.totalPages}`) - this.pagesShowedWithEllipsis; page <= parseInt(`${this.totalPages}`); page++) {
                            pages.push(page);
                        }
                    } else {
                        for (let page = this.currentPage - 1; page < parseInt(`${this.currentPage}`) + this.pagesShowedWithEllipsis - 2; page++) {
                            pages.push(page);
                        }
                        pages.push('right');
                    }
                }
            } else {
                for (let page = 1; page <= parseInt(`${this.totalPages}`); page++) {
                    pages.push(page);
                }
            }
            return pages;
        }
    },
    mounted() {
        console.log('get pages():', this.pages);
    }
};
</script>
