<template>
  <v-container>
    <h1>{{ pageTitle }}</h1>
    <div class="content" v-html="pageContent"></div>
  </v-container>
</template>

<script>
import {documentToHtmlString} from '@contentful/rich-text-html-renderer';
import {createClient} from '../plugins/contentful';

const contentfulClient = createClient();

export default {
  async asyncData({route, store, params, error}) {

    return contentfulClient.getEntries({
      'content_type': 'page',
      'fields.urlPath': route.path
    }).then(results => {
      if (results.items.length == 0) {
        error({statusCode: 404, message: 'Page Not Found'});
        return
      }

      const page = results.items[0];
      return {
        headers: {
          title: page.fields.title,
          meta: [
            {hid: "og-url", name: "og:url", content: page.fields.urlPath},
            {hid: "og-type", name: "og:type", content: "article"},
            {hid: "og-title", name: "og:title", content: page.fields.title},
            // {hid: "og-description", name: "og:description", content: ogDescription},
            // {hid: "og-image", name: "og:image", content: image},
          ]
        },
        pageTitle: page.fields.title,
        pageContent: documentToHtmlString(page.fields.pageContent)
      }
    }).catch(console.error)

  },
  head: function () {
    return {
      ...this.headers
    }
  }
}
</script>
