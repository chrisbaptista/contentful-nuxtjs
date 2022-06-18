<template>
  <v-container>
    <h1>{{ page.fields.title }}</h1>
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
        page: page,
        pageContent: documentToHtmlString(page.fields.pageContent)
      }
    }).catch(console.error)

  }
}
</script>
