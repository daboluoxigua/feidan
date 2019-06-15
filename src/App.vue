<template>
  <div id="app">
    <el-row :gutter="20">
      <el-col :span="4">
        <div class="title">
          <i class="el-icon-setting"></i>
          <p>同步设置</p>
        </div>
      </el-col>
      <el-col :span="20">
        <el-form :model="synchronization" label-width="80px" size="small" class="miniItem">
          <el-form-item label="房间编号">
            <el-input v-model="synchronization.number"></el-input>
          </el-form-item>
          <el-form-item label="打盘游戏">
            <el-checkbox
              v-for="(item,index) in synchronization.game"
              :key="index"
              :label="item.name"
              v-model="item.checked"
            ></el-checkbox>
          </el-form-item>
          <el-form-item label="语音提示">
            <el-checkbox
              v-for="(item,index) in synchronization.voice"
              :key="index"
              :label="item.name"
              v-model="item.checked"
            ></el-checkbox>
          </el-form-item>
          <el-form-item label="两面对冲">
            <el-checkbox label="开启两面对冲" v-model="synchronization.hedging"></el-checkbox>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="4">
        <div class="title site">
          <i class="el-icon-guide"></i>
          <p>线路1</p>
        </div>
      </el-col>
      <el-col :span="20">
        <el-row>
          <el-form :model="site1" label-width="80px">
            <el-form-item label="线路类型">
              <el-checkbox
                v-for="(item,index) in site1.type"
                :key="index"
                :label="item.name"
                v-model="item.checked"
              ></el-checkbox>
            </el-form-item>
          </el-form>
        </el-row>
        <el-row :gutter="20">
          <el-col :span='6'>
            <div class="wrap">
              <el-divider content-position="left">盘口信息</el-divider>
              <el-form size='mini' label-width="80px" v-model="site1.handicap">
                <el-form-item label="账户状态"><el-input readonly></el-input></el-form-item>
                <el-form-item label="登录账户"><el-input readonly></el-input></el-form-item>
                <el-form-item label="当前余额"><el-input readonly v-model="site1.handicap.balance"></el-input></el-form-item>
                <el-form-item label="当前使用"><el-input readonly></el-input></el-form-item>
                <el-form-item label-width="0" style="text-align: center;"><el-button type="primary">刷新信息</el-button></el-form-item>
              </el-form>
            </div>
          </el-col>
          <el-col :span='6'>
            <div class="wrap">
              <el-divider content-position="left">登录设置</el-divider>
              <el-form size='mini' label-width="80px" v-model="site1.login">
                <el-form-item label="盘口地址"><el-input v-model="site1.login.platform"></el-input></el-form-item>
                <el-form-item label="登录账户"><el-input v-model="site1.login.user"></el-input></el-form-item>
                <el-form-item label="登录密码"><el-input show-password v-model="site1.login.pass"></el-input></el-form-item>
                <el-form-item label-width="0" style="text-align: center;">
                  <el-button type="primary" @click="login('site1')">登录</el-button>
                  <el-button type="danger">退出</el-button>
                </el-form-item>
                <el-form-item label="">&nbsp;</el-form-item>
              </el-form>
            </div>
          </el-col>
          <el-col :span='12'>
            <el-tabs v-model="activeName" type="card">
              <el-tab-pane label="用户管理" name="first">
                <div class="tableBox">
                  <el-table
                    :data="tableData"
                    border
                    size='mini'
                    max-height="222"
                    style="width: 100%">
                    <el-table-column
                      prop="date"
                      label="日期"
                      width="180">
                    </el-table-column>
                    <el-table-column
                      prop="name"
                      label="姓名"
                      width="180">
                    </el-table-column>
                    <el-table-column
                      prop="address"
                      label="地址">
                    </el-table-column>
                  </el-table>
                </div>
              </el-tab-pane>
              <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
              <el-tab-pane label="角色管理" name="third">角色管理</el-tab-pane>
              <el-tab-pane label="定时任务补偿" name="fourth">定时任务补偿</el-tab-pane>
            </el-tabs>
          </el-col>
        </el-row>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="4">
        <div class="title site">
          <i class="el-icon-guide"></i>
          <p>线路2</p>
        </div>
      </el-col>
      <el-col :span="20">2</el-col>
    </el-row>
    <el-row>
      <el-col :span="4">
        <div class="title site">
          <i class="el-icon-guide"></i>
          <p>线路3</p>
        </div>
      </el-col>
      <el-col :span="20">2</el-col>
    </el-row>
  </div>
