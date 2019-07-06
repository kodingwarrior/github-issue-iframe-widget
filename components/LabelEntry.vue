<template>
    <div class="label" :style='{ backgroundColor: "#" + label.color }'>
        <a href="#">{{ label.name }}</a>
    </div>
</template>

<script>
export default {
    props: ['label']
}
</script>

<style scoped>
.label {
    display: inline-block;

    height: 30px;
    line-height: 30px;    
    border-radius: 4px;
    padding: 5px;

    margin-right: 10px;

    word-break: keep-all;
}

.label > a {
    display: block;
    width: 100%;
    height: 100%;
    line-height: 100%;

    text-decoration: none;
    color: #000;
    font-weight: 900;
}
</style>