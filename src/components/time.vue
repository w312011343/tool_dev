<template>
  <div class='timeTrans'>
  <div class="pageTitle">时间戳转换</div>
  <div class="tiemTransWrap">
    <div class="item flex-align-center">
      <span class="title">现在</span>
      <div class="valueWrap flex-align-center">
          <div class="value flex-align-center mr-8">
            <input placeholder="自动获取时间戳" class="input mr-5" type="text" disabled="true" v-model="nowDate" />
            <div class="button mr-5" @click="autoTimeSwitch()">{{switchLabel}}</div>
          </div>
      </div>
    </div>
    <div class="item flex-align-center">
      <label class="title">时间戳</label>
       <div class="valueWrap flex-align-center">
        <div class="value flex-align-center mr-8">
            <input placeholder="请输入时间戳" class="input" type="text" v-model="time" />
            <select name="timeSelect" class="select mr-5" v-model="timeSelect">
            <option v-for="(item,index) in options" :key="index" :value="item" :selected="timeSelect.id === item.id">{{item.label}}</option>
            </select>
            <div class="button mr-5" @click="getFormatTime()">转换</div>
            <input placeholder="转换值" class="input" type="text" disabled="true" v-model="timeTransVal" />
          </div>
       </div>
    </div>
    <div class="item flex-align-center">
      <label class="title">时间</label>
       <div class="valueWrap flex-align-center">
        <div class="value flex-align-center mr-8">
            <input placeholder="请输入时间" class="input" type="text" v-model="date" />
            <select name="timeSelect" class="select mr-5" v-model="dateVal">
            <option v-for="(item,index) in options" :key="index" :value="item" :selected="dateVal.id === item.id">{{item.label}}</option>
            </select>
            <div class="button mr-5" @click="getDateTime()">转换</div>
            <input placeholder="转换值" class="input" type="text" disabled="true" v-model="dateTransVal" />
          </div>
       </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: "TimeTrans",
  data () {
    return {
      nowDate:null,
      interval:null,
      time:null,
      autoTimeSwitchFlag:true,
      switchLabel:'暂停',
      options:[
        {
          label:'毫秒(ms)',
          len:13,
          id:'ms'
        },
        {
          label:'秒(s)',
          len:10,
          id:'s'
        }
      ],
      timeSelect: {
          label:'毫秒(ms)',
          len:13,
          id:'ms'
      },
      dateVal: {
          label:'毫秒(ms)',
          len:13,
          id:'ms'
      },
      date:null,
      timeTransVal:'',
      dateTransVal:''
      
    }
  },
  mounted() {
    this.setDate();
    this.time = this.getNowTime();
    this.date = this.getFormate(this.getNowTime());
  },
  methods: {
    /**
     * 定时器，一秒
     */
   setDate() {
      this.interval = setInterval(() => {
        this.nowDate = this.getNowTime();
      },1000);
   },
   /**
    * 获取现在时间毫秒数
    */
   getNowTime() {
      return (new Date()).getTime();
   },
   /**
    * 毫秒数转换时间
    */
   getFormatTime() {
    let len = this.timeSelect.len;
    let val = Number(String(this.time).padEnd(len,0));
    this.timeTransVal = this.getFormate(val);
   },
   /**
    * 时间格式
    */
   getFormate(val) {
      let date = new Date(val);
      let year = date.getFullYear();
      let month = String(date.getMonth() + 1).padStart(2,0);
      let day = String(date.getDate()).padStart(2,0);
      let hour = String(date.getHours()).padStart(2,0);
      let minutes = String(date.getMinutes()).padStart(2,0);
      let secoonds = String(date.getSeconds()).padStart(2,0);
      return `${year}-${month}-${day} ${hour}:${minutes}:${secoonds}`
   },
   /**
    * 暂停、开启定时器
    */
   autoTimeSwitch() {
    this.autoTimeSwitchFlag = !this.autoTimeSwitchFlag;
    if(this.autoTimeSwitchFlag) {
      this.switchLabel = '暂停';
      this.setDate();
    }else {
      this.switchLabel = '开启';
      clearInterval(this.interval);
    }
   },

   /**
    * 时间转换时间戳
    */
   getDateTime() {
    let len = this.dateVal.len;
    let val = (new Date(this.date)).getTime();
    this.dateTransVal = String(val).split('').splice(0,len).join('');
   }
  }
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
  .input{
    width: 150px;
    height: 24px;
    line-height: 24px;
    text-indent: 10px;
  }
  .select{
    width: 80px;
    height: 30px;
    line-height: 30px;
    margin-left: 8px;
  }
  .mr-8{
    margin-right: 8px;
  }
  .mr-5{
    margin-right: 5px;
  }
  .timeTrans{
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
    .tiemTransWrap{
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

 
  
  .button{
    padding: 3px 15px;
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
