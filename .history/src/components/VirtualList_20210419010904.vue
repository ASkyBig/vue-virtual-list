<template>
    <div class="viewport" ref="viewport" @scroll="scrollFn">
        <div class="scrollBar" ref="scrollBar"/>
        <div class="scrollList" :style="{transform: `translate3d(0, ${offset}px, 0)`}">
            <div v-for="item in visibleData" :key="item.id" :vid="item.id">
                <slot :item="item"></slot>
            </div>
        </div>
    </div>
</template>
<script>
import throttle from 'lodash'

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
            offset: 0,
            positions: []
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
        getStartIndex (scrollTop) {
            return this.binarySearch(this.positions, scrollTop)
        },
        binarySearch (list, value) {
            let start = 0;
            let end = list.length - 1;
            let tempIndex = null;
            while (start <= end) {
                const midIndex = parseInt((start + end) / 2);
                const midValue = list[midIndex].bottom;
                if (midValue == value) {
                return midIndex + 1;
                } else if (midValue < value) {
                start = midIndex + 1;
                } else if (midValue > value) {
                if (tempIndex == null || tempIndex > midIndex) {
                    tempIndex = midIndex;
                }
                end = midIndex - 1;
                }
            }
            return tempIndex;
            },
        handleScroll () {
            let scrollTop = this.$refs.viewport.scrollTop;
            if (this.variable) {
                this.start = this.getStartIndex(scrollTop);
                this.end = this.start + this.remain;
                this.offset = this.positions[this.start - this.prevCount].top ?this.positions[this.start - this.prevCount].top: 0;
                console.log('this.start =====', this.start);
            } else {
                this.start = Math.floor(scrollTop / this.size);
                this.end = this.start + this.remain;
                this.offset = this.start * this.size - this.size * this.prevCount;
            }
        }
    },
    created () {
        this.scrollFn
    },
    mounted () {
        console.log('items ====', this.items);
        this.$refs.viewport.style.height = this.size * this.remain + 'px';
        this.$refs.scrollBar.style.height = this.items.length * this.size + 'px';
        // 缓存每一项的高度
        // 滚动时，获取渲染后dom真实高度，更新缓存内容
        // 重新计算滚动条高度
        this.cacheList();
    },
    updated () {
        this.$nextTick(() => {
            let nodes = this.$refs.items;
            if (!(nodes && nodes.length > 0)) return;
            nodes.forEach(node => {
                let { height } = node.getBoundingClientRect();
                let id = +node.getAttribute('vid');
                let oldHeight = this.positions[id].height;
                let val = oldHeight - height;
                if (val) {
                    this.positions[id].height = height;
                    this.positions[id].bottom = this.positions[id].bottom - val;
                    for (let i = id + 1; i < this.positions.length; i++) {
                        this.positions[i].top = this.positions[i-1].bottom;
                        this.positions[i].bottom = this.positions[i].bottom - val;
                    }
                }
            })
            this.$refs.scrollBar.style.height = this.positions[this.positions.length - 1].bottom + 'px';
        })
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