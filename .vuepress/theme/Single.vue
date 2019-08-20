<template>
    <div>
        <div class="backlink">
            <a href="/">← Home</a>
        </div>
        <div class="content">
            <h1 class="title">{{ $page.frontmatter.title }}</h1>
            <Content></Content>
        </div>
    </div>
</template>
<script>
    export default {};
</script>

<style>
    p {
        margin-block-start: 1em;
    }
</style>