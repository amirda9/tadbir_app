<template>
<ion-page>
  <ion-header>
  <ion-toolbar>
    <ion-title
      class="ion-text-center"
      style="color: white;">هزینه ها</ion-title
    >

  </ion-toolbar>
</ion-header>
<ion-content>
    
    <ion-grid>
    <ion-row>
        <ion-col class="ion-text-center">
            <h2>
                اطلاعات شخصی
            </h2>
        </ion-col>
    </ion-row>
    <ion-row>
        <ion-col>
            <ion-card style="--background:#87ceeb;">
                <ion-row
      class="ion-justify-content-center ion-padding-horizontal ion-padding-top"
    >
    <ion-col class="ion-text-center" size="6">
          <img src="../pics/app.jpg" alt="" style="border-radius: 50%; height: 12vh; ">
    </ion-col>
                </ion-row>

                <ion-row class="ion-padding-horizontal" >
                    <ion-col class="ion-text-center">
                        <ion-label style="color:white;">
                              {{fname}} {{lname}} 
                        </ion-label>
                    </ion-col>
                    <!-- <ion-col class="ion-text-center">
                        <ion-label style="color:white;">
                             
                        </ion-label>
                    </ion-col> -->
                </ion-row>
                <!-- <ion-row class="ion-padding" >
                    
                </ion-row> -->
                <ion-row class="ion-padding-horizontal ion-padding-top" >
                    <ion-col class="ion-text-center">
                        <ion-label style="color:white;">
                             کدملی <br>{{melliCode}}
                        </ion-label>
                    </ion-col>
                     <ion-col class="ion-text-center">
                        <ion-label style="color:white;">
                             شماره قرارداد <br>{{code}}
                        </ion-label>
                    </ion-col>
                </ion-row>
                <!-- <ion-row class="ion-padding" >
                   
                </ion-row> -->
                <ion-row class="ion-padding-horizontal ion-padding-top" >
                    <ion-col class="ion-text-center">
                        <ion-label style="color:white;">
                             نام سازمان <br> {{bimegozar}}
                        </ion-label>
                    </ion-col>
                    <ion-col class="ion-text-center">
                        <ion-label style="color:white;">
                             بیمه <br>{{bimegar}}
                        </ion-label>
                    </ion-col>
                </ion-row>
                <!-- <ion-row class="ion-padding" >
                    
                </ion-row> -->
                <ion-row class="ion-padding-horizontal ion-padding-top" >
                    <ion-col class="ion-text-center">
                        <ion-label style="color:white;">
                             بیمه شده : {{melliCode}}
                        </ion-label>
                    </ion-col>
                </ion-row>

<ion-row class="ion-padding ion-justify-content-center" >
                    <ion-col size="5" size-md="7" size-lg="7" class="ion-text-center">
                        <ion-button
            @click="$router.push('/changePass')"
              shape="round"
              size="small"
              style="width: 100%; color: white; --background: #191970; text-transform: none;">
              تغییر رمز عبور
            </ion-button>
                    </ion-col>
                </ion-row>
                
            </ion-card>
        </ion-col>
    </ion-row>
    <ion-row>
        <ion-col class="ion-text-center">
            <h1>
                هزینه ها
            </h1>
        </ion-col>
    </ion-row>

     <!-- {{ item.node.personalSaghf }}, -->
  <ion-row>
      <ion-col>
          <div v-if="loading">Loading...</div>

  <div v-else-if="error">Error: {{ error.message }}</div>

  <div v-else-if="res">
    <div v-for="item of res" :key="item">
      <ion-card>
        <ion-row>
            <ion-col class="ion-text-center">
               نام :‌ {{item.node.hazine.name}}‌
            </ion-col>
        </ion-row>
        <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                      <div v-if="item.node.personalSaghf != -1">
                        <ion-label>
                             سقف هزینه شخصی : {{item.node.personalSaghf}} ریال
                        </ion-label>
                        </div>

                        <div v-if="item.node.personalSaghf == -1">
                        <ion-label>
                             سقف هزینه شخصی : نامحدود
                        </ion-label>
                        </div>

                    </ion-col>
                </ion-row>
    </ion-card>
    </div>

  </div>
      </ion-col>
  </ion-row>
    </ion-grid>
</ion-content>
</ion-page>
</template>

<script lang="ts">
import { IonRow, IonCol,IonHeader,IonCard,IonToolbar,IonTitle,IonPage , IonContent , IonLabel,IonGrid,IonButton} from '@ionic/vue';
import { defineComponent } from 'vue';

import { useQuery,useResult } from '@vue/apollo-composable'
import gql from 'graphql-tag'
import { useRouter } from 'vue-router';
// import { ref } from '@vue/composition-api'
// import { watch } from '@vue/composition-api'


export default defineComponent({

  name: 'Home',
  setup(){
    //   my:"VXNlck5vZGU6Mg=="
    // const idLocal = ref('')
    const router = useRouter();
      console.log(localStorage.id)
      const { result,loading, error,variables } = useQuery(gql`
      query hazines($idLocal:ID!){
  user(id:$idLocal){
    bimeshavanadegharardad{
      gharardad{
        code
        bimegar{
          name
        }
        bimegozar{
          name
        }
      }
      bimeshavandegharardadhazineSet{
        edges{
          node{
              hazine{
              name
              code
              category {
                name
              }
              createdAt
            }
            personalSaghf
          }
        }
      }
    }
  }
}
    `,{
  
    idLocal: localStorage.id,
  
},
)

    const res = useResult(result, null, data => data.user.bimeshavanadegharardad.bimeshavandegharardadhazineSet.edges)
    const bimegar = useResult(result, null, data => data.user.bimeshavanadegharardad.gharardad.bimegar.name)
    const bimegozar = useResult(result, null, data => data.user.bimeshavanadegharardad.gharardad.bimegozar.name)
    const code = useResult(result, null, data => data.user.bimeshavanadegharardad.gharardad.code)

    return{
        res,loading,error,bimegar,code,bimegozar
    }
  },
  data(){
      return{
        id:localStorage.id,
        fname:localStorage.fname,
        lname:localStorage.lname,
        melliCode:localStorage.melliCode,
      }
  },
  components: {
    IonRow, IonCol,IonHeader,IonCard,IonToolbar,IonTitle,IonPage,IonContent,IonLabel,IonGrid,IonButton
  },

   methods:{
       button(){
        //    console.log(this.id)
        // this.result({id:this.id})
       },

       
   }
  
  
});
</script>

<style scoped>
ion-toolbar{
  --background:#191970;
}

ion-content{
  --ion-background-color: #f7f7f7;
}
</style>