<template>
  <div>
    <header class="tag-page header">
      <div class="foreground">
        <div class="page-bar wrapper">
          <a href="/" class="person-name">Результаты по тэгу</a>
          <Navigation></Navigation>
        </div>
        <div class="page-info wrapper">
          <h2>#{{ tag }}</h2>
        </div>
      </div>
    </header>

    <section class="body-container">
      <div class="items-bar wrapper">
        <h2>Все артикли с тегом #{{ tag }} ({{ posts.length }})</h2>
      </div>
      <ul class="items-list wrapper">
        <li class="item" v-for="post in posts">
          <event-preview :post="post"></event-preview>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import {createClient} from '~/plugins/contentful.js'
import Navigation from '~/components/navigation.vue'
import ArticlePreview from '~/components/article-preview.vue'
import EventPreview from '~/components/event-preview.vue'

const client = createClient()

export default {
  asyncData ({ env, params }) {
    return client.getEntries({
      'content_type': env.CTF_EVENTS_TYPE_ID,
      'fields.tags[in]': params.tag,
      order: '-sys.createdAt'
    }).then(entries => {
      return {
        posts: entries.items,
        tag: params.tag
      }
    })
  },
  components: {
    ArticlePreview,
    Navigation,
    EventPreview
  }
}
</script>
