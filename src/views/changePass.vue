<template>
<ion-page>
    <ion-header>
  <ion-toolbar>
    <ion-title
      class="ion-text-center"
      style="color: white;">تغییر رمز</ion-title
    >

  </ion-toolbar>
</ion-header>

<ion-content>
  <div style="margin-top: 7em">
    <ion-grid>
        <div class="footer">
        <ion-card>
            <ion-row class="ion-padding">
                <ion-col class="ion-text-center">
                    <ion-label>
                        <h2>
                        تغییر رمز
                        </h2>
                    </ion-label>
                </ion-col>
            </ion-row>

            <ion-row class="ion-justify-content-center ion-padding-horizontal">
        <ion-col class="ion-text-left" size="12" size-lg="8">
          <ion-input v-model="passOld" placeholder=" رمز عبور قدیمی"> </ion-input>
        </ion-col>
      </ion-row>
      <ion-row class="ion-justify-content-center ion-padding-horizontal">
        <ion-col class="ion-text-left" size="12" size-lg="8">
          <ion-input v-model="pass" type="password" placeholder="رمز عبور"> </ion-input>
        </ion-col>
      </ion-row>

      <ion-row
        class="ion-justify-content-center ion-padding-horizontal"
        style="padding-top: 0.3em"
      >
        <ion-col class="ion-text-left" size="12" size-lg="8">
          <ion-input
            shape="round"
            placeholder=" تکرار رمز"
            type="password"
            v-model="passNew"
          >
          </ion-input>
        </ion-col>
      </ion-row>

      <!-- <ion-row
        class="ion-justify-content-center ion-padding-horizontal"
        style="padding-bottom: 1em"
      >
        <ion-col class="ion-text-left">
          <ion-label (click)="forget()" style="font-size: 0.8em; color: #3eb5df; padding-left: 0.5em;" translate>
            Forgot password?
          </ion-label>
        </ion-col>
      </ion-row> -->

    
        <ion-row class="ion-padding ion-justify-content-center ion-padding-horizontal">
          <ion-col class="ion-text-center" size="8" size-lg="6">
            <ion-button
            @click="CP()"
              shape="round"
              style="width: 100%; color: white; --background: #191970; text-transform: none;">
              برو
            </ion-button>
          </ion-col>
        </ion-row>
        </ion-card>
        </div>
    </ion-grid>
  </div>
</ion-content>
</ion-page>
</template>

<script lang="ts">
import { IonRow, IonCol,IonInput,IonCard,IonButton,IonHeader,IonToolbar,IonTitle,IonLabel,IonGrid,IonContent,IonPage } from '@ionic/vue';
import { defineComponent,ref } from 'vue';
import { useMutation } from '@vue/apollo-composable'
import gql from 'graphql-tag'

import { useRouter,useRoute } from 'vue-router';
// import { ref } from '@vue/composition-api'

export default defineComponent({
  name: 'changePass',
  components:{
IonRow, IonCol,IonInput,IonCard,IonButton,IonHeader,IonToolbar,IonTitle,IonLabel,IonGrid,IonContent,IonPage
  },
  setup(){
      const passOld = ref('')
      const pass = ref('')
      const passNew = ref('')
      const router = useRouter();
      const route = useRoute();
    //   const amir:()console.log(pass)
    //   return { router };
       const { mutate: changePasswordMutation,onDone } = useMutation(gql`
      mutation changePasswordMutation ($currentPassword: String!,$newPassword:String!) {
        changePasswordMutation (currentPassword: $currentPassword , newPassword:$newPassword) {
            status
        }
      }
    `, )



    onDone(result=>{
        console.log(result.data.changePasswordMutation.status)
        // console.log(passOld.value)
        if(result.data.changePasswordMutation.status==true){
            router.push('/home')
        }
        
    })

    return{
        changePasswordMutation,
        router,
        route,
        pass,
        passOld,passNew
    }
  },
  methods:{
      CP(){
        //   console.log(this.pass)
        if(this.pass == this.passNew){
        this.changePasswordMutation({currentPassword:this.passOld,newPassword:this.pass})
        }
        else{
            alert("رمز عبور با تکرار آن مطابقت ندارد.")
        }
      }
  }
  
});
</script>


<style scoped>
ion-toolbar{
  --background:#191970;
}


ion-input{
  border:1px solid #aaaaaa;
  border-radius: 20px 20px 20px 20px;
  --placeholder-color:#aaaaaa;
  --background:#ffffff;
  --padding-start:8px;
  text-align: right;
  --padding-end: 8px;
}


ion-content{
  --ion-background-color: #f7f7f7;
}


</style>