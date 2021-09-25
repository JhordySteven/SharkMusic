<template>
   <div >
     <v-simple-table class="container">
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left" >#</th>
              <th class="text-left">TÍTULO</th>
              <th class="text-left">ALBUM</th>
              <th class="text-left">FECHA INCORPORACIÓN</th>
              <th class="text-left"><Icon path={mdiAccount}
        title="User Profile"
        size={1}
        horizontal
        vertical
        rotate={90}
        color="red"
        spin/>
    )</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(item,index) in objListMusic"
              :key="index">
              <td class="text-center">{{ index+1 }}</td>
              <td class="text-left"><img class="listImages" :src="item.channel.urls.logo_image.original">{{ item.title }}</td>
              <td class="text-left">{{ item.channel.title}}</td>
              <td class="text-left">{{formatDate(item.uploaded_at)}}</td>
              <td class="text-left">{{dateDuration(item.duration)}}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
   </div>
</template>

<script>
import axios from 'axios';
import { mdiAccount } from '@mdi/js'

export default {
  name: 'HelloWorld',
  data(){
    return{
      objListMusic:[]
    }
  },
  props: {
    msg: String
  },
  methods:{
    obtenerListadoMusic(){
      axios.get('https://api.audioboom.com/audio_clips/popular').then((response)=>{
        this.objListMusic=response.data.body.audio_clips
        console.log(response.data.body.audio_clips);
      })
    },
    dateDuration(seconds){
       var hour = Math.floor(seconds / 3600);
      hour = (hour < 10)? '0' + hour : hour;
      var minute = Math.floor((seconds / 60) % 60);
      minute = (minute < 10)? '0' + minute : minute;
      var second = Math.round(seconds % 60);
      second = (second < 10)? '0' + second : second;
      return hour + ':' + minute + ':' + second;
    },
    formatDate(date){
      let dFecha = new Date(date);
      console.log(dFecha);
      let dFechaFormat=dFecha.getDate()+'/' + (dFecha.getMonth()+1) + '/'+dFecha.getFullYear();
      return dFechaFormat;
    }
  },
  mounted(){
    this.obtenerListadoMusic();
    this.dateDuration();
  }
}
</script>

<style scoped>
.container{
  width: 100%;
  height: 100%;
  padding: 0px;
  margin: 0px;
  color: white;
}
.listImages{
  width: 50px;
  height: 100%;
  margin: 0px;
  padding: 0px;
}
/*.containerList{
  border: 2px solid blue;
  text-decoration: none;
  padding: 0px;
}
.containerList>li{
  border: 2px solid greenyellow;
  display: block;
  text-decoration: none;
  margin: 0px;
  padding: 0px;
  width: 100%;
  height: 50px;
  overflow: hidden;
}
.containerList>li>section{
  width: 100%;
  height: 100%;
  border:1px solid blueviolet;
  overflow: hidden;
}
.containerList>li>section>span{
  font-family: 'Marcellus', serif;;
  font-size: 20px;
  color: white;
  border: 1px solid orange;
  height: 100%;
  width: auto;
  text-align: center;
}
.listImages{
  width: 50px;
  height: 100%;
  margin: 0px;
  padding: 0px;
} */
</style>
