<template>
  <ion-page class="container">
    <ion-header v-if="backLink">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button
            :defaultHref="backLink"
            color="medium"
          ></ion-back-button>
        </ion-buttons>
        <ion-progress-bar v-if="loadingO || loadingA || loadingT || loadingC || loadingW || loadingD || loadingL" type="indeterminate" color="tertiary"></ion-progress-bar>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ctn" :scroll-events="true" @ionScroll="handleScroll($event)">
      <ion-row
        v-if="title"
        class="ion-margin-start ion-margin-bottom ion-padding-bottom"
      >
        <h1 class="view-title ion-text-capitalize">{{ title }}</h1>
      </ion-row>

        <slot />
        <div v-show="isVisible && !containerDisturb" class="router-container">
          <ion-icon
          @click="() => router.push('/home')"
          size="large"
          color="dark"
          :icon="home"
        ></ion-icon>
        <ion-icon
          @click="() => router.push('/transactions')"
          size="large"
          color="dark"
          :icon="swapHorizontal"
        ></ion-icon>
        <ion-icon
        @click="() => router.push('/settings')"
        size="large"
        color="dark"
        :icon="settings"
        ></ion-icon>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  IonContent,
  IonHeader,
  IonBackButton,
  IonButtons,
  IonPage,
  IonToolbar,
  IonProgressBar
} from "@ionic/vue";
import { settings } from "ionicons/icons";
import { swapHorizontal } from "ionicons/icons";
import { home } from "ionicons/icons";
import { useRouter } from "vue-router";
import {useAuthStore} from "../../stores/auth"
import {useOverviewStore} from "../../stores/overview";
import {useAssetsStore} from "../../stores/assets";
import {useTxStore} from "../../stores/transactions";
import {useCardStore} from "../../stores/cards";
import {useWalletStore} from "../../stores/wallets";
import {useDocsStore} from "../../stores/documents";
import {useLoanStore} from "../../stores/loans";
import {defineComponent, ref} from "vue";
import {storeToRefs} from "pinia";


export default defineComponent({
  name: "HomePage",
  props: ["backLink", "title", "containerDisturb"],
  components: {
    IonPage,
    IonContent,
    IonHeader,
    IonBackButton,
    IonButtons,
    IonToolbar,
    IonProgressBar
  },
  setup() {
    const isVisible = ref(true);
    const router = useRouter();
    const authStore = useAuthStore();
    const overviewStore = useOverviewStore();
    const assetsStore = useAssetsStore()
    const txStore = useTxStore()
    const cardStore = useCardStore()
    const walletStore = useWalletStore()
    const docsStore = useDocsStore()
    const loanStore = useLoanStore()
    const scrollTop = ref(0)
    const {loadingUser} = storeToRefs(authStore);

     const {loading: loadingO} = storeToRefs(overviewStore);
     const {loading: loadingA} = storeToRefs(assetsStore);
     const {loading: loadingT} = storeToRefs(txStore);
     const {loading: loadingC} = storeToRefs(cardStore);
     const {loading: loadingW} = storeToRefs(walletStore);
     const {loading: loadingD} = storeToRefs(docsStore);
     const {loading: loadingL} = storeToRefs(loanStore);

     const handleScroll = (ev) => {
      scrollTop.value = ev.detail.scrollTop
        if(scrollTop.value >= 100){
          isVisible.value = false
          }else{
            isVisible.value = true
        }
    }

    return {
      settings,
      swapHorizontal,
      home,
      router,
      loadingUser,
      loadingO,
      loadingA,
      loadingT,
      loadingC,
      loadingW,
      loadingD,
      loadingL,
      isVisible,
      handleScroll,
    };
  },
});
</script>

<style scoped>
.router-container {
  transition: all 0.2s ease;
  background: var(--ion-color-tertiary);
  border: 1px solid white;
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 40vw;
  max-width: 160px;
  height: 6vh;
  border-radius: 9999px;
  position: fixed;
  top: 90vh;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1;
  margin: 0 auto;
}
</style>
