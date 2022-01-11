<template>
  <v-app>

    <!-- Navbar Mobile Side Menu -->

    <v-navigation-drawer
        v-model="drawer"
        v-if="loggedIn && role !== 3"
        class="d-flex d-lg-none"
        fixed
    >
      <v-list class="pa-4">
        <v-list-item class="pt-12">
          <v-avatar height="60px" width="60px" v-if="profilePic">
            <v-img :src="profilePic">
              <template v-slot:placeholder v-if="!profilePic">
                <v-row
                    class="fill-height ma-0"
                >
                  <v-progress-circular
                      indeterminate
                      color="grey"
                  ></v-progress-circular>
                </v-row>
              </template>
            </v-img>
          </v-avatar>
        </v-list-item>

        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>
              <h3>{{ user.firstname }} {{ user.lastname }}</h3>
            </v-list-item-title>
            <v-list-item-subtitle>
              {{ user.email }}
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>


      <!-- Navbar Mobile items User -->

      <v-list
          class="pa-8"
          v-if="role === 1"
          dense
          nav
      >

        <v-list-item

            active-class="primary--text text--accent-4"
            v-for="item in itemsUser"
            :key="item.title"
            :to="item.to"
            link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <!-- Navbar Mobile items Vendor -->

      <v-list
          class="pa-8"
          v-if="role === 2"
          dense
          nav
      >
        <v-list-item

            active-class="primary--text text--accent-4"
            v-for="item in itemsVendor"
            :key="item.title"
            :to="item.to"
            link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!-- Navbar Desktop for User -->

    <v-card v-if="role === 1">
      <v-app-bar
          app
          v-if="loggedIn"
          color="white"
          elevate-on-scroll
      >
        <v-app-bar-nav-icon
            @click="drawer = !drawer" color="primary"
            class="d-flex d-lg-none">
        </v-app-bar-nav-icon>


        <v-img
            class="mx-2"
            src="./assets/general/logo.png"
            max-height="30"
            max-width="30"
            contain
            @click="$router.push('/')"
        ></v-img>

        <v-toolbar-title @click="$router.push('/')">WedPlan</v-toolbar-title>


        <v-toolbar-items class="pl-lg-16 d-none d-lg-flex">
          <v-btn @click="tab = !tab" text depressed>
            Deine Hochzeit
          </v-btn>
          <v-btn to="/search" text @click="tab = false">Branchen</v-btn>
          <v-btn to="/blog" text @click="tab = false">Blog</v-btn>
          <v-btn to="/invites" text @click="tab = false">Einladungen</v-btn>
        </v-toolbar-items>

        <v-spacer></v-spacer>

        <div class="pr-2"  v-if="this.unreadCount > 0">
          <v-badge
              :content="this.unreadCount"
              color="tertiary"
              left
              overlap
          >
            <v-btn
                icon
                to="/messages"

            >
              <v-icon color="primary">mdi-email</v-icon>
            </v-btn>
          </v-badge>
        </div>


        <div class="pr-2" v-else>
          <v-btn
              icon
              to="/messages"
          >
            <v-icon color="primary">mdi-email</v-icon>
          </v-btn>
        </div>


        <div class="pr-5">
          <v-btn color="white"
                 small
                 depressed
                 icon
                 to="/account"
          >
            <v-avatar size="40px">
              <v-img :src="profilePic">
                <template v-slot:placeholder v-if="!profilePic">
                  <v-row
                      class="fill-height ma-0"
                      align="center"
                      justify="center"
                  >
                    <v-progress-circular
                        indeterminate
                        color="primary"
                    ></v-progress-circular>
                  </v-row>
                </template>
              </v-img>
            </v-avatar>
          </v-btn>
        </div>

        <div>
          <v-btn depressed color="quinary" @click="signedOut">
            <span class="d-none d-lg-flex">Abmelden</span>
            <v-icon color="primary">mdi-logout</v-icon>
          </v-btn>
        </div>


        <template v-slot:extension v-if="tab">
          <v-tabs
              background-color="quinary"
              align-with-title
          >
            <v-tabs-slider color="primary"></v-tabs-slider>

            <v-tab
                v-for="(item, index) in userLinks"
                :key="index"
                :to="item.to"
            >
              {{ item.title }}
            </v-tab>
          </v-tabs>
        </template>
      </v-app-bar>
    </v-card>

    <!-- Navbar Desktop for Vendor -->

    <v-card v-if="role === 2">
      <v-app-bar
          app
          v-if="loggedIn"
          color="white"
          elevate-on-scroll
      >
        <v-app-bar-nav-icon
            @click="drawer = !drawer" color="primary"
            class="d-flex d-lg-none">
        </v-app-bar-nav-icon>

        <v-img
            class="mx-2"
            src="./assets/general/logo.png"
            max-height="30"
            max-width="30"
            contain
            @click="$router.push('/')"
        ></v-img>

        <v-toolbar-title @click="$router.push('/')">WedPlan</v-toolbar-title>

        <v-toolbar-items class="pl-lg-16 d-none d-lg-flex">
          <v-btn text to="/vendor">Dashboard</v-btn>
          <v-btn text :to="'/vendor/' + user.id">Deine Seite</v-btn>
          <v-btn text to="/account">Account</v-btn>
          <v-btn text to="/messages">Nachrichten</v-btn>
        </v-toolbar-items>

        <v-spacer></v-spacer>

        <div class="pr-2" v-if="this.unreadCount > 0">
          <v-badge
              :content="this.unreadCount"
              color="tertiary"
              left
              overlap

          >
            <v-btn
                icon
                to="/messages"
            >
              <v-icon color="primary">mdi-email</v-icon>
            </v-btn>
          </v-badge>
        </div>

        <div class="pr-2" v-else>
          <v-btn
              icon
              to="/messages"
          >
            <v-icon color="primary">mdi-email</v-icon>
          </v-btn>
        </div>


        <div class="pr-5">
          <v-btn color="white"
                 small
                 depressed
                 icon
                 to="/account"
          >
            <v-avatar size="40px">
              <v-img :src="profilePic">
                <template v-slot:placeholder v-if="!profilePic">
                  <v-row
                      class="fill-height ma-0"
                      align="center"
                      justify="center"
                  >
                    <v-progress-circular
                        indeterminate
                        color="grey"
                    ></v-progress-circular>
                  </v-row>
                </template>
              </v-img>
            </v-avatar>
          </v-btn>
        </div>

        <div>
          <v-btn depressed @click="signedOut" color="quinary">
            <span class="d-none d-lg-flex">Abmelden</span>
            <v-icon color="primary">mdi-logout</v-icon>
          </v-btn>
        </div>

      </v-app-bar>
    </v-card>

    <v-card v-if="role === 3">
      <v-app-bar
          app
          v-if="loggedIn"
          color="white"
          elevate-on-scroll
      >
        <v-img
            class="mx-2"
            src="./assets/general/logo.png"
            max-height="30"
            max-width="30"
            contain
            @click="$router.push('/')"
        ></v-img>

        <v-toolbar-title @click="$router.push('/')">WedPlan</v-toolbar-title>


        <v-spacer></v-spacer>

        <div>
          <v-btn depressed @click="signedOut" color="quinary">
            <span class="d-none d-lg-flex">Abmelden</span>
            <v-icon color="primary">mdi-logout</v-icon>
          </v-btn>
        </div>

      </v-app-bar>
    </v-card>


    <!-- Navbar for not logged in Users (Home) Mobile -->

    <v-navigation-drawer
        v-model="drawer"
        v-if="!loggedIn && !($route.path === '/login' || $route.path === '/register')"
        class="d-flex d-sm-none"
        fixed
    >
      <v-list v-for="(item, index) in items" :key="index">
        <v-list-item>
          <v-btn text @click="$vuetify.goTo(item.link); drawer = !drawer">
            {{ item.title }}
          </v-btn>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <div class="pa-8 pb-2">
          <v-btn
              to="/register"
              rounded
              depressed
              color="primary"
              width="50vw">
            Registrieren
            <v-icon class="pl-2">mdi-account-plus</v-icon>
          </v-btn>
        </div>
        <div class="pa-8 pt-0">
          <v-btn
              to="/login"
              rounded
              depressed
              color="primary"
              width="50vw">
            Anmelden
            <v-icon class="pl-2">mdi-account</v-icon>
          </v-btn>
        </div>
      </template>

    </v-navigation-drawer>

    <!-- Navbar for not logged in Users (Home) Mobile -->

    <v-app-bar
        v-if="!loggedIn && !($route.path === '/login' || $route.path === '/register')"
        app
        color="white"
        elevate-on-scroll
    >
      <v-app-bar-nav-icon
          @click="drawer = !drawer"
          class="d-flex d-sm-none"
          color="primary">
      </v-app-bar-nav-icon>

      <v-img
          class="mx-2"
          src="./assets/general/logo.png"
          max-height="30"
          max-width="30"
          contain
      ></v-img>

      <v-toolbar-title>WedPlan</v-toolbar-title>

      <v-toolbar-items class="pl-lg-16 d-none d-sm-flex">
        <v-list v-for="(item, index) in items" :key="index">
          <v-list-item>
            <v-btn text @click="$vuetify.goTo(item.link)">
              {{ item.title }}
            </v-btn>
          </v-list-item>
        </v-list>

      </v-toolbar-items>

      <v-spacer></v-spacer>

      <div class="pr-lg-5 pr-1">
        <v-btn
            to="/register"
            rounded
            depressed
            color="primary">
          <span class="d-none d-sm-flex d-lg-flex">Registrieren</span>
          <v-icon class="pl-lg-2">mdi-account-plus</v-icon>
        </v-btn>
      </div>

      <div>
        <v-btn
            to="/login"
            rounded
            depressed
            color="primary">
          <span class="d-none d-sm-flex d-lg-flex">Anmelden</span>
          <v-icon>mdi-account</v-icon>
        </v-btn>
      </div>


    </v-app-bar>


    <v-main>
      <router-view/>
    </v-main>

    <v-footer
        v-if="!($route.path === '/login' || $route.path === '/register')"
        color="secondary"
        class="pt-6"
    >

      <v-row
          justify="center"
          no-gutters
      >
        <v-btn
            v-for="link in links"
            :key="link.id"
            :to="link.to"
            text
            rounded
        >
          {{ link.title }}
        </v-btn>
        <v-col class="secondary text-center"
               cols="12 pt-2">
          <p>Besuche unsere Social Media Kanäle</p>
        </v-col>
        <v-btn
            v-for="social in socialMedia"
            :key="social"
            color="tertiary darken-1"
            text
            rounded
        >
          <v-icon>
            {{ social }}
          </v-icon>

        </v-btn>
        <v-col
            class="secondary py-4 text-center"
            cols="12"
        >
          {{ new Date().getFullYear() }} — WEDPLAN
        </v-col>

      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
