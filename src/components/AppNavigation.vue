<template>
  <v-card
    flat
    tile
    v-bind="$attrs"
  >
    <v-card-text class="pa-0">
      <v-list>
        <v-list-item v-if="isMobile">
          <v-text-field
            v-model="textSearchQuery"
            autocomplete="off"
            :label="`Search`"
            rounded
            small
            dense
            hide-details
            outlined
            @keydown.enter="textSearch"
          >
            <template v-slot:append>
              <v-btn
                small
                dense
                icon
              >
                <v-icon>mdi-magnify</v-icon>
              </v-btn>
            </template>
          </v-text-field>
        </v-list-item>
        <v-list-item v-if="!isLoggedIn">
          <ToolTip
            text="Login"
            :condition="isMDPI"
          >
            <template v-slot:activator="{ show, hide }">
              <v-btn
                class="text-h6 justify-start"
                block
                text
                @mouseover="show"
                @mouseleave="hide"
                @click="openDialog('login')"
              >
                <v-icon>mdi-login</v-icon>
                <span
                  v-show="!iconOnly"
                  class="ml-5"
                >Login</span>
              </v-btn>
            </template>
          </ToolTip>
        </v-list-item>
        <v-list-item>
          <ToolTip
            text="Home"
            :condition="isMDPI"
          >
            <template v-slot:activator="{ show, hide }">
              <v-btn
                :to="'/'"
                class="text-h6 justify-start"
                block
                text
                @mouseover="show"
                @mouseleave="hide"
              >
                <v-icon>mdi-home</v-icon>
                <span
                  v-show="!iconOnly"
                  class="ml-5"
                >Home</span>
              </v-btn>
            </template>
          </ToolTip>
        </v-list-item>
        <v-list-item v-if="isLoggedIn && isMobile">
          <ToolTip
            text="Notifications"
            :condition="isMDPI"
          >
            <template v-slot:activator="{ show, hide }">
              <v-btn
                class="text-h6 justify-start"
                block
                text
                :to="{ name: 'notifications' }"
                @mouseover="show"
                @mouseleave="hide"
              >
                <v-badge
                  :value="notificationCount > 0"
                  bordered
                  color="primary"
                  :content="notificationCount"
                  overlap
                >
                  <v-icon>mdi-bell</v-icon>
                </v-badge>
                <span
                  v-show="!iconOnly"
                  class="ml-5"
                >Notifications</span>
              </v-btn>
            </template>
          </ToolTip>
        </v-list-item>
        <v-list-item v-if="isLoggedIn && isMobile">
          <ToolTip
            text="Messenger"
            :condition="isMDPI"
          >
            <template v-slot:activator="{ show, hide }">
              <v-btn
                class="text-h6 justify-start"
                block
                text
                :to="{ name: 'messenger' }"
                @mouseover="show"
                @mouseleave="hide"
              >
                <v-badge
                  :value="hasUnreadMessages"
                  bordered
                  color="primary"
                  :content="' '"
                  overlap
                >
                  <v-icon>mdi-message</v-icon>
                </v-badge>
                <span
                  v-show="!iconOnly"
                  class="ml-5"
                >Messages</span>
              </v-btn>
            </template>
          </ToolTip>
        </v-list-item>
        <v-list-item v-if="isLoggedIn">
          <ToolTip
            text="Profile"
            :condition="isMDPI"
          >
            <template v-slot:activator="{ show, hide }">
              <v-btn
                class="text-h6 justify-start"
                block
                text
                :to="{
                  name: 'profile',
                  params: { username: account.username },
                }"
                @mouseover="show"
                @mouseleave="hide"
              >
                <v-icon>mdi-account</v-icon>
                <span
                  v-show="!iconOnly"
                  class="ml-5"
                >Profile</span>
              </v-btn>
            </template>
          </ToolTip>
        </v-list-item>
        <v-list-item v-if="isLoggedIn">
          <ToolTip
            text="Wallet"
            :condition="isMDPI"
          >
            <template v-slot:activator="{ show, hide }">
              <v-btn
                class="text-h6 justify-start"
                block
                text
                :to="{ name: 'wallet' }"
                @mouseover="show"
                @mouseleave="hide"
              >
                <v-icon>mdi-wallet</v-icon>
                <span
                  v-show="!iconOnly"
                  class="ml-5"
                >Wallet</span>
              </v-btn>
            </template>
          </ToolTip>
        </v-list-item>
        <v-list-item v-if="isLoggedIn">
          <ToolTip
            text="Logout"
            :condition="isMDPI"
          >
            <template v-slot:activator="{ show, hide }">
              <v-btn
                text
                class="text-h6 justify-start"
                @mouseover="show"
                @mouseleave="hide"
                @click="logout"
              >
                <v-icon>mdi-logout</v-icon>
                <span
                  v-show="!iconOnly"
                  class="ml-5"
                >Logout</span>
              </v-btn>
            </template>
          </ToolTip>
        </v-list-item>
        <v-list-item v-if="isLoggedIn">
          <v-menu offset-y>
            <template v-slot:activator="{ on, attrs }">
              <ToolTip
                text="More"
                :condition="isMDPI"
              >
                <template v-slot:activator="{ show, hide }">
                  <v-btn
                    class="text-h6 justify-start"
                    block
                    text
                    v-bind="attrs"
                    @mouseover="show"
                    @mouseleave="hide"
                    v-on="on"
                  >
                    <v-icon>mdi-dots-horizontal-circle-outline</v-icon>
                    <span
                      v-show="!iconOnly"
                      class="ml-5"
                    >More</span>
                  </v-btn>
                </template>
              </ToolTip>
            </template>
            <v-list>
              <v-list-item v-if="isLoggedIn">
                <v-btn
                  text
                  block
                  :to="{ name: 'settings' }"
                >
                  <v-icon class="mr-2">
                    mdi-cog
                  </v-icon>
                  <span>Settings</span>
                </v-btn>
              </v-list-item>
              <v-list-item v-if="isLoggedIn">
                <v-btn
                  text
                  block
                  :to="{ name: 'moderation' }"
                >
                  <v-icon class="mr-2">
                    mdi-human-edit
                  </v-icon>
                  <span>Moderation</span>
                </v-btn>
              </v-list-item>
              <v-list-item v-if="isLoggedIn">
                <v-btn
                  text
                  block
                  :to="{ name: 'community' }"
                >
                  <v-icon class="mr-2">
                    mdi-account-group
                  </v-icon>
                  <span>Community</span>
                </v-btn>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-list-item>
        <v-list-item
          v-if="isLoggedIn"
          class="mt-4"
        >
          <v-btn
            rounded
            color="primary"
            block
            @click="openDialog('submitpost')"
          >
            <v-icon>mdi-pencil</v-icon>
            <span v-if="!isMobile && !isMDPI">Post</span>
          </v-btn>
        </v-list-item>
      </v-list>
    </v-card-text>
  </v-card>
</template>

<script>
import { mapState } from "vuex";
import mixins from "../mixins";

import ToolTip from "./ToolTip";

export default {
  name: "AppNavigation",
  components: {
    ToolTip,
  },
  mixins: [mixins.Common, mixins.Login],
  props: {
    iconOnly: { type: Boolean, default: false },
  },
  data: () => ({
    textSearchQuery: "",
  }),
  computed: {
    ...mapState(["notificationCount", "hasUnreadMessages"]),
  },
  watch: {},
  async created() {},
  async mounted() {},
  async beforeDestroy() {},
  methods: {
    textSearch() {
      this.$router.push({
        name: "search",
        query: { q: this.textSearchQuery ?? "" },
      });
    },
    async test(sender) {
      sender;
      //console.log('mouseover');
      //this.openPopover('profile', { $el: sender.srcElement });
    },
  },
};
</script>