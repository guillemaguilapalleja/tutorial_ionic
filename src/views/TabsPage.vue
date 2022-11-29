<template>
    <ion-page>
      <ion-tabs>
        <ion-router-outlet></ion-router-outlet>
        <ion-tab-bar slot="bottom">
          <ion-tab-button tab="HomePage" href='/home'>
            <ion-icon :icon="calculatorOutline"></ion-icon>
            <ion-label>Home Page</ion-label>
          </ion-tab-button>
            
          <ion-tab-button tab="Tab2" href='/tab2'>
            <ion-icon :icon="cameraOutline" />
            <ion-label>Tab 2</ion-label>
          </ion-tab-button>
          
          <ion-tab-button tab="Tab3" href='/tab3'>
            <ion-icon :icon="sunny" />
            <ion-label>Tab 3</ion-label>
          </ion-tab-button>
        </ion-tab-bar>
      </ion-tabs>
    </ion-page>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import { IonTabBar, IonTabButton, IonTabs, IonLabel, IonIcon, IonPage, IonRouterOutlet } from '@ionic/vue';
  import { cameraOutline, exitOutline, sunny, calculatorOutline } from 'ionicons/icons';
  
  export default defineComponent({
    name: 'TabsPage',
    components: { IonLabel, IonTabs, IonTabBar, IonTabButton, IonIcon, IonPage, IonRouterOutlet },
    setup() {
      return {
        cameraOutline, 
        sunny, 
        exitOutline,
        calculatorOutline
      }
    }
  });
  </script>
  