import {mapGetters} from "vuex";

export default {
  data() {
    return {
      tab: false,
      drawer: false,
      userLinks: [
        {title: 'Dashboard', icon: 'mdi-home', to: '/dashboard'},
        {title: 'Dein Account', icon: 'mdi-account', to: '/account'},
        {title: 'Dein Budget', icon: 'mdi-chart-pie', to: '/budget'},
        {title: 'Deine Todo-Liste', icon: 'mdi-format-list-checks', to: '/todo'},
        {title: 'Deine Checkliste', icon: 'mdi-playlist-check', to: '/checklist'},
        {title: 'Deine Einladungen', icon: 'mdi-email-open', to: '/invites'},
        {title: 'Deine Nachrichten', icon: 'mdi-email', to: '/messages'},
      ],
      items: [
        {title: "Vorteile", link: '#advantages'},
        {title: "Angebot", link: '#offer'},
        {title: "Dienstleister", link: '#ourVendors'},
        {title: "So geht's", link: '#howTo'}
      ],
      itemsUser: [
        {title: 'Dashboard', icon: 'mdi-home', to: '/dashboard'},
        {title: 'Dein Account', icon: 'mdi-account', to: '/account'},
        {title: 'Dein Budget', icon: 'mdi-chart-pie', to: '/budget'},
        {title: 'Deine Todo-Liste', icon: 'mdi-format-list-checks', to: '/todo'},
        {title: 'Deine Checkliste', icon: 'mdi-playlist-check', to: '/checklist'},
        {title: 'Deine Einladungen', icon: 'mdi-email-open', to: '/invites'},
        {title: 'Deine Nachrichten', icon: 'mdi-email', to: '/messages'},
        {title: 'Branchen', icon: 'mdi-magnify', to: '/search'},
        {title: 'Blog', icon: 'mdi-post-outline', to: '/blog'},
      ],
      links: [
        {title: 'Kontakt', to: '/about'},
        {title: 'Datenschutz', to: '/dataprotection'},
        {title: 'Impressum', to: '/imprint'},
      ],
      socialMedia: [
        'mdi-instagram', 'mdi-facebook', 'mdi-twitter', 'mdi-youtube'
      ]
    }
  },
  computed: {
    ...mapGetters('login', {
      role: 'role',
      loggedIn: 'loggedIn',
      profilePic: 'profilePic',
      user: 'user'
    }),
    itemsVendor() {
      return [
        {title: 'Dashboard', icon: 'mdi-home', to: '/vendor'},
        {title: 'Deine Seite', icon: 'mdi-book-open-page-variant-outline', to: '/vendor/' + this.user.id},
        {title: 'Dein Account', icon: 'mdi-account', to: '/account'},
        {title: 'Deine Nachrichten', icon: 'mdi-email', to: '/messages'}
      ]
    },
    ...mapGetters('messages', {
      unreadCount: 'unreadCount'
    }),

  },
  mounted() {
    this.$store.dispatch('login/checkLogin', {}).then((user) => {
      console.log('User eingeloggt!')
      this.$store.dispatch('messages/getUnreadCount', user.uid)
    }).catch((e) => {
      console.log(e)
      console.log('User noch nicht eingeloggt')
    })
  },
  methods: {
    signedOut() {
      this.$store.dispatch('login/signedOut', {}).then(() => {
        if (this.$route.name === 'Home') {
          console.log("Already in Home")
        } else {
          this.$router.push({name: "Home"});
        }
      }).catch((e) => {
        console.log(e)
      })
    },
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@200&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Parisienne&family=Poppins:wght@200&display=swap');

#app {
  font-family: Poppins, Poppins sans-serif;
  color: #3a3a3a;
}

.secondFont {
  font-family: Parisienne, Parisienne, sans-serif;
}

.v-toolbar__title {
  font-family: Parisienne, Parisienne, sans-serif;
  color: #BDB5B3;
  font-size: 1.5rem !important;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

</style>
