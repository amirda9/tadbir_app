<template>
<ion-page>
  <ion-header>
  <ion-toolbar>
    <ion-title
      class="ion-text-center"
      style="color: white;">اسناد</ion-title
    >

  </ion-toolbar>
</ion-header>
<ion-content>
  <ion-row>
      <ion-col>
          <!-- <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                             تعداد کل اسناد : {{res2}}
                        </ion-label>
                    </ion-col>
                </ion-row> -->

          <!-- <ion-button @click="result({id:'VXNlck5vZGU6Mg=='})"></ion-button> -->
          <div v-if="loading">Loading...</div>

  <div v-else-if="error">Error: {{ error.message }}</div>


  <div v-else-if="res">
    <div v-for="item of res" :key="item">
      <ion-card class="ion-padding">
          <!-- <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                             {{item.node.id}} :شماره
                        </ion-label>
                    </ion-col>
                </ion-row> -->
                <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label v-if="item.node.status == 'WAITFORKARSHENAS'">
                               در حال بررسی کارشناس
                        </ion-label>
                         <ion-label v-if="item.node.status == 'WAITFORARZYAB'">
                               در حال بررسی ارزیاب
                        </ion-label>
                         <ion-label v-if="item.node.status == 'ACCEPTED'">
                               تایید برای پرداخت
                        </ion-label>
                         <ion-label v-if="item.node.status == 'REJECTED'">
                               عودت
                        </ion-label>
                    </ion-col>
                </ion-row>
       <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                             هزینه درخواستی : {{item.node.hazineDarkhasti}}
                        </ion-label>
                    </ion-col>
                </ion-row>
                <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                             هزینه تاییدی : {{item.node.hazineTaeidi}}
                        </ion-label>
                    </ion-col>
                </ion-row>
                <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label v-if="item.node.bimePaye == true">
                             بیمه پایه دارد
                        </ion-label>
                        <ion-label v-if="item.node.bimePaye == false">
                             بیمه پایه ندارد
                        </ion-label>
                    </ion-col>
                </ion-row>

                <ion-row class="ion-padding" >
                    <ion-col class="ion-text-center">
                        <ion-label>
                             نام هزینه  : {{item.node.bimeshavandeGharardadHazine.hazine.name}}
                        </ion-label>
                    </ion-col>
                </ion-row>
    </ion-card>
    </div>
    <!-- {{res}} -->
  </div>
      </ion-col>
  </ion-row>
</ion-content>
</ion-page>
</template>

<script lang="ts">
import { IonRow, IonCol,IonTitle,IonToolbar,IonHeader,IonLabel,IonCard,IonContent ,IonPage} from '@ionic/vue';
import { defineComponent } from 'vue';

import { useQuery,useResult } from '@vue/apollo-composable'
import gql from 'graphql-tag'


export default defineComponent({
  name: 'Docs',
  setup(){
      const { result,loading, error,variables } = useQuery(gql`
      query hazines($idLocal:ID!){
  user(id:$idLocal){
    bimeshavanadegharardad{
      bimeshavandegharardadhazineSet{
        edges{
          node{
            hazine{
              name
            }
            personalSaghf
            pazireshSet{
              totalCount
              edges{
                node{
                  hazineTaeidi
                  hazineDarkhasti
                  bimePaye
                  status
                  id
                  bimeshavandeGharardadHazine{
                    hazine{
                      name
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
  `,{
  
    idLocal: localStorage.id,
  
})

    const res = useResult(result, null, data => data.user.bimeshavanadegharardad.bimeshavandegharardadhazineSet.edges[0].node.pazireshSet.edges)
    const res2 = useResult(result, null, data => data.user.bimeshavanadegharardad.bimeshavandegharardadhazineSet.edges)

    return{
        res,loading,error
    }
  },
  data(){
      return{
          id:"VXNlck5vZGU6Mg==",
      }
  },
  components: {
    IonRow, IonCol,IonTitle,IonToolbar,IonHeader,IonLabel,IonCard ,IonContent,IonPage
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