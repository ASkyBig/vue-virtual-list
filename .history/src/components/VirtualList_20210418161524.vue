<template>
    <div class="viewport" ref="viewport">
        <div class="scrollBar" ref="scrollBar"/>
        <div class="scrollList">

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
        this.$refs.viewport.style.height = this.size * this.remain + 'px';
        this.$refs.scrollBar.style.heigh = this.items.length * this.size + 'px';
    }
}
</script>