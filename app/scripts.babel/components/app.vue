<template>
  <div class="viewport">
    <md-app>
      <md-app-toolbar>
        <md-button class="md-icon-button" @click="menuVisible = !menuVisible">
          <md-icon>menu</md-icon>
        </md-button>
        <div class="md-title" style="flex: 1">{{ $i18n('appTitle') }}</div>
      </md-app-toolbar>


      <md-app-drawer :md-active.sync="menuVisible">
        <md-toolbar class="md-title" style="flex: 1" md-elevation="0"> {{ $i18n('appTitle') }}</md-toolbar>




        <!-- Support section -->

        <md-list>
          <md-subheader class="md-accent">{{ $i18n('supportlSetting') }}</md-subheader>
          <md-list-item>
            <div class="md-list-item-text">
              <a href="https://chrome.google.com/webstore/detail/opendyslexic-font-for-chr/cdnapgfjopgaggbmfgbiinmmbdcglnam/reviews"
                target="_blank">
                {{ $i18n('reviews') }}
              </a>
            </div>
            <md-tooltip md-direction="top">{{ $i18n('supportToolTip') }}</md-tooltip>
            <div class="md-list-item">
              <a href="https://chrome.google.com/webstore/detail/opendyslexic-font-for-chr/cdnapgfjopgaggbmfgbiinmmbdcglnam/reviews"
                target="_blank">
                <md-icon>rate_review</md-icon>
              </a>
            </div>
          </md-list-item>

          <md-list-item>
            <div class="md-list-item-text">
              <a href="http://opendyslexic.com/"
                target="_blank">
                {{ $i18n('websiteLabel') }}
              </a>
            </div>
  
            <div class="md-list-item">
              <a href="http://opendyslexic.com/"
                target="_blank">
                <md-icon>rate_review</md-icon>
              </a>
            </div>
          </md-list-item>
          <md-list-item>
            <div class="md-list-item-text">
              <a href="https://chrome.google.com/webstore/detail/opendyslexic-font-for-chr/cdnapgfjopgaggbmfgbiinmmbdcglnam/support"
                target="_blank">
                {{ $i18n('supportlSetting') }}
              </a>
            </div>
            <md-tooltip md-direction="top">{{ $i18n('supportToolTipTwo') }}</md-tooltip>
            <div class="md-list-item">
              <a href="https://chrome.google.com/webstore/detail/opendyslexic-font-for-chr/cdnapgfjopgaggbmfgbiinmmbdcglnam/support"
                target="_blank">
                <md-icon>help</md-icon>
              </a>
            </div>
          </md-list-item>
        </md-list>


        <md-list>
          <md-subheader class="md-accent">{{ $i18n('socialSetting') }}</md-subheader>
          <md-list-item>
            <div class="md-list-item-text">
              <a href="https://twitter.com/OpenDyslexic" target="_blank">
                {{ $i18n('followDyslexic') }}
              </a>
            </div>
            <md-tooltip md-direction="top">{{ $i18n('followDyslexicToolTip') }}</md-tooltip>
            <div class="md-list-item">
              <a href="https://twitter.com/OpenDyslexic" target="_blank">
                <md-icon>person_add</md-icon>
              </a>
            </div>
          </md-list-item>
          <md-list-item>
            <div class="md-list-item-text">
              <a href="https://www.patreon.com/opendyslexic" target="_blank">
                {{ $i18n('followAbbie') }}
              </a>
            </div>
            <md-tooltip md-direction="top">{{ $i18n('followAbbieToolTip') }}</md-tooltip>
            <div class="md-list-item">
              <a href="https://www.patreon.com/opendyslexic" target="_blank">
                <md-icon>person_add</md-icon>
              </a>
            </div>
          </md-list-item>
          <md-list-item>
            <div class="md-list-item-text">
              <a href="https://www.helperbird.com/" target="_blank">
                {{ $i18n('followRobertJames') }}
              </a>
            </div>
            <md-tooltip md-direction="top">{{ $i18n('followRobertJamesToolTip') }}</md-tooltip>
            <div class="md-list-item">
              <a href="https://www.helperbird.com/" target="_blank">
                <md-icon>person_add</md-icon>
              </a>
            </div>
          </md-list-item>
        </md-list>

      </md-app-drawer>

      <md-app-content>
        <md-content>
          <!-- Enable font -->
          <md-list>
            <md-subheader class="md-accent">{{ $i18n('options') }}</md-subheader>
            <md-list-item>
              <span class="md-list-item-text"> {{ $i18n('enableFontLabel') }}</span>
              <div class="md-list-item">
                <md-switch v-model="enable" @change="enableDyslexica"></md-switch>
              </div>
            </md-list-item>
          </md-list>

          <md-list>
            <md-subheader class="md-accent">{{ $i18n('comingSoon') }}</md-subheader>
            <md-list-item>
              <span class="md-list-item-text"> {{ $i18n('featureTint') }}</span>
              <div class="md-list-item">
                <md-switch disabled></md-switch>
              </div>
            </md-list-item>
          </md-list>



        </md-content>


      </md-app-content>


    </md-app>



    <md-snackbar :md-duration="duration" :md-active.sync="showSnackbar" v-html='popupMessage' md-persistent>
    </md-snackbar>
  </div>


