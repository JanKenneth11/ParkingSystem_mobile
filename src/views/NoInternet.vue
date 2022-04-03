<template>
    <ion-page>
        <ion-content force-overscroll="false" overflow-scroll="false" class="ps-background-color">
            <ion-row>
            <div class="ion-justify-content-center ion-align-items-center img-container">
                <ion-img src="/assets/img/lost-wireless-connection.png"></ion-img>
                <h2>No Internet connection found. Check your connection or try again.</h2>
            </div>
            </ion-row>
        </ion-content>
    </ion-page>
</template>
<script>
import{IonPage,IonContent,IonRow,IonImg} from '@ionic/vue';
export default {
    setup() {
        
    },
    components: {
        IonPage,IonContent,IonRow,IonImg
    },
}
</script>
<style scoped>
div.img-container {
    display: flex;
    height: 90vh;
    flex-direction: column;
}
h2 {
    color: #F6FBFF;
    text-align: center;
    font-size: 20px;
    margin: 0 30px;
}
</style>