<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title class="ion-text-center">Calculator</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title class="ion-text-center">Calculator</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-button @click="isNumberDividableBy2">Place a number</ion-button>
      <ion-input v-model="number"></ion-input>
      <ion-button v-if="even" v-model="number">Congrats! Your number is even</ion-button>
      <ion-button v-if="!even" v-model="number">Oops! Your number is not even</ion-button>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton, IonInput } from '@ionic/vue';
import { defineComponent, ref, inject } from 'vue';

export default defineComponent({
  name: 'HomePage',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton,
    IonInput
  },
  setup() {
    let number = ref(0);
    let even = ref(false);
    let client = inject('mqttClient');

    function isNumberDividableBy2(){
      if (number.value % 2 == 0){
        even.value = true;
      }
      else{
        even.value = false;
      }
    }
    return{
      number,
      even,
      client,
      isNumberDividableBy2
    }
  }
});
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
