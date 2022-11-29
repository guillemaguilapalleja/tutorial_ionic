<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Camera actions</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <ion-row>
        <ion-col col-1>
          <ion-button style="width:100%" @click="startVideoStream">Start video stream</ion-button>
        </ion-col>
        <ion-col col-2>
          <ion-button style="width:100%" color="danger" @click="stopVideoStream">Stop video stream</ion-button>
        </ion-col>
      </ion-row>
      <canvas style = "margin-left: 5%; margin-top: 5%; width: 60%; height: 70%; border-style: solid" id = "output"></canvas>
      <ion-list>
        <ion-item>
          <ion-select v-model="mode" interface="popover">
            <ion-select-option value="Normal">Normal</ion-select-option>
            <ion-select-option value="Shovel">Shovel</ion-select-option>
            <ion-select-option value="Scharr">Scharr</ion-select-option>
            <ion-select-option value="Laplacian">Laplacian</ion-select-option>
          </ion-select>
        </ion-item>
      </ion-list>
      <ion-list>
        <ion-item>
          <ion-select v-model="mode" interface="popover">
            <ion-select-option value="Normal">Normal</ion-select-option>
            <ion-select-option value="Gray">Gray</ion-select-option>
            <ion-select-option value="Canny">Canny</ion-select-option>
            <ion-select-option value="Blued">Blued</ion-select-option>
            <ion-select-option value="Pinked">Pinked</ion-select-option>
          </ion-select>
        </ion-item>
        </ion-list>
    </ion-content>
  </ion-page>
</template>

<script>
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonRow, IonCol, IonButton, IonItem, IonSelect, IonSelectOption, IonList } from '@ionic/vue';
import { defineComponent, inject, onMounted, ref } from 'vue';
import * as cv from 'opencv.js'

export default defineComponent({
  name: 'Tab2Page',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonRow, 
    IonCol,
    IonButton,
    IonItem,
    IonSelect,
    IonSelectOption,
    IonList
  },

  setup () {

    const emitter = inject('emitter');
    let client = inject('mqttClient');
    let mode = ref("Normal");

    onMounted(() => {
      client.on('message', (topic, message) => {
        if (topic == "cameraService/IonicTutorial/videoFrame") {
          const img = new Image();
          img.src = "data:image/jpg;base64,"+message;
          img.onload = () => {  
            let dst = new cv.Mat();
            let source = cv.imread(img);

            cv.cvtColor(source, source, cv.COLOR_RGB2GRAY, 0);
            if (mode.value == "Normal") {
              dst = cv.imread(img);
            }

            if (mode.value == "Shovel") {
              cv.Sobel(source, dst, cv.CV_8U, 1, 0, 3, 1, 0, cv.BORDER_DEFAULT);
            }

            if (mode.value == 'Scharr') {
                cv.Scharr(source, dst, cv.CV_8U, 1, 0, 1, 0, cv.BORDER_DEFAULT);
            }

            if (mode.value == 'Laplacian') {
              cv.Laplacian(source, dst, cv.CV_8U, 1, 1, 0, cv.BORDER_DEFAULT);
            }

            if (mode.value == 'Gray') {
              dst = source;
            }

            if (mode.value == 'Canny') {
              let mat = cv.imread(img);
              dst = new cv.Mat();
              cv.cvtColor(mat, dst, cv.COLOR_RGB2GRAY,0);
              cv.Canny(mat, dst, 50, 100, 3, false);
              mat.delete()
            }

            if(mode.value == 'Blued'){
              let mat = cv.imread(img);
              dst = new cv.Mat();
              cv.cvtColor(mat, dst, cv.COLOR_RGB2XYZ,0);
              mat.delete()
            }
            
            if(mode.value == 'Pinked'){
              let mat = cv.imread(img);
              dst = new cv.Mat();
              cv.cvtColor(mat, dst, cv.COLOR_RGB2YCrCb,0);
              mat.delete()
            }
            cv.imshow('output', dst)
          }
        }
      })
    })

  function startVideoStream () {
    client.subscribe("cameraService/IonicTutorial/videoFrame")
    client.publish("IonicTutorial/cameraService/startVideoStream", "");
  }

  function stopVideoStream () {
    client.publish("IonicTutorial/cameraService/stopVideoStream", "")
  }
    return {
      emitter,
      client,
      startVideoStream,
      stopVideoStream, 
      mode
    }
  }
});
</script>

<style>

</style>
