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
        items: Array,
        variable: Boolean
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
            let start = this.start - this.prevCount;
            let end = this.end + this.nextCount;
            return this.items.slice(start, end);
        },
        prevCount() {
            return Math.min(this.start, this.remain)
        },
        nextCount() {
            return Math.min(this.remain, this.items.length - this.end)
        }
    },
    methods: {
        cacheList () {
            this.positions = this.items.map((item, index) => {
                return {
                    height: this.size,
                    top: index * this.size,
                    bottom: (index + 1) * this.size
                }
            })
            console.log('this.positions ====', this.positions);
        },
        handleScroll () {
            let scrollTop = this.$refs.viewport.scrollTop;
            this.start = Math.floor(scrollTop / this.size);
            this.end = this.start + this.remain;
            this.offset = this.start * this.size - this.size * this.prevCount;

        }
    },
    mounted () {
        console.log('items ====', this.items);
        this.$refs.viewport.style.height = this.size * this.remain + 'px';
        this.$refs.scrollBar.style.height = this.items.length * this.size + 'px';
        // 缓存每一项的高度
        this.cacheList();
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