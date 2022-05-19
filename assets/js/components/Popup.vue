<template>
  <div class="wrapper">
    <h1 class="title">Comed Konfigurasyon Yönetimi</h1>
    <div class="buttons">
      <button type="button" class="state-off is-active" :class="{'is-active': !active}" @click="setActive(false)">Off
      </button>
      <button type="button" class="state-on" :class="{'is-active': active}" @click="setActive(true)">On</button>
    </div>
    <div class="sites">
      <p>Konfigurasyon Listesi</p>
      <textarea v-model="list" rows="8" autocomplete="off" autocorrect="off" autocapitalize="off"
                spellcheck="false"></textarea>
      <button type="button" class="state-save" @click="saveList">Kaydet</button>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      active: true,
      list: "example.com",
      icons: {
        active: 'images/sample-icon-48.png',
        inactive: 'images/sample-icon-48-off.png'
      }
    }
  },
  created() {
    chrome.storage.sync.get(["toggleSitesActive", "toggleSitesList"], (result) => {
      this.active = result.toggleSitesActive;
      this.list = result.toggleSitesList;
    })
  },
  methods: {
    setActive(active) {
      this.active = active;
      chrome.storage.sync.set({
        toggleSitesActive: active
      }, () => {
      });

      chrome.browserAction.setIcon({
        path: this.icons[active ? 'active' : 'inactive']
      })
    },
    saveList() {
      chrome.storage.sync.set({
        toggleSitesList: this.list
      }, () => {
      });
    }
  }
}
</script>
