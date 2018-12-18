<template>
  <div class="dashboard">
    <dashboard-info-widgets></dashboard-info-widgets>
    {{ post }}
    <vuestic-widget class="no-padding no-v-padding">
      <vuestic-tabs
        :names="[$t('dashboard.dataVisualization'), $t('dashboard.usersAndMembers'), $t('dashboard.setupProfile'), $t('dashboard.features')]"
        ref="tabs"
      >
        <div :slot="$t('dashboard.dataVisualization')">
          <data-visualisation-tab></data-visualisation-tab>
        </div>
        <div :slot="$t('dashboard.usersAndMembers')">
          <users-members-tab></users-members-tab>
        </div>
        <div :slot="$t('dashboard.setupProfile')">
          <setup-profile-tab></setup-profile-tab>
        </div>
        <div :slot="$t('dashboard.features')">
          <features-tab></features-tab>
        </div>
      </vuestic-tabs>
    </vuestic-widget>

    <dashboard-bottom-widgets></dashboard-bottom-widgets>
  </div>
</template>

<script>
import DashboardInfoWidgets from "./DashboardInfoWidgets";
import UsersMembersTab from "./users-and-members-tab/UsersMembersTab.vue";
import SetupProfileTab from "./setup-profile-tab/SetupProfileTab.vue";
import FeaturesTab from "./features-tab/FeaturesTab.vue";
import DataVisualisationTab from "./data-visualisation-tab/DataVisualisation.vue";
import DashboardBottomWidgets from "./DashboardBottomWidgets.vue";

import axios from "axios";

export default {
  name: "dashboard",
  components: {
    DataVisualisationTab,
    DashboardInfoWidgets,
    UsersMembersTab,
    SetupProfileTab,
    FeaturesTab,
    DashboardBottomWidgets
  },
  data() {
    return {
      post: '',
      token: localStorage.getItem('token')
    };
  },
  created() {
    var headers = {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer ' + this.token 
    }
    var self = this;
      axios.get('http://192.168.10.10/api/user', {headers:headers})
      .then(response => {
        console.log(self.token);
        console.log(response.data);

      })
  },

  methods: {
    launchEpicmaxToast() {
      this.showToast(`Let's work together!`, {
        icon: "fa-star-o",
        position: "top-right",
        duration: Infinity,
        action: {
          text: "Hire us",
          href: "http://epicmax.co/#/contact",
          class: "vuestic-toasted-link"
        }
      });
    }
  }
};
</script>
<style lang="scss" scoped>
</style>
