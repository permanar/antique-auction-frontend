<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" temporary fixed app>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-title> {{ item.title }} </v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon
        @click.stop="drawer = !drawer"
        class="hidden-sm-and-up "
      />
      <v-toolbar-side-icon
        @click.stop="drawer = !drawer"
        class="hidden-sm-and-up "
      ></v-toolbar-side-icon>
      <v-toolbar-title>
        <router-link to="/" tag="span" style="cursor: pointer">
          {{ title }}
        </router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn dense text v-for="(item, i) in items" :key="i" :to="item.to">
          {{ item.title }}
        </v-btn>
      </v-toolbar-items>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <small
        >&copy; {{ new Date().getFullYear() }} Antique Auction. All Right
        Reserved.</small
      >
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Home",
          to: "/"
        },
        {
          icon: "mdi-chart-bubble",
          title: "Auctions",
          to: "/auctions"
        },
        {
          icon: "mdi-lock",
          title: "Login",
          to: "/login"
        }
      ],
      title: "Antique Auction"
    };
  }
};
</script>