</template>

<script>
let api = 'http://47.104.148.193:43990'

import axios from 'axios'
import qs from 'qs'
export default {
  name: "app",
  data() {
    return {
      activeName: 'first',
      synchronization: {
        number: "12345",
        game: [
          { name: "北京赛车", gameId: 1, checked: false },
          { name: "幸运废土", gameId: 2, checked: true }
        ],
        voice: [
          { name: "北京赛车", gameId: 1, checked: false },
          { name: "幸运废土", gameId: 2, checked: true }
        ]
      },
      site1: {
        type: [
          { name: "IDC", ID: 1, checked: false },
          { name: "SGWIN", ID: 1, checked: false },
          { name: "MOA", ID: 1, checked: false }
        ],
        handicap:{
          balance:''
        },
        login:{
          user:'dz037',
          pass: 'QWqw1212',
          platform: 'sgwin'
        }
      },
      tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }]
    };
  },
  methods:{
    login(formNmae){
        let postData = {}
        if(formNmae == 'site1'){
          postData = this.site1.login
        }else if(formNmae == 'site2'){
          postData = this.site2.login
        }else if(formNmae == 'site3'){
          postData = this.site3.login
        }
        postData.number = this.synchronization.number

        axios.post(api+'/my_game/login', qs.stringify(postData))
        .then(response=>{
          if(response.data.result == '登录成功'){
            this.$message({
              message: response.data.result,
              type: 'success'
            });
            //登录成功后开始查询余额
            this.balance(postData.platform,formNmae)

            //查询飞单时间
            this.header_time()
          }else{
            this.$message({
              message: response.data.result,
              type: 'error'
            });
          }
        })
        .catch(error=>  {
          console.log(error);
        });
    },
    //余额查询
    balance(platform,formNmae){
      let postData = {
        number:this.synchronization.number,
        'platform':platform
      }

      axios.post(api+'/my_game/balance', qs.stringify(postData))
        .then(response=>{
          if(response.data.msg == 'success'){

            if(formNmae == 'site1'){
              this.site1.handicap.balance = response.data.balance
            }else if(formNmae == 'site2'){
              this.site2.handicap.balance = response.data.balance
            }else if(formNmae == 'site3'){
              this.site3.handicap.balance = response.data.balance
            }

          }else{
            this.$message({
              message: response.data.msg,
              type: 'error'
            });
          }
        })
        .catch(error=>  {
          console.log(error);
        });
    },
    //飞单时间
    header_time(geme_id=3){
      let postData = {
        geme_id
      }
      axios.post(api+'/my_game/header_time', qs.stringify(postData))
        .then(response=>{
          if(response.data.result == '登录成功'){
            this.$message({
              message: response.data.result,
              type: 'success'
            });
          }else{
            this.$message({
              message: response.data.result,
              type: 'error'
            });
          }
        })
        .catch(error=>  {
          console.log(error);
        });
    },
    //飞单接口
    order(){
      let postData = { }
      axios.post(api+'/my_game/order', qs.stringify(postData))
        .then(response=>{
          console.log
          if(response.data.result == '登录成功'){
            this.$message({
              message: response.data.result,
              type: 'success'
            });
          }else{
            this.$message({
              message: response.data.result,
              type: 'error'
            });
          }
        })
        .catch(error=> {
          console.log(error);
        });
    },
  }
};
</script>

<style lang="less">
body{background: #EBEEF5;height: 100%;}
#app {
  width: 100%;
  height: 100%;
  padding: 0 15px;
  box-sizing: border-box
}
#app > .el-row {
  margin-top: 15px;
  background: #fff;
  border-radius: 5px;
  padding: 10px;
}
.title {
  padding: 25px;
  text-align: center;
  background: #f56c6c;
  color: #fff;
}
.title.site {
  background: #67c23a;
}
.title i {
  font-size: 40px;
}
.title p {
  margin-top: 10px;
}


.wrap{border-radius: 5px;border: 1px solid #E4E7ED;padding: 0 10px;}
.tableBox{padding: 10px;border-radius:0 0 5px 5px;border: 1px solid #E4E7ED;border-top: 0;}


.el-tabs__header{margin-bottom: 0 !important}
.miniItem{
  .el-form-item {
    margin-bottom: 5px !important;
  }
}
</style>
