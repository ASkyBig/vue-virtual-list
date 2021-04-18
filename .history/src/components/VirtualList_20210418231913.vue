<template>
    <div class="viewport" ref="viewport" @scroll="handleScroll">
        <div class="scrollBar" ref="scrollBar"/>
        <div class="scrollList" :style="{transform: `translate3d(0, ${offset}px, 0)`}">
            <div v-for="item in visibleData" :key="item.id" :vid="item.id">
                <slot :item="item"></slot>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        size: Number,
        remain: Number,
        items: Array
    },
    data () {
        return {
            start: 0, 
            end: this.remain,
            offset: 0
        }
    },
    computed: {
        visibleData() {
            return this.items.slice(this.start, this.end);
        }
    },
    methods: {
        handleScroll () {
            let scrollTop = this.$refs.viewport.scrollTop;
            this.start = Math.floor(scrollTop / this.size);
            this.end = this.start + this.remain;
        }
    },
    mounted () {
        console.log('items ====', this.items);
        this.$refs.viewport.style.height = this.size * this.remain + 'px';
        this.$refs.scrollBar.style.height = this.items.length * this.size + 'px';
        this.offset = this.start * this.size;

    }
}
</script>
<style>
.viewport {
    position: relative;
    overflow-y: scroll;
}

.scrollList {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
}

</style>