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
    }
}
</script>