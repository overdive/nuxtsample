<template>
    <div>
        <!-- render data of the person -->
        <h1>ニュース詳細</h1>
        <!-- render blog posts -->
        <h2>{{ currentNews.fields.title }}</h2>
        <div>
            <vue-markdown :source="currentNews.fields.body"></vue-markdown>
        </div>
    </div>
</template>

<script>
    import VueMarkdown from 'vue-markdown'
    import {createClient} from '~/plugins/contentful.js'

    const client = createClient()
    export default {
        data () {
            return {
                allNews: [],
                currentNews: []
            }
        },
        asyncData ({ env, params }) {
            return client.getEntries({
                'content_type': env.CTF_BLOG_POST_TYPE_ID,
                //order: '-fields.publishDate'
            }).then(entries => {
                const posts = entries.items
                const current = posts.filter(function (item) {
                    // debug
                    console.log(item.sys.id)
                    return item.sys.id === params.id
                })
                // debug
                console.log(current[0])
                return {
                    allNews: posts,
                    currentNews: current[0]
                };
            }).catch(console.error)
        },
        components: {
            VueMarkdown
        }
    }
</script>
