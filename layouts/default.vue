<!--<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
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
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="fixed = !fixed"
      >
        <v-icon>mdi-minus</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  }
}
</script>-->


<template>
  <v-app>
    <v-content>
      <!-- desktop -->
      <!-- <div class="d-none d-md-block"></div> -->
      <!-- mobile -->
      <div class="d-block">
        <v-container fluid class="barona-topbar">
          <v-row no-gutters align="center" justify="start">
            <v-col cols="1" class="flex-grow-0 flex-shrink-0">
              <v-icon @click="drawer = true">mdi-menu</v-icon>
            </v-col>
            <v-col
              cols="1"
              style="min-width: 100px; max-width: 100%"
              class="flex-grow-1 flex-shrink-0"
            >
              <v-row no-gutters align="center" justify="center">
                <nuxt-link to="/">
                  <v-row no-gutters align="center" justify="center">
                    <h3 class="logo">Barona OMS</h3>
                  </v-row>
                </nuxt-link>
              </v-row>
            </v-col>
          </v-row>
        </v-container>
        <v-navigation-drawer
          v-model="drawer"
          fixed
          bottom
          class="d-block grey lighten-4"
        >
          <v-container fluid class="barona-topbar">
            <v-row no-gutters align="center" justify="space-between">
              <v-icon @click="drawer = false">mdi-close</v-icon>

              <v-row no-gutters align="center" justify="end">
                <p class="barona-user">
                  Signed in as<br />
                  {{ $nuxt.$fire.auth.currentUser.email }}
                </p>
                &nbsp;
                <v-icon>mdi-account-circle-outline</v-icon>
              </v-row>
            </v-row>
          </v-container>
          <v-container class="d-block menu-items">
            <div class="d-flex flex-column">
              <div class="d-flex">
                <br />
              </div>
              <div class="d-flex">
                <nuxt-link to="/">
                  <v-icon>mdi-view-dashboard-variant-outline</v-icon
                  >&nbsp;&nbsp;<b>My Boards</b>
                </nuxt-link>
              </div>
              <div class="d-flex">
                <nuxt-link to="/auth/signout">
                  <v-icon>mdi-exit-to-app</v-icon>&nbsp;&nbsp;<b>Sign out</b>
                </nuxt-link>
              </div>
            </div>
          </v-container>
        </v-navigation-drawer>
      </div>
      <!-- content -->
      <nuxt />
    </v-content>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
    }
  },
}
</script>

<style lang="scss" scoped>
a {
  text-decoration: none;
}
.menu-items a {
  color: $text-color;
  padding: 10px 0px 10px 3px;
  font-size: 24px;
}
