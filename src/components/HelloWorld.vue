<template>

  <div>
    <h1>{{ msg }}</h1> 
    <div class="row" style="margin-top: 1%;"><input type="text" id="ctrl" v-model="magicword" class="form-control" style="width:15%;margin-left: 42%;"/></div>
    <div class="btn-group" style="margin-top: 1%;">
      <button type="button" style="width:180px;height:50px" class="btn btn-primary" @click="start">Start Watcher</button>
      <button type="button" style="width:180px;height:50px" class="btn btn-primary" @click="stop">Stop Watcher</button>
      <button type="button" style="width:200px;height:50px" class="btn btn-primary" @click="getFileList">File Created List</button>
    </div>
    <div v-if="Showlist">
      <table style="width:80%;margin-top: 5%;margin-left: 10%;" border="1 solid">
        <tr style="height:40px;"><th>FileName</th>
        <th style="font-weight:bold">Action</th>
        <th style="font-weight:bold">TaskStartTime</th>
        <th style="font-weight:bold">TaskEndTime</th>
        <th style="font-weight:bold">Total Runtime</th>
        <!-- <th></th> -->
        <th style="font-weight:bold">MagicWord</th>
        <th style="font-weight:bold">Count</th></tr>
        <tr v-for="val in FileList" v-bind:key="val.id" style="height:40px;">
         <td>{{ val.FileName }}</td>
         <td>{{ val.TaskName }}</td>
         <td>{{ val.TaskStartTime }}</td>
         <td>{{ val.TaskEndTime }}</td>
         <td>{{ val.TaskDuration }}</td>
         <td>{{ val.MagicWord }}</td>
         <td>{{ val.Count }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default{
  data(){
    return{
      status:false,
      magicword:"",
      FileList :[],
      Showlist:false,
      // WatcherRunning:false
    }
  },
  props:{
    msg:{
      type:String,
      default:""
    }
  },
  methods:{
    start(){
      let self=this
      if (self.status===true){
        alert("Already watcher is running")
        return
      }
      if (self.magicword==""){
        alert("Magic Word Is Require")
        return
      }
      self.status =true
      let param={
        MagicWord :self.magicword,
        Status :self.status
      }
      axios.post("http://localhost:8081/start-watcher", param)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    },
    stop(){
      let self=this
      self.status =false
      axios.post("http://localhost:8081/stop-watcher")
      .then(function (response) {
        alert(response.Msg);
      })
      .catch(function (error) {
        console.log(error);
      });
    },
    getFileList(){
      let self=this
      axios.get("http://localhost:8081/get-task-details")
      .then(function (response) {
        console.log("response data",self.FileList)
        self.Showlist=true
        self.FileList=response.data
        //console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  }
}
</script>
<style>

</style>