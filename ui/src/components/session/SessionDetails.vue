<template>
<fragment>

  <div class="d-flex pa-0 align-center">
    <h1>Session Details</h1>
    <v-spacer/>
    <v-spacer/>
  </div>

  <v-card class="mt-2">
    <v-toolbar flat color="transparent">

      <v-toolbar-title v-if="this.session.device">
        <v-icon color="success" v-if="this.session.active">check_circle</v-icon>
        <v-tooltip bottom v-else>
          <template #activator="{ on }">
            <v-icon v-on="on">check_circle</v-icon>
          </template>
          <span>active {{ session.last_seen | moment("from", "now") }}</span>
        </v-tooltip>
        {{ this.session.device.name }}
      </v-toolbar-title>

    </v-toolbar>
        
    <v-divider></v-divider>

    <v-card-text>
      <div class="mt-2">
        <div class="overline">Uid</div>
        <div>{{ this.session.uid }}</div>
      </div>

      <div class="mt-2">
        <div class="overline">User</div>
        <div>{{ this.session.username }}</div>
      </div>

      <div class="mt-2"> 
        <div class="overline">Ip Address</div>
        <code>{{ this.session.ip_address }}</code>
      </div>

      <div class="mt-2">
        <div class="overline">Started</div>
        <div>{{ this.session.started_at | moment("dddd, MMMM Do YYYY, h:mm:ss a") }}</div>
      </div>

      <div class="mt-2">
        <div class="overline">Last Seen</div>
        <div>{{ this.session.last_seen | moment("dddd, MMMM Do YYYY, h:mm:ss a") }}</div>
      </div>
    </v-card-text>

    <v-snackbar v-model="copySnack" :timeout=3000>Device SSHID copied to clipboard</v-snackbar>
  </v-card>

</fragment>
</template>

<script>

export default {
  name: "SessionDetails",

  data() {
    return {
      uid: '',
      session: [],
      copySnack: false
    };
  },

  async created() {
    this.uid = this.$route.params.id
    await this.$store.dispatch("sessions/get", this.uid);
    this.session = this.$store.getters["sessions/get"];
  },

};
</script>