<template xmlns:v-bind="http://www.w3.org/1999/xhtml" xmlns:v-on="http://www.w3.org/1999/xhtml">
  <ul class='side-menu'>
    <li v-for="(pItem,i) in sideMenuItems">
      <template v-if="pItem.items">
        <a>
          <i v-bind:class='[pItem.icon]'></i>
          {{pItem.name}}
          <i class="down-arrow"></i>
        </a>
        <ul v-if='pItem.items' class='sub-ul'
            v-bind:style="{height:(pItem.items.length)*32+'px'}">
          <li v-for="(cItem,j) in pItem.items">
            <a href="javascript:void(0)"
               v-bind:class="{'active':activeIndex==10*(i+1)+(j+1)}"
               @click="onSubItemClicke(pItem)"
            >
              {{cItem}}
            </a>
          </li>
        </ul>
      </template>
      <template v-else>
        <a href="javascript:void(0)" v-bind:class="{'active':activeIndex==10*(i+1)}"
        >
          <i v-bind:class='[pItem.icon]'></i>
          {{pItem.name}}
        </a>
      </template>
    </li>
  </ul>
</template>
<style>
     {
	  	 padding: 0;
	  	 margin: 0;
	  	 text-decoration: none;
	  	 list-style: none;
	  }
	  body
	  {
	  	 font-family: Helvetica, Tahoma, Arial, "Hiragino Sans GB", "Hiragino Sans GB W3", "Microsoft YaHei", STXihei, STHeiti, Heiti, SimSun, sans-serif;
	  }
      nav
      {
      	 height: 60px;
      	 width: 100%;
      	 background-color: black;
      }
      .side-menu
      {
      	position: fixed;
      	left: 0;
      	top: 60px;
      	width: 150px;
      	height: 100%;
      	border: 1px solid #e0e0e0;
        background-color: #ffffff;
      }
      a
      {
      	color: black;
      }
      .side-menu>li>a
      {
      	display: block;
      	padding: 10px;
      	font-size: 16px;
      	padding-left: 25px;
      	background: bisque;
      	margin-top: 8px;
      }
      .sub-ul
      {
         transition:all,.2s,ease-in-out;
         overflow: hidden;
      }
      .hide
      {
      	 height: 0px !important;

      }
      .sub-ul a
      {
      	font-size: 12px;
      	padding: 8px;
      	display: block;
      	padding-left: 40px;
      }
      i{
      	background: url(http://di-yibot.wezhuiyi.com/img/icon-spirit.png) no-repeat;
        display: inline-block;
        vertical-align: middle;
        overflow: hidden;
        width: 20px;
        height: 20px;
      }
      .active
      {
      	background-color: #1c81e6;
        color: #ffffff;
      }
      i.overview-icon
      {
      	background-position:  0px -16px;
      }
      i.knowledge-icon
      {
      	background-position: -20px -16px;
      }
      i.data-icon
      {
      	background-position: -40px -16px;
      }
      i.down-arrow
      {
        background-position: -72px -36px;
        width: 12px;
        height: 12px;
        transition:all,.2s,ease-in-out;
      }
      i.flip
      {
      	transform:rotate(180deg);
      }


</style>
<script>
    export default{
        data(){
            return{
            }
        },
        props:['sideMenuItems','onSubItemClicked'],
        data:function()
        {
            return     {
    	          activeIndex:10,
    	          hideIndex:[],
            }

        },
        methods:{
          onSubItemClicke(subItem){
            console.log('subItme'+subItem);
            this.onSubItemClicked(subItem);
          }
        }
}




</script>
