<template>
  <ion-app class="scanner-hide">
    <Menu v-if="checkUser"></Menu>
    <ion-router-outlet id="main-content"/>
  </ion-app>
    <div class="scanner-ui">
      <p>To scan QR code, place QR at the center of the camera</p>
      <div class="scanner-filter"></div>
    </div>
</template>

<script>
import { chevronBackOutline } from 'ionicons/icons';
import { IonApp, IonRouterOutlet, useBackButton, useIonRouter } from '@ionic/vue';
import Menu from './views/MainPages/Menu.vue';
import { App } from '@capacitor/app';


export default {
  name: 'App',
  setup() {
      const ionRouter = useIonRouter();
      useBackButton( -1, () => {
        if (!ionRouter.canGoBack()) {
            if(window.confirm('are you sure wanna exit app.')){
              console.log('true');
              App.exitApp();
            } else {
              console.log('false');
            }
        }
      });
  },
  data: () => ({
        chevronBackOutline,
  }), 
  components: {
    IonApp,
    IonRouterOutlet,
    Menu,
  },
  mounted() {
    window.addEventListener('load', () => {
      // 1st, we set the correct status when the page loads
      if(!navigator.onLine){
        window.alert('No network connection!');
        setTimeout(App.exitApp(), 100);
      }
      // now we listen for network status changes
      window.addEventListener('online', () => {
        console.log('online window listener');
        this.successNotify('Back Online');
          this.$router.push(localStorage.getItem("lastVisitedPage"));
      });

      window.addEventListener('offline', () => {
        console.log('offline window listener');
        this.errorNotify('You are now Offline');
        localStorage.setItem("lastVisitedPage", this.$router.currentRoute._value.fullPath);

        this.$router.push('/no-internet-page');
      });
    });
    // QRScanner.prepare(this.onDone); // show the prompt
  },
  methods : {
    onDone(err, status){
      if (err) {
      // here we can handle errors and clean up any loose ends.
      console.error(err);
      }
      if (status.authorized) {
        // W00t, you have camera access and the scanner is initialized.
        // QRscanner.show() should feel very fast.
      } else if (status.denied) {
      // The video preview will remain black, and scanning is disabled. We can
      // try to ask the user to change their mind, but we'll have to send them
      // to their device settings with `QRScanner.openSettings()`.
      } else {
        // we didn't get permission, but we didn't get permanently denied. (On
        // Android, a denial isn't permanent unless the user checks the "Don't
        // ask again" box.) We can ask again at the next relevant opportunity.
      }
    },
    checkUser(){
      return this.$store.getters.user.id ? true : false
    },
  }
};
</script>
