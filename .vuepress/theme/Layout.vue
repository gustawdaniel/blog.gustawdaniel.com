<template>
    <div class="full-layout">
        <header class="hero has-background-info">
            <div class="hero-body">
                <div class="container has-text-white">
                    <h1 class="title has-text-white has-text-centered">{{ $site.title }}</h1>
                </div>
            </div>
        </header>
        <section class="section layout-main">
            <div class="theme-default-content">
                <Index v-if="isIndex"/>
                <Single v-if="!isIndex"/>
            </div>
        </section>
        <footer class="footer">
            <div class="content container">
                <p class="level">
                    <strong class="level-left">Daniel Gustaw</strong>
                    <span class="level-right">
                        <a target="_blank" href="http://github.com/gustawdaniel" class="mx-1 icon has-text-grey"><i class="fa-lg fab fa-github"></i></a>
                        <a target="_blank" href="https://stackoverflow.com/users/6398044/daniel" class="mx-1 icon has-text-grey"><i class="fa-lg fab fa-stack-overflow"></i></a>
                    </span>
                </p>
<!--                <hr>-->
<!--                <p class="is-family-monospace">version: 1.2.3</p>-->
            </div>
        </footer>
    </div>
</template>
<script>
    import Index from './Index';
    import Single from './Single';

    export default {
        components: {
            Index,
            Single,
        },
        computed: {
            isIndex() {
                return this.$page.path.endsWith("/");
            }
        },
    };
</script>

<style lang="scss">
    @import "style/bluma";
    @import "style/lato";
    @import "style/custom";
    @import "style/prism-darcula";
</style>
