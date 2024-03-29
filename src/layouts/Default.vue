<template>
  <v-app  app class="default-layout">
    <Sidebar
      :user="$user"
      class="sidebar"
      :xs="$vuetify.breakpoint.xs"
      @drawer="toggleDrawer($event)"
    >
      <SidebarNavList br :items="routes" class="sidebar__nav-list">
        <template #route="{ route }">
          <SidebarNavListItem
            exact
            :name="route.name"
            :icon="route.icon"
            :to="route.path"
            :key="route.name"
            :hasChildrens="route.childrens && route.childrens.length"
            @click="currentRoute = route.name"
          >
            <SidebarNavList :items="route.childrens">
              <template #route="{ route: child }">
                <SidebarNavListItem
                  exact
                  :name="child.name"
                  :icon="child.icon"
                  :to="(route.path || '') + child.path"
                  :key="child.name"
                />
              </template>
            </SidebarNavList>
          </SidebarNavListItem>
        </template>
      </SidebarNavList>
    </Sidebar>
    <AppBar
      title="Admin"
      :theme="$theme"
      :isAuth="$isAuth"
      :loading="$loading"
      @drawer="toggleDrawer"
    >
      <template #actions>
        <v-menu
          offset-y
          rounded="b"
          :close-on-content-click="false"
          transition="slide-y-transition"
        >
          <template v-slot:activator="{ attrs, on }">
            <v-avatar
              size="32"
              v-on="on"
              color="primary"
              v-bind="attrs"
              class="white--text"
            >
              A
            </v-avatar>
          </template>
          <v-card width="300">
            <v-list dense class="transparent">
              <v-list-item @click="onChangeTheme">
                <v-list-item-icon>
                  <v-icon v-if="$theme.dark">mdi-weather-night</v-icon>
                  <v-icon v-else>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title v-text="'Dark theme'" />
                <v-switch
                  inset
                  dense
                  hide-details
                  class="mt-0 no-events"
                  :input-value="$theme.dark"
                ></v-switch>
              </v-list-item>
              <v-list-item-group color="primary">
                <v-list-item :disabled="$loading" @click="handleLogout">
                  <v-list-item-icon>
                    <v-icon>mdi-location-exit</v-icon>
                  </v-list-item-icon>
                  <v-list-item-content>
                    <v-list-item-title v-text="'Signout'" />
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-card>
        </v-menu>
      </template>
    </AppBar>

    <!-- Main content / router -->
    <v-main
      class="default-layout__main grey"
      :class="$theme.dark ? 'darken-4' : 'lighten-3'"
    >
      <router-wrapper>
        <router-view class="" />
      </router-wrapper>
    </v-main>
  </v-app>
</template>

<script>
import { mapActions, mapGetters, mapMutations } from "vuex";

// Components
import AppBar from "@/components/basic/AppBar";
import Sidebar from "@/components/basic/Sidebar";
import SidebarNavList from "@/components/basic/SidebarNavList";
import SidebarNavListItem from "@/components/basic/SidebarNavListItem";
import RouterWrapper from "@/components/basic/RouterWrapper";

// import { formatDate } from "../helpers";

export default {
  name: "DefaultLayout",
  components: {
    AppBar,
    Sidebar,
    RouterWrapper,
    SidebarNavList,
    SidebarNavListItem,
  },
  data() {
    return {
      isLoaded: false,
      routes: [
        {
          path: "/",
          name: "Dashboard",
          icon: "mdi-home",
        },
        {
          path: "/users",
          name: "Users",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Users",
              icon: "mdi-account-group",
            },
          ],
        },
        {
          path: "/invoice",
          name: "Invoicing",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Invoicing Child",
              icon: "mdi-account-group",
            },
          ],
        },
        {
          path: "/invoice",
          name: "Invoicing",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Invoicing Child",
              icon: "mdi-account-group",
            },
          ],
        },
        {
          path: "/invoice",
          name: "Expenses",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Expenses Child",
              icon: "mdi-account-group",
            },
          ],
        },
        {
          path: "/invoice",
          name: "Cash Flow",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Cash Flow Child",
              icon: "mdi-account-group",
            },
          ],
        },
        {
          path: "/invoice",
          name: "Employees",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Invoicing Child",
              icon: "mdi-account-group",
            },
          ],
        },
        {
          path: "/invoice",
          name: "Reports",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Invoicing Child",
              icon: "mdi-account-group",
            },
          ],
        },
        {
          path: "/invoice",
          name: "Taxes",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Invoicing Child",
              icon: "mdi-account-group",
            },
          ],
        },
         {
          path: "/invoice",
          name: "Accounting",
          icon: "mdi-account-group",
          childrens: [
            {
              path: "",
              name: "Accounting Child",
              icon: "mdi-account-group",
            },
          ],
        }
        
      ],
    };
  },
  created() {
    this.onCreated();
  },
  computed: {
    ...mapGetters(["$drawer", "$loading", "$theme"]),
    ...mapGetters("AUTH", ["$isAuth", "$user"]),
    mDrawer: {
      get() {
        return this.$drawer;
      },
      set(v) {
        this.setState({ drawer: v });
      },
    },
  },
  methods: {
    ...mapMutations(["SET"]),
    ...mapActions("AUTH", ["logout", "fetchProfile"]),
    ...mapActions(["toggleDrawer", "toggleLoading", "toggleTheme"]),
    async onCreated() {
      // this.isLoaded = false;
      // this.toggleLoading(true);
      // await this.fetchProfile();
      // if (!this.$isAuth) {
      //   await this.fetchRooms();
      //   await this.fetchMeetings({ date: formatDate() });
      // }
      // this.isLoaded = true;
      // this.toggleLoading(false);
    },
    async handleLogout() {
      this.SET({ loading: true });
      await this.logout();
      this.$router.replace("/login");
      this.SET({ loading: false });
    },
    onChangeTheme() {
      const theme = !this.$theme.dark;
      this.$vuetify.theme.dark = theme;
      this.toggleTheme(theme);
    },
  },
};
</script>
<style lang='scss'>
.default-layout {
  &__main {
    padding-top: 72px !important;
  }
  &__scroll {
    max-height: calc(100vh - 72px);
    overflow-y: auto;
  }
  .sidebar {
    &__nav-list {
      height: calc(100vh - 96px);
    }
  }
}
</style>