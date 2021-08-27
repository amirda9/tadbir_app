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
    
    <ion-row>
        <ion-col class="ion-text-center">
            <h1>
                اطلاعات شخصی
            </h1>
        </ion-col>
    </ion-row>
    <ion-row>
        <ion-col>
            <ion-card>
                <ion-row
      class="ion-justify-content-center ion-padding-horizontal ion-padding-top"
    >
    <ion-col class="ion-text-center" size="6">
          <img src="../pics/app.jpg" alt="" style="border-radius: 50%; height: 12vh; ">
    </ion-col>
                </ion-row>

                <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                             نام : {{fname}}
                        </ion-label>
                    </ion-col>
                </ion-row>
                <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                              نام خانوادگی : {{lname}}
                        </ion-label>
                    </ion-col>
                </ion-row>
                <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                             کدملی : {{melliCode}}
                        </ion-label>
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
                        <ion-label>
                             سقف هزینه شخصی : {{item.node.personalSaghf}}
                        </ion-label>
                    </ion-col>
                </ion-row>
    </ion-card>
    </div>

  </div>
      </ion-col>
  </ion-row>
  
</ion-content>
</ion-page>
</template>

<script lang="ts">
import { IonRow, IonCol,IonHeader,IonCard,IonToolbar,IonTitle,IonPage , IonContent , IonLabel} from '@ionic/vue';
import { defineComponent } from 'vue';

import { useQuery,useResult } from '@vue/apollo-composable'
import gql from 'graphql-tag'
// import { ref } from '@vue/composition-api'
// import { watch } from '@vue/composition-api'


export default defineComponent({

  name: 'Home',
  setup(){
    //   my:"VXNlck5vZGU6Mg=="
    // const idLocal = ref('')
      console.log(localStorage.id)
      const { result,loading, error,variables } = useQuery(gql`
      query hazines($idLocal:ID!){
  user(id:$idLocal){
    bimeshavanadegharardad{
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
  
})
    const res = useResult(result, null, data => data.user.bimeshavanadegharardad.bimeshavandegharardadhazineSet.edges)

    return{
        res,loading,error
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
    IonRow, IonCol,IonHeader,IonCard,IonToolbar,IonTitle,IonPage,IonContent,IonLabel
  },

   methods:{
       button(){
        //    console.log(this.id)
        // this.result({id:this.id})
       }
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