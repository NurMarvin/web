<template>
    <div class="root">
        <Header :dark=isDarkTheme />
        <div class="content" :class="{ 'is-dark': isDarkTheme }">
            <nuxt />
            <GoogleAnalytics v-if=brand.ga_tracking_id />
        </div>
    </div>
</template>
<script>
    import { mapGetters } from 'vuex'
    import { parse } from 'cookieparser'
    
    import brand from '~/brand/config'

    import Header from '~/components/header/index'
    import GoogleAnalytics from '~/components/analytics'

    export default {
        head() {
            const script = []

            if(this.brand.ga_tracking_id)
                script.push({
                    src: `https://www.googletagmanager.com/gtag/js?id=${this.brand.ga_tracking_id}`
                })

            return {
                titleTemplate: chunk => this.$route.name === 'room' && this.room ? `${this.room.name} - ${this.brand.name}` : (chunk ? `${chunk} - ${this.brand.name}` : this.brand.name),
                meta: [
                    { charset: 'utf-8' },
                    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
                    { name: 'description', content: `${this.brand.name} makes it easy to enjoy what you love with your friends` },
                    { name: 'theme-color', content: '#000000' },
                    { property: 'og:image', content: '/img/icon-hq.png'}
                ],
                link: [
                    { rel: 'icon', type: 'image/x-icon', href: '/favicon.png' }
                ],
                script: [
                    ...script
                ]
            }
        },
        computed: {
            ...mapGetters(['room']),
            isDarkTheme() {
                return this.$route.name === 'room'
            }
        },
        data() {
            return {
                brand
            }
        },
        components: {
            Header,
            GoogleAnalytics
        }
    }
</script>
<style src="~/static/css/master.css"></style>
<style src="~/static/css/components.css"></style>
