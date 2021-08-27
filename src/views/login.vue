<template>
<ion-page>
    <ion-header>
  <ion-toolbar>
    <ion-title
      class="ion-text-center"
      style="color: white;">ورود</ion-title
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
                        خوش آمدید
                        </h2>
                    </ion-label>
                </ion-col>
            </ion-row>
      <ion-row class="ion-justify-content-center ion-padding-horizontal">
        <ion-col class="ion-text-left" size="12" size-lg="8">
          <ion-input v-model="user" placeholder="نام کاربری"> </ion-input>
        </ion-col>
      </ion-row>

      <ion-row
        class="ion-justify-content-center ion-padding-horizontal"
        style="padding-top: 0.3em"
      >
        <ion-col class="ion-text-left" size="12" size-lg="8">
          <ion-input
            shape="round"
            placeholder="رمز عبور"
            type="password"
            v-model="pass"
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
            @click="auth({user:this.user,pass:this.pass})"
              shape="round"
              style="width: 100%; color: white; --background: #191970; text-transform: none;">
              ورود
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
import { defineComponent } from 'vue';
import { useMutation } from '@vue/apollo-composable'
import gql from 'graphql-tag'

import { useRouter,useRoute } from 'vue-router';


export default defineComponent({
  name: 'Login',
  components:{
IonRow, IonCol,IonInput,IonCard,IonButton,IonHeader,IonToolbar,IonTitle,IonLabel,IonGrid,IonContent,IonPage
  },
  setup(){
      const router = useRouter();
      const route = useRoute();
    //   return { router };
       const { mutate: auth,onDone } = useMutation(gql`
      mutation auth ($user: String!,$pass:String!) {
        tokenAuth (username: $user , password:$pass) {
          token
          user{
      id
      firstName
      lastName
      melliCode
    }
        }
      }
    `)

    onDone(result=>{
        console.log(result.data.tokenAuth.token)
        // localStorage.setItem(token,result.data.tokenAuth.token)
        localStorage.token = result.data.tokenAuth.token;
        localStorage.id = result.data.tokenAuth.user.id;
        localStorage.fname = result.data.tokenAuth.user.firstName
        localStorage.lname = result.data.tokenAuth.user.lastName
        localStorage.melliCode = result.data.tokenAuth.user.melliCode
        router.push({ path: '/tabs/home' })
        // router.replace()
    })

    return{
        auth,
        router,
        route
    }
  },
  data(){
      return{
          user:'',
          pass:''
      }
  }
  ,methods:{
login(){
      console.log(this.user,this.pass)
     

    // return{
    //     auth({username:this.username,password:this.pass})
    // }
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