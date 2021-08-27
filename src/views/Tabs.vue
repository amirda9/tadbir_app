<template>
  <ion-page>
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="Home" href="/tabs/home">
          <ion-icon :icon="home" />
          <ion-label>خانه</ion-label>
        </ion-tab-button>
          
        <ion-tab-button tab="Docs" href="/tabs/docs">
          <ion-icon :icon="document" />
          <ion-label>اسناد</ion-label>
        </ion-tab-button>
        
        <!-- <ion-tab-button tab="tab3" href="/tabs/tab3">
          <ion-icon :icon="square" />
          <ion-label>Tab 3</ion-label>
        </ion-tab-button> -->
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script lang="ts">
import {IonPage,IonRouterOutlet,IonTabBar, IonTabButton, IonTabs, IonLabel, IonIcon, } from '@ionic/vue';
import { ellipse, square, triangle,home,document } from 'ionicons/icons';

export default {
  name: 'Tabs',
  components: {  IonPage , IonRouterOutlet,IonTabBar, IonTabButton, IonTabs, IonLabel, IonIcon, },
  setup() {
    return {
      ellipse, 
      square, 
      triangle,
      home,
      document
    }
  }
}
</script>