<template>
  <div class='password'>
  <div class="pageTitle">随机密码生成</div>
  <div class="passwordWrap">
    <div class="item flex-align-center">
      <span class="title">所用字符</span>
      <div class="valueWrap flex-align-center">
          <div class="value flex-align-center mr-8" v-for="(item,index) in labels" :key="index">
            <input class="mr-5" type="checkbox" v-model="item.check" @change="changeLabel()"/>
            <label class="label" v-if="item.label">{{item.label}}</label>
            <input placeholder="请输入特殊字符" class="input" type="text" v-model="item.value" v-if="item.type === 'input'"/>
          </div>
      </div>
    </div>
    <div class="item flex-align-center">
      <lable class="title">排除字符</lable>
       <div class="valueWrap flex-align-center">
        <div class="value flex-align-center mr-8">
            <input placeholder="请输入不想要的字符" class="input" type="text" v-model="exclude" />
          </div>
       </div>
    </div>
    <div class="item flex-align-center">
      <lable class="title">密码长度</lable>
      <div class="valueWrap flex-align-center">
        <div class="value flex-align-center mr-8">
            <select name="passwordLen" class="select mr-5" v-model="passwordLen">
              <option v-for="(item,index) in passwordOptions" :key="index" :value="index + 1">{{index + 1}}</option>
            </select>
            <label>位</label>
          </div>
       </div>
    </div>
    <div class="item flex-align-center">
      <lable class="title">密码数量</lable>
      <div class="valueWrap flex-align-center">
        <div class="value flex-align-center mr-8">
            <select name="passwordNum" class="select mr-5" v-model="passwordNum">
              <option v-for="(item,index) in passwordNumOptions" :key="index" :value="index + 1">{{index + 1}}</option>
            </select>
            <label>个</label>
          </div>
       </div>
    </div>
    <div class="item flex-align-center buttonWrap">
        <div class="button" :class="{disabled:disabled}" @click="getPassword()">生成密码</div>
    </div>
     <div class="item flex-align-center">
      
      <div class="valueWrap flex-align-center resultWrap">
        <div class="resultTitle">结果展示</div>
        <textarea placeholder="生成密码结果展示" class="resultInfo" v-model="result"></textarea>
       </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: "PassWord",
  data () {
    return {
      labels:[
        {
          check:true,
          label:'a-z',
          value:this.getAToZ().toLocaleLowerCase(),
          id:'lower'
        },
        {
          check:true,
          label:'A-Z',
          value:this.getAToZ(),
          id:'upper'
        },
        {
          check:true,
          label:'0-9',
          value:'0123456789',
          id:'number'
        },
        {
          check:true,
          type:'input',
          value:"!@#$%^&*~<>?{}[]|()-=",
          id:'special'
        }
      ],
      exclude:'',
      passwordLen:20,
      passwordOptions:50,
      passwordNum:1,
      disabled:false,
      passwordNumOptions:10,
      result:''
    }
  },
  methods: {
    /**
     * 判断勾选
     */
    changeLabel() {
      let length = this.labels.length;
      let num = 0;
      this.labels.forEach(item => {
          if(!item.check) {
            num++;
          }
      });
      this.disabled = length === num;
    },
    /**
     * 获取密码
     */
    getPassword() {
      if(this.disabled) {
        return;
      }
      let labels = '';
      this.labels.forEach(item => {
        if(item.check) {
          labels += item.value;
        }
      });
      if(this.exclude.length) {
        this.exclude.split('').forEach(item => {
            labels = labels.replaceAll(item,'');
        });
      }
      let num = this.passwordNum;
      let len = this.passwordLen;
      let strLen = labels.length;
      let result = '';
      for (let i = 0; i < num; i++) {
        let temp = '';
       for(let j = 0; j < len; j++) {
          let random = parseInt(Math.random() * strLen);
          temp += labels[random];
       }
       result += temp + (i+1 !== num?'\n':'');
        
      }
      this.result = result;
    },
    /**
     * 大写A-Z
     */
    getAToZ() {
      let result = '';
      for (let i = 65; i <= 90; i++) {
        result += String.fromCharCode(i);
      }
      return result;
    }
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
  .input{
    width: 300px;
    height: 24px;
    line-height: 24px;
    text-indent: 10px;
  }
  .select{
    width: 50px;
  }
  .mr-8{
    margin-right: 8px;
  }
  .mr-5{
    margin-right: 5px;
  }
  .password{
    width: 60%;
    margin: 0 auto;
    .pageTitle{
      font-weight: bold;
      font-size: 24px;
      line-height: 28px;
    }
    .flex-align-center {
      display: flex;
      align-items: center;
    }
    .passwordWrap{
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      margin-top: 12px;
      border:1px solid rgb(128, 128, 128);
      .item {
        width: 100%;
        min-height: 60px;
        border-bottom:1px solid rgb(128, 128, 128);
        .title {
          background-color: #F5F5F5;
          width: 25%;
          display: inline-block;
          text-align: left;
          text-indent: 10px;
          margin-right: 12px;
          height: 100%;
          min-height: 60px;
          display: flex;
          align-items: center;
          font-size: 14px;
        }
        .valueWrap{
          flex: 1;
          justify-items: flex-start;
        }
      }
    }
  }
  .buttonWrap{
    align-items: center;
    justify-content: center;
  }
  .resultWrap{
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-start  !important;
    padding: 10px;
    .resultTitle {
    font-size: 16px;
    line-height: 20px;
    font-weight: bold;
    margin-bottom: 5px;
    }
    .resultInfo{
      width: 80%;
      min-height: 60px;
      padding: 8px;
    }
  }
  
  .button{
    padding: 10px 20px;
    background-color: #4CAF50; 
    border: none;
    color: white;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    cursor: pointer;
    &.disabled{
      background: gray;
      cursor: not-allowed;
    }
  }
</style>
