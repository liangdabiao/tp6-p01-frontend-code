<template>
  <v-app>
    <v-container style="min-height: 100vh">
      <v-row class="fill-height">
        <v-col cols="2">
          <v-navigation-drawer permanent>
            <div class="d-flex flex-column justify-space-between fill-height">
              <v-list dense rounded nav>
                <v-list-item
                  v-for="item in navItems"
                  :key="item.title"
                  link
                  :to="item.to"
                  color="primary"
                >
                  <v-list-item-icon>
                    <v-icon>{{ item.icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ item.title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>

              <v-list v-if="!$store.state.user.isLogin" dense rounded nav>
                <v-list-item
                  v-for="item in navUserItems"
                  :key="item.title"
                  link
                  :to="item.to"
                  color="primary"
                >
                  <v-list-item-icon>
                    <v-icon>{{ item.icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ item.title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>

              <v-list v-else dense rounded nav>
                <v-list-item link color="primary">
                  <v-menu
                    :close-on-content-click="false"
                    :nudge-width="200"
                    offset-x
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-list-item-icon>
                        <v-icon>mdi-account</v-icon>
                      </v-list-item-icon>

                      <v-list-item-content v-bind="attrs" v-on="on">
                        <v-list-item-title>
                          {{ $store.state.user.data.email }}
                          <v-icon right>mdi-dots-horizontal</v-icon>
                        </v-list-item-title>
                      </v-list-item-content>
                    </template>

                    <v-card>
                      <v-list>
                        <v-list-item>
                          <v-list-item-avatar>
                            <v-icon>mdi-account</v-icon>
                          </v-list-item-avatar>

                          <v-list-item-content>
                            <v-list-item-title>
                              {{ $store.state.user.data.email }}
                            </v-list-item-title>
                            <v-list-item-subtitle
                              >网站用户</v-list-item-subtitle
                            >
                          </v-list-item-content>

                          <v-list-item-action>
                            <v-btn icon @click="$api.logout()">
                              <v-icon color="error">mdi-logout</v-icon>
                            </v-btn>
                          </v-list-item-action>
                        </v-list-item>
                      </v-list>
                    </v-card>
                  </v-menu>
                </v-list-item>
              </v-list>
            </div>
          </v-navigation-drawer>
        </v-col>

        <v-col cols="8">
          <v-main>
            <router-view :key="$route.fullPath"></router-view>
          </v-main>
        </v-col>
      </v-row>
    </v-container>

    <Snackbar />
  </v-app>
</template>

<script>
import Snackbar from "./components/_partial/Snackbar";

export default {
  components: {
    Snackbar
  },
  data: () => ({
    navUserItems: [
      { title: "注册", icon: "mdi-account-plus", to: "/sign" },
      { title: "登录", icon: "mdi-login", to: "/login" }
    ]
  }),
  computed: {
    navItems() {
      const data = [{ title: "主页", icon: "mdi-home", to: "/" }];
      if (!this.$store.state.user.isLogin) {
        return data;
      }

      const user = {
        title: "个人资料",
        icon: "mdi-account",
        to: this.$store.state.user.data.id
      };

      return [...data, user];
    }
  },
  mounted() {
    this.$api.me();
  },
  methods: {}
};
</script>
