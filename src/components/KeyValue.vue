<template>
  <div class="wrapper">
    <div class="keyValueItemWrapper">
      <div class="keyValueItem" v-for="item in inputValueArray" :key="item.id" @click="changeKeyValue">
        {{item}}
        <img class="close" src="../assets/icon/close.png" alt="" @click="removeKeyValue">
      </div>
      <label>
        <input class="input" type="text" v-model="inputValue" placeholder="请输入key=value" @keyup.enter="handleSubmit">
      </label>
      <div class="add" @click="addKeyValue">
        添加
      </div>
    </div>
    <div>
      <button class="refer" @click="logInputItem">查询</button>
      <button class="empty" @click="inputEmpty">清空</button>
    </div>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component} from 'vue-property-decorator';

  @Component
  export default class KeyValue extends Vue {
    inputValue = ''
    inputValueArray: string[] = this.fetchRecords()
    inputValueObj: InputItem={}
    inputValueObjArray: InputItem[]=[]
    fetchRecords() {
      const data = JSON.parse(window.localStorage.getItem('key-value') || '[]') ;
      console.log(this.inputValueArray)
      return data
    }
    handleSubmit(){
      if (this.inputValue.search('=')===-1){
        alert('输入内容的格式有误')
      }else{
        this.fetchRecords()
        this.inputValueArray.push(this.inputValue)
        const cache: string = JSON.stringify(this.inputValueArray)
        localStorage.setItem('key-value', cache)
        const array: string[] = this.inputValue.split('=')
        const key = array[0]
        const value = array[1]
        this.inputValueObj={
          [key]:value
        }
        this.inputValueObjArray.push(this.inputValueObj)
        this.inputEmpty()
        const input = document.querySelector('label') as HTMLLabelElement
        input.style.display = 'none'
        const div = document.querySelector('.add') as HTMLDivElement
        div.style.display = 'block'
      }
    }
    addKeyValue(){
      const input = document.querySelector('label') as HTMLLabelElement
      input.style.display = 'flex'
      const div = document.querySelector('.add') as HTMLDivElement
      div.style.display = 'none'
    }
    inputEmpty(){
      this.inputValue=''
    }
    logInputItem(){
      const keyValue = localStorage.getItem('key-value')
      if (keyValue){
        console.log(JSON.parse(keyValue))
      }
    }
    getDom(e: Event){
      const myDom: HTMLDivElement = e.target as HTMLDivElement
      return myDom
    }
    changeKeyValue(e: Event){
      const item: HTMLDivElement = this.getDom(e)
      item.parentNode!.parentNode!.removeChild
    }
    removeKeyValue(e: Event){
      const item: HTMLDivElement = this.getDom(e)
      item.parentNode!.parentNode!.removeChild
      // this.fetchRecords()
      // const index = 1
      // this.inputValueArray.splice(index,1)
      // const cache: string = JSON.stringify(this.inputValueArray)
      // localStorage.setItem('key-value', cache)
    }

  }
</script>

<style lang="scss" scoped>
  .wrapper {
    display: flex;
    justify-content: start;
    flex-direction: column;
    align-items: start;
    > .keyValueItemWrapper{
      display: flex;
      flex-direction: row;
      justify-content: start;
      > label {
        display: none;
      }
      > .keyValueItem{
        border: 1px solid #333;
        border-radius: 5px;
        display: flex;
        flex-direction: row;
        >.close {
          width: 15px;
          height: 15px;
          vertical-align: -0.15em;
          overflow: hidden;
        }
    }

    }

  }

</style>