</template>

<script>
  import Vue from 'vue';
  import 'material-icons/iconfont/material-icons.css';

  import i18n from 'vue-plugin-webextension-i18n';
  Vue.use(i18n);
  module.exports = {
    data: function () {
      return {
        paid: true,
        enable: null,
        font: 'opendyslexic',
        duration: 3000,
        showSnackbar: false,
        popupMessage: '',
        menuVisible: false,
      }
    },
    mounted: function () {
      const SETTING_KEYS = ['font', 'enabled'];
      chrome.storage.sync.get(SETTING_KEYS, settings => {
        this.enable = settings.enabled ? true : false;
        this.font = settings.font ? settings.font : 'opendyslexic';
        return this.enable;
      });
    },
    methods: {
      fontChanged: function () {
        this.enable = false;
        this.save('font', this.font);
      },
      save: function (token, type) {
        let setting = {};
        setting[token] = type;
        chrome.storage.sync.set(setting);
        return setting;
      },

      enableDyslexica: function () {
        // Dont change the settings if its the same or no past
        this.showSnackbar = true;
        this.popupMessage = chrome.i18n.getMessage('saved');
        this.save('enabled', this.enable);
        this.reload();
      },
      reload: function () {
        chrome.tabs.query({
          active: true,
          lastFocusedWindow: true
        }, function (tabs) {
          this.showSnackbar = true;
          this.popupMessage = chrome.i18n.getMessage('reloadingMessage');
          // and use that tab to fill in out title and url
          const CURRENT_TAB = tabs[0];
          const SCRIPT = 'window.location.reload();';
          chrome.tabs.executeScript(CURRENT_TAB.id, {
            code: SCRIPT
          })
        });
      }
    }
  }
</script>




<style scoped>
  .viewport,
  body {
    width: 320px;
    max-width: 100%;
    display: inline-block;
    vertical-align: top;
    border: 1px solid rgba(#000, .12);
    height: 500px;
    background-color: #fafafa;
  }

  .md-drawer.md-temporary.md-left+.md-app-container .md-content,
  .md-list.md-theme-default {
    background-color: #fafafa;
  }

  .md-app {
    height: 535px;
  }

  .md-drawer.md-temporary.md-active {
    background-color: #fafafa;
  }

  .full-control {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap-reverse;
  }

  .list {
    padding: 0 16px;
  }

  .full-control>.md-list {
    width: 320px;
    max-width: 100%;
    height: 400px;
    display: inline-block;
    overflow: auto;
    border: 1px solid rgba(#000, .12);
    vertical-align: top;
  }

  .control {
    min-width: 250px;
    display: flex;
    flex-direction: column;
    padding: 16px;
  }
</style>