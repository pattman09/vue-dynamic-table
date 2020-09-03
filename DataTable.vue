
<template>
<div id="search-results">
    <div class="divTable">
        <div class="divTableBody">
            <div class="divTableRowHeader header-row">
                <div class="divTableHead text-uppercase" v-for="(header, index) in headers" :key="index">{{header}}</div>
            </div>
            <template v-for="(item, index) in tableContent">
                <div
                    v-bind:class="{'divTableRow':true, 'grey-background':(index % 2)}"
                    :key="index"
                    :id="index"
                    @click="selectedRow(item, $event)">
                    <div class="divTableCell" v-for="(cell, index) in item" :key="index">{{cell}}</div>
                </div>
            </template>
        </div>
    </div>
    <div v-if="rowCount === 0" class="deviceNotFoundMessage">No data found</div>
    <div v-else class="pagingButtons">
        <div v-if="paged">
            <button class="icon-button" @click="back()" :disabled="disableBack"><font-awesome-icon icon="chevron-left"/></button>
            Page {{page}} of {{pageTotal}}
            <button :disabled="disableNext" @click="next()" class="icon-button"><font-awesome-icon icon="chevron-right"/></button>
        </div>
    </div>
</div>
</template>
<script>
    export default {
        props: {
            headers: {},
            tableContent: {},
            rowCount: {
                type: Number,
                default: 100000
            },
            paged: {
                type: Boolean,
                default: true
            }
        },
        data: function () {
            return {
                hidePaging: false,
                page: 1
            }
        },
        methods:{
            sortTable: function(columnName) {
                this.sortBy = columnName;
            },
            selectedRow(device){
                this.$emit('rowSelected', device);
            },
            next() {
                this.page++;
                this.$emit('updateCurrentPage', this.page);
            },
            back() {
                this.page--;
                this.$emit('updateCurrentPage', this.page);
            }
        },
        computed: {
            pageTotal() {
                let pageNum = Math.ceil(this.rowCount / 20);
                return pageNum === 0 ? 1 : pageNum;
            },
            disableNext() {
                return this.page === this.pageTotal ? true : false;
            },
            disableBack() {
                return this.page === 1 ? true : false;
            }
        },
        watch: {
            rowCount() {
                this.page = 1;
            }
        }
    }
</script>

<style lang="scss" scoped>
@import '../assets/styles/variables.scss';

#search-results {
    width: 100%;
    margin-left: auto;
    margin-right: auto;
}

.divTableRow {
    min-width: 375px;
}

.divTableRowHeader {
    color: white;
    background-color: $color-primary;
    min-width: 375px;
}

.divTableHead {
    padding-left: 5px;
}

.divTableBody {
    width: 90%;
    overflow: scroll;
}

.deviceNotFoundMessage {
    text-align: center;
    margin-top: 20px;
    margin-bottom: 20px;
}
</style>