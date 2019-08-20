<template>
    <div>
        <header class="hero has-background-info">
            <div class="hero-body">
                <div class="container">
                    <h1 class="title has-text-white">{{ $site.title }}</h1>
                </div>
            </div>
        </header>
        <section class="section">
            <div class="container">
                <Index v-if="isIndex"/>
                <Single v-if="!isIndex"/>
            </div>
        </section>
    </div>
</template>
<script>
    import Index from './Index';
    import Single from './Single';

    // import 'bulma/css/bulma.css'


    export default {
        components: {
            Index,
            Single
        },
        computed: {
            isIndex() {
                return this.$page.path.endsWith("/");
            }
        }
    };
</script>

<style lang="scss">
    @import "style/bluma";

    * {
        font-family: 'Lato', sans-serif !important;
    }
</style>