<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div class="dooioo-config-container">

      <div class="user-name-container">
        <input v-model="userName"/>
      </div>

      <div class="user-password-container">
        <input v-model="password"/>
      </div>

      <div class="user-location-container">
        <input v-model="userLocation"/>
      </div>

    </div>

    <left-menu :side-menu-items='sideMenuItems'
               :onSubItemClicked='onSubItemClickedD'
               class='left-menu-view'
    >
    </left-menu>

    <div class='right-container'>

      <div class='test-case-detail-process-container'>

        <button @click="addNewLine">add</button>
        <button @click="ok">ok</button>
        <button @click="execute">execute</button>

        <table border="1">
          <tr>
            <td>element_id</td>
            <td>data</td>
            <td>action</td>
            <td>should_sleep</td>
            <td>sleep_mills</td>
            <td>wish</td>
          </tr>

          <tr v-for="testCase in testCaseDetails">
            <td>{{testCase.elementId}}</td>
            <td>{{testCase.data}}</td>
            <td>{{testCase.action}}</td>
            <td>{{testCase.shouldSleep}}</td>
            <td>{{testCase.sleepMills}}</td>
            <td>{{testCase.wish}}</td>
          </tr>

          <tr v-show="addLine">
            <td>
              <input type="text" v-model="new_element_id"/>
            </td>
            <td>
              <input type="text" v-model="new_data"/>
            </td>
            <td>
              <input type="text" v-model="new_action"/>
            </td>
            <td>
              <input type="text" v-model="new_should_sleep"/>
            </td>
            <td>
              <input type="text" v-model="new_sleep_mills"/>
            </td>
            <td>
              <input type="text" v-model="new_wish"/>
            </td>
          </tr>

        </table>

      </div>

    </div>

  </div>

</template>

<script>

import LeftMenu from './LeftMenu'
import {NetRequest} from '../utils/NetUtils';
import * as apis from '../apis/APIs';

var jsonfile = require('fs');

export default {
  name: 'hello',
  data () {
    return {
      msg: 'Dooioo-Android-Test',
      userName: 'user-name',
      password: 'password',
      userLocation: 'userLocation',
      addLine:false,
      sideMenuItems:[
        {
          icon:'overview-icon',
          name:'NodeJs'
        },
        {
          icon:'knowledge-icon',
          name:'web前端',
          items:['css','html']
        },
        {
          icon:'data-icon',
          name:'框架',
          items:['vue','anguarlar']
        }
      ],
      testCaseDetails:[],
      test_case_category:"",
      test_case_name:"",
      new_element_id:"",
      new_data:"",
      new_action:"",
      new_should_sleep:false,
      new_sleep_mills:0,
      new_wish:""
    }
  },
  components:{
    'left-menu': LeftMenu
  },
  methods:{
    onSubItemClickedD(subItem){
      console.log(subItem);
    },
    addNewLine(){
      this.addLine = true;
    },
    new_case_line_id_change(ll){
      console.log('changed'+ll);
    },
    ok(){
      this.test_case_category = "testTen";
      this.test_case_name = "case_for_test";
      let newLine = {
        "elementId": this.new_element_id,
        "data": this.new_data,
        "action": this.new_action,
        "shouldSleep": this.new_should_sleep,
        "sleepMills": this.new_sleep_mills,
        "wish": this.new_wish
      };
      this.testCaseDetails.push(newLine);
      this.addLine = false;

      let testCaseJson = {
        "testCaseCategory": this.test_case_category,
        "testCaseName": this.test_case_name,
        "caseSteps": this.testCaseDetails
      };

      let strfyed = JSON.stringify(testCaseJson);
      console.log('testCaseJson:'+strfyed);
      NetRequest.postSend(apis.generate, strfyed)
      .then((responseJson)=> {
        console.log('保存成功!')
      })
      .catch((error)=> {
        console.log("error");
      })
    },
    execute(){
        console.log('开始执行测试用例');
        NetRequest.requestUrl(apis.execute +"?testCaseCategory="+'testcaseFive'+'&testCaseName='+'doLogin', "GET")
        .then((responseJson)=> {

        })
        .catch((error)=> {
          // todo handle error
        })
    }
  },

  created(){
    fetch('../testcases/testcase.json')
    .then(response => response.json())
    .then((json) => {
        console.log(json);
        this.testCaseDetails = json;
       });
    }
}








</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.dooioo-config-container{
  display: inline-flex;
}

.user-name-container{
  margin-right: 50px;
}

.userName{
}

.user-password-container{
}

.password{
}

.user-location-container{
  margin-left: 50px;
}

.userLocation{
}

.left-menu-view{

}

.right-container{
    width:80%;
    height: 600px;
    background: antiquewhite;
    margin-top: 50px;
    float:right;
}

.test-case-detail-process-container{
    width: 100%;
    height: 400px;
    background: palegreen;
    margin-bottom: 20px;
}

td{
  width:100px;
}





</style>
