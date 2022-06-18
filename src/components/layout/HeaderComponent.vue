<template>
  <div>
    <v-app-bar app
               elevate-on-scroll>
      <v-container class="pa-0 fill-height">
        <span class="hidden-sm-and-up">
          <v-app-bar-nav-icon @click.stop="sidebar = !sidebar"></v-app-bar-nav-icon>
        </span>

        <v-toolbar-title>
          <NuxtLink to="/">Sample Site</NuxtLink>
        </v-toolbar-title>

        <v-spacer></v-spacer>

        <div class="hidden-xs-only">
          <NuxtLink v-for="item in menuItems"
                    width="auto"
                    :key="item.fields.title"
                    :to="item.fields.urlPath">
            {{ item.fields.title }}
          </NuxtLink>
        </div>
      </v-container>
    </v-app-bar>

    <v-navigation-drawer
        app
        v-model="sidebar"
        absolute
        temporary
    >
      <v-list>
        <v-list-item to="/">
          Home
        </v-list-item>
        <v-list-item
            v-for="item in menuItems"
            :key="item.fields.title"
            :to="item.fields.urlPath">
          {{ item.fields.title }}
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </div>
</template>


<script>
import {createClient} from '../../plugins/contentful';

const contentfulClient = createClient();

export default {
  name: 'header_component',

  data() {
    return {
      sidebar: false,
      menuItems: []
    }
  },

  async fetch() {
    return contentfulClient.getEntries({
      'content_type': 'navigation',
      'fields.slug': 'navigation-header'
    }).then(results => {
      if (results.items.length > 0) {
        this.menuItems = results.items[0].fields.links;
      }
    }).catch(console.error)
  }
}
</script>