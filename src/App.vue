<script setup lang="ts">
  import { useI18n } from "vue-i18n";
  import router from "src/router";
  // import { useUsersStore } from "./store/usersStore";

  const leftDrawer = ref<boolean>(true);
  // const usersStore = useUsersStore();
  // const loggedUser = computed(() => usersStore.getLoggedUser);
  // const notLoggedIn = computed(() => usersStore.getLoggedUser == null);

  let { locale, t } = useI18n({
    inheritLocale: true,
    useScope: "global", // Change to "local" if you want to add <i18n></i18n> locally
  });

  const menuItems = ref([
    {
      icon: "mdi-home",
      text: t("startPage"),
      name: "startPage",
      route: "/",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-soccer",
      text: t("examples"),
      name: "examples",
      route: "/examples",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-cart-outline",
      text: "VM-Socks",
      name: "",
      route: "/socks",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-earth",
      text: "VM-Real World Vue3",
      name: "",
      route: "/realworldvue3",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-state-machine",
      text: "VM-Vuex fundamentals",
      name: "",
      route: "/vuexfundamentals",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-grid",
      text: t("gridDemo"),
      name: "gridDemo",
      route: "/grid",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-account",
      text: t("account"),
      name: "account",
      route: "/account",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-information",
      text: t("about"),
      name: "about",
      route: "/about",
      disabled: false,
      separator: true,
    },
  ]);

  function toggleLanguage() {
    locale.value = locale.value == "hu" ? "en" : "hu";
    menuItems.value.forEach((e) => {
      if (e.name != "") e.text = t(e.name);
    });
  }

  const links = ref([
    {
      icon: "mdi-github",
      text: "GitHub repo",
      name: "",
      link: "https://github.com/nitslaszlo/jedlik-vite-quasar-template",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-tire",
      text: "Quasar",
      name: "",
      link: "https://quasar.dev/",
      disabled: false,
      separator: false,
    },
    {
      icon: "mdi-fruit-pineapple",
      text: "Pinia",
      name: "",
      link: "https://pinia.vuejs.org/introduction.html",
      disabled: false,
      separator: false,
    },
  ]);
</script>

<template>
  <div class="q-pa-md">
    <q-layout view="hHh Lpr fFf">
      <q-header class="bg-primary text-white text-left" elevated>
        <q-toolbar>
          <q-btn dense flat icon="mdi-menu" round @click="leftDrawer = !leftDrawer" />
          <q-toolbar-title id="title" style="cursor: pointer" @click="router.push({ path: '/' })">
            <q-avatar>
              <img src="src/assets/Jedlik_small.png" />
            </q-avatar>
            Jedlik Vite-Quasar Template 2022
          </q-toolbar-title>
          <q-btn flat icon="mdi-comment-text-multiple" @click="toggleLanguage">
            <q-badge color="red" floating>{{ locale }}</q-badge>
          </q-btn>
          <q-btn flat icon="mdi-theme-light-dark" @click="$q.dark.toggle" />
          <q-btn dense flat icon="mdi-menu" round @click="leftDrawer = !leftDrawer" />
        </q-toolbar>
      </q-header>

      <q-drawer
        v-model="leftDrawer"
        bordered
        :breakpoint="500"
        :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-3'"
        show-if-above
        :width="200"
      >
        <q-scroll-area class="fit">
          <!-- routes: -->
          <q-list>
            <template v-for="(menuItem, index) in menuItems" :key="index">
              <q-item clickable :to="menuItem.route">
                <q-item-section avatar>
                  <q-icon :name="menuItem.icon" />
                </q-item-section>
                <q-item-section>
                  {{ menuItem.text }}
                </q-item-section>
              </q-item>
              <q-separator v-if="menuItem.separator" :key="'sep' + index" />
            </template>
          </q-list>
          <!-- links: -->
          <q-list>
            <template v-for="(linkItem, index) in links" :key="index">
              <q-item clickable :href="linkItem.link">
                <q-item-section avatar>
                  <q-icon :name="linkItem.icon" />
                </q-item-section>
                <q-item-section>
                  {{ linkItem.text }}
                </q-item-section>
              </q-item>
              <q-separator v-if="linkItem.separator" :key="'sep' + index" />
            </template>
          </q-list>
        </q-scroll-area>
      </q-drawer>

      <q-page-container id="container">
        <router-view v-slot="{ Component }">
          <transition mode="out-in" name="slide-fade">
            <component :is="Component" />
          </transition>
        </router-view>
      </q-page-container>
    </q-layout>
  </div>
</template>
