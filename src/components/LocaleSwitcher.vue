<template>
  <v-select
    prepend-icon="mdi-translate"
    v-model="$i18n.locale"
    :items="[
      { text: 'English', value: 'en' },
      { text: 'Español', value: 'es' },
    ]"
    @change="changeLocale()"
  >
  </v-select>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
export default {
  created() {
    this.initialLoad();
  },
  computed: {
    ...mapGetters(["getAllConfig"]),
  },
  methods: {
    ...mapActions(["updateConfig", "fetchAllConfig"]),
    async initialLoad() {
      await this.fetchAllConfig();
      var langText = await this.getAllConfig;
      var langObj = await JSON.parse(langText);
      this.$i18n.locale = langObj.lang;
      this.$vuetify.lang.current = langObj.lang;
    },
    async changeLocale() {
      var langText = await this.getAllConfig;
      var langObj = await JSON.parse(langText);
      langObj.lang = this.$i18n.locale;
      this.$vuetify.lang.current = this.$i18n.locale;
      langText = await JSON.stringify(langObj);
      await this.updateConfig(langText);
      await this.fetchAllConfig();
      langText = await this.getAllConfig;
    },
  },
};
</script>

<style></style>
