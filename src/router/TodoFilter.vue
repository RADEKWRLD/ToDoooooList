<template>
     <div class="sorttodo">
        <button @click="$emit('filter','all')">全部</button>
        <button @click="$emit('filter','doing')">进行中</button>
        <button @click="$emit('filter','done')">已完成</button>
        </div>
</template>

<script setup>


</script>