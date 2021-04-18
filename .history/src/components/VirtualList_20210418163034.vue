<template>
    <div class="viewport" ref="viewport">
        <div class="scrollBar" ref="scrollBar"/>
        <div class="scrollList">
            <div v-for="item in items" :key="item.id">
                <slot></slot>
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
    mounted () {
        console.log('items ====', this.items);
        this.$refs.viewport.style.height = this.size * this.remain + 'px';
        this.$refs.scrollBar.style.height = this.items.length * this.size + 'px';
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