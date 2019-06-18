<template>
  <div id="app">
    <el-row class="bg" :gutter="20">
      <el-col style="width:180px">
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
          <!-- <el-form-item label="语音提示">
            <el-checkbox
              v-for="(item,index) in synchronization.voice"
              :key="index"
              :label="item.name"
              v-model="item.checked"
            ></el-checkbox>
          </el-form-item> -->
          <el-form-item label="两面对冲">
            <el-checkbox label="开启两面对冲" v-model="synchronization.is_duichong"></el-checkbox>
          </el-form-item>
          <el-form-item label="开启飞单">
            <el-checkbox label="开启飞单" v-model="synchronization.is_feidan"></el-checkbox>
          </el-form-item>
          <el-form-item label="">
            <el-button type="primary" @click="startFly">开始飞单</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="14">
        <el-row class="bg" :gutter="20">
          <el-col style="width:180px" :span="6">
            <div class="title site">
              <i class="el-icon-guide"></i>
              <p>线路1</p>
            </div>
          </el-col>
          <el-col :span="18">
              <el-form :model="site1" label-width="80px">
                <el-form-item label="线路类型">
                  <el-radio-group v-model="site1.login.platform">
                    <el-radio label="IDC">IDC</el-radio>
                    <el-radio label="SGWIN">SGWIN</el-radio>
                  </el-radio-group>
                </el-form-item>
              </el-form>

              <el-col :span='12'>
                <div class="wrap">
                  <el-divider content-position="left">盘口信息</el-divider>
                  <el-form size='mini' label-width="80px" v-model="site1.handicap">
                    <el-form-item label="账户状态"><el-input v-model="site1.state.state" readonly></el-input></el-form-item>
                    <el-form-item label="登录账户"><el-input v-model="site1.state.user" readonly></el-input></el-form-item>
                    <el-form-item label="当前余额"><el-input readonly v-model="site1.handicap.balance"></el-input></el-form-item>
                    <el-form-item label="当前使用"><el-input v-model="site1.handicap.betting" readonly></el-input></el-form-item>
                    <el-form-item label-width="0" style="text-align: center;"><el-button type="primary" @click="balance(site1.login.platform,'site1')">刷新信息</el-button></el-form-item>
                  </el-form>
                </div>
              </el-col>
              <el-col :span='12'>
                <div class="wrap">
                  <el-divider content-position="left">登录设置</el-divider>
                  <el-form size='mini' label-width="80px" v-model="site1.login">
                    <el-form-item label="盘口地址"><el-input v-model="site1.login.login_url"></el-input></el-form-item>
                    <el-form-item label="登录账户"><el-input v-model="site1.login.user"></el-input></el-form-item>
                    <el-form-item label="登录密码"><el-input show-password v-model="site1.login.pass"></el-input></el-form-item>
                    <el-form-item label-width="0" style="text-align: center;">
                      <el-button type="primary" @click="login('site1')">登录</el-button>
                      <el-button type="danger" @click="logout('site1')">退出</el-button>
                    </el-form-item>
                    <el-form-item label="">&nbsp;</el-form-item>
                  </el-form>
                </div>
              </el-col>
          </el-col>
        </el-row>
        <el-row class="bg" :gutter="20">
          <el-col style="width:180px" :span="6">
            <div class="title site">
              <i class="el-icon-guide"></i>
              <p>线路2</p>
            </div>
          </el-col>
          <el-col :span="18">
              <el-form :model="site2" label-width="80px">
                <el-form-item label="线路类型">
                  <el-radio-group v-model="site2.login.platform">
                    <el-radio label="IDC">IDC</el-radio>
                    <el-radio label="SGWIN">SGWIN</el-radio>
                  </el-radio-group>
                </el-form-item>
              </el-form>

              <el-col :span='12'>
                <div class="wrap">
                  <el-divider content-position="left">盘口信息</el-divider>
                  <el-form size='mini' label-width="80px" v-model="site2.handicap">
                    <el-form-item label="账户状态"><el-input v-model="site2.state.state" readonly></el-input></el-form-item>
                    <el-form-item label="登录账户"><el-input v-model="site2.state.user" readonly></el-input></el-form-item>
                    <el-form-item label="当前余额"><el-input readonly v-model="site2.handicap.balance"></el-input></el-form-item>
                    <el-form-item label="当前使用"><el-input v-model="site2.handicap.betting" readonly></el-input></el-form-item>
                    <el-form-item label-width="0" style="text-align: center;"><el-button type="primary" @click="balance(site2.login.platform,'site2')">刷新信息</el-button></el-form-item>
                  </el-form>
                </div>
              </el-col>
              <el-col :span='12'>
                <div class="wrap">
                  <el-divider content-position="left">登录设置</el-divider>
                  <el-form size='mini' label-width="80px" v-model="site2.login">
                    <el-form-item label="盘口地址"><el-input v-model="site2.login.login_url"></el-input></el-form-item>
                    <el-form-item label="登录账户"><el-input v-model="site2.login.user"></el-input></el-form-item>
                    <el-form-item label="登录密码"><el-input show-password v-model="site2.login.pass"></el-input></el-form-item>
                    <el-form-item label-width="0" style="text-align: center;">
                      <el-button type="primary" @click="login('site2')">登录</el-button>
                      <el-button type="danger" @click="logout('site2')">退出</el-button>
                    </el-form-item>
                    <el-form-item label="">&nbsp;</el-form-item>
                  </el-form>
                </div>
              </el-col>
          </el-col>
        </el-row>
        <el-row class="bg" :gutter="20">
          <el-col style="width:180px" :span="6">
            <div class="title site">
              <i class="el-icon-guide"></i>
              <p>线路3</p>
            </div>
          </el-col>
          <el-col :span="18">
              <el-form :model="site3" label-width="80px">
                <el-form-item label="线路类型">
                  <el-radio-group v-model="site3.login.platform">
                    <el-radio label="IDC">IDC</el-radio>
                    <el-radio label="SGWIN">SGWIN</el-radio>
                  </el-radio-group>
                </el-form-item>
              </el-form>

              <el-col :span='12'>
                <div class="wrap">
                  <el-divider content-position="left">盘口信息</el-divider>
                  <el-form size='mini' label-width="80px" v-model="site3.handicap">
                    <el-form-item label="账户状态"><el-input v-model="site3.state.state" readonly></el-input></el-form-item>
                    <el-form-item label="登录账户"><el-input v-model="site3.state.user" readonly></el-input></el-form-item>
                    <el-form-item label="当前余额"><el-input readonly v-model="site3.handicap.balance"></el-input></el-form-item>
                    <el-form-item label="当前使用"><el-input v-model="site3.handicap.betting" readonly></el-input></el-form-item>
                    <el-form-item label-width="0" style="text-align: center;"><el-button type="primary" @click="balance(site3.login.platform,'site3')">刷新信息</el-button></el-form-item>
                  </el-form>
                </div>
              </el-col>
              <el-col :span='12'>
                <div class="wrap">
                  <el-divider content-position="left">登录设置</el-divider>
                  <el-form size='mini' label-width="80px" v-model="site3.login">
                    <el-form-item label="盘口地址"><el-input v-model="site3.login.login_url"></el-input></el-form-item>
                    <el-form-item label="登录账户"><el-input v-model="site3.login.user"></el-input></el-form-item>
                    <el-form-item label="登录密码"><el-input show-password v-model="site3.login.pass"></el-input></el-form-item>
                    <el-form-item label-width="0" style="text-align: center;">
                      <el-button type="primary" @click="login('site3')">登录</el-button>
                      <el-button type="danger" @click="logout('site3')">退出</el-button>
                    </el-form-item>
                    <el-form-item label="">&nbsp;</el-form-item>
                  </el-form>
                </div>
              </el-col>
          </el-col>
        </el-row>
      </el-col>
      <el-col :span="10">
        <div class="bg" style="height: 1114px;maring-right:-10px">
        <el-tabs type="card" v-model="activeName" v-if="tabs.length>0">
              <el-tab-pane v-for="(item,index) in tabs" :key="index" :label="item.name" :name="index">
                <div class="tableBox">
                  <el-table
                    :data="tableData[item.gameId]"
                    border
                    size='mini'
                    height="1050"
                    style="width: 100%">
                    <el-table-column
                      prop="create_time"
                      label="日期"
                      width="180">
                    </el-table-column>
                    <el-table-column
                      prop="game_periods"
                      label="期数"
                      width="90">
                    </el-table-column>
                    <el-table-column
                      prop="game_text"
                      label="内容">
                    </el-table-column>
                    <el-table-column
                      prop="return_msg"
                      label="状态">
                    </el-table-column>
                  </el-table>
                </div>
              </el-tab-pane>
            </el-tabs>
            <div v-else>请先登录</div>
          </div>
      </el-col>
    </el-row>

  </div>
</template>

<script>
let api = 'http://47.104.81.141:8050'

import axios from 'axios'
import qs from 'qs'
export default {
  name: "app",
  data() {
    return {
      header_times:[],//存飞单剩余时间
      header_index:0,//存飞到第几个单
      activeName: 0,//tab显示
      //同步设置
      synchronization: {
        number: "",
        is_duichong:true,
        is_feidan:true,
        game: [
          { name: "北京赛车",obj:'a', gameId: 1, checked: true },
          { name: "幸运飞艇",obj:'b', gameId: 2, checked: true },
          { name: "极速时时彩",obj:'c',gameId: 3, checked: true },
          { name: "极速赛车",obj:'d', gameId: 4, checked: true },
          { name: "极速飞艇",obj:'e', gameId: 5, checked: true },
          { name: "澳洲幸运5",obj:'f', gameId: 6, checked: true },
          { name: "澳洲幸运10",obj:'g', gameId: 7, checked: true },
          { name: "重庆欢乐生肖",obj:'h', gameId: 8, checked: true },
        ],
        voice: [
          { name: "北京赛车", gameId: 1, checked: false },
          { name: "幸运废土", gameId: 2, checked: true }
        ]
      },
      tabs:[],
      //线路1、2、3
      site1: {
        state:{
          user:'',
          state:''
        },
        handicap:{
          balance:'',
          betting:''
        },
        login:{
          user:'dz037',
          pass: 'QWqw1212',
          platform: 'IDC',
          login_url:''
        },
      },
      site2: {
        state:{
          user:'',
          state:''
        },
        handicap:{
          balance:'',
          betting:''
        },
        login:{
          user:'',
          pass: '',
          platform: '',
          login_url:''
        },
      },
      site3: {
        state:{
          user:'',
          state:''
        },
        handicap:{
          balance:'',
          betting:''
        },
        login:{
          user:'',
          pass: '',
          platform: '',
          login_url:''
        },
      },
      tableData:{}
    };
  },
  methods:{
    logout(formNmae){
        let postData = {}
        if(formNmae == 'site1'){
          postData = this.site1.login
        }else if(formNmae == 'site2'){
          postData = this.site2.login
        }else if(formNmae == 'site3'){
          postData = this.site3.login
        }
        postData.number = this.synchronization.number

        axios.post(api+'/my_game/logout', qs.stringify(postData))
        .then(response=>{
          if(response.data.status == 1){
            this.$message({
              message: response.data.msg,
              type: 'success'
            });

            //设置状态
            this[formNmae].state = {
              state:'',
              user:''
            }
            this[formNmae].handicap = {
              balance:'',
              betting:''
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
        if(postData.number == ''){
          this.$message('请输入房间编号')
          return
        }
        if(postData.login_url == ''){
          this.$message('请输入盘口地址')
          return
        }

        axios.post(api+'/my_game/login', qs.stringify(postData))
        .then(response=>{
          if(response.data.result == '登录成功'){
            this.$message({
              message: response.data.result,
              type: 'success'
            });
            //设置状态
            this[formNmae].state = {
              state:'在线',
              user:postData.user
            }

            //登录成功后开始查询余额
            this.balance(postData.platform,formNmae)

            //查询飞单时间
            this.header_times = [] //飞单时间先清空
            let game_id = this.synchronization.game.filter(item => item.checked == true)
            if(this.synchronization.is_feidan && game_id.length>0){
              game_id.forEach((item,index) => {
                this.header_time(formNmae,item.gameId,index)
              })
            }

            //设置tabs
            this.tabs = game_id
            
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
      if(this[formNmae].state.state == ''){
        this.$message('请先登录')
        return
      }

      let postData = {
        number:this.synchronization.number,
        'platform':platform
      }

      axios.post(api+'/my_game/balance', qs.stringify(postData))
        .then(response=>{
          if(response.data.msg == 'success'){
            this[formNmae].handicap = response.data

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
    startFly(){

      let game_id = this.synchronization.game.filter(item => item.checked == true)
      if(game_id.length == 0){
        this.$message('请选择打盘游戏')
      }

      let formNmae = ''
      if(this.site1.state.state !== ''){
        formNmae = 'site1'
        this.header_times = []
        game_id.forEach((item,index) => {
        this.header_time(formNmae,item.gameId,index)
      })
      }else if(this.site2.state.state !== ''){
        formNmae = 'site2'
        this.header_times = []
        game_id.forEach((item,index) => {
          this.header_time(formNmae,item.gameId,index)
        })
      }else if(this.site3.state.state !== ''){
        formNmae = 'site3'
        this.header_times = []
        game_id.forEach((item,index) => {
          this.header_time(formNmae,item.gameId,index)
        })
      }else{
        this.$message('请先登录')
        return
      }


      //设置tabs
      this.tabs = game_id
    },
    //飞单时间
    header_time(formNmae,game_id,index){
      let postData = {
        game_id
      }
      axios.post(api+'/my_game/header_time', qs.stringify(postData))
        .then(response=>{
          if(response.data.msg == 'success'){
            this.header_times.push(response.data.header_times.filter(item => item.header_time>0))
            this.headerIndex(formNmae,game_id,index);
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
    //飞单时间和飞单之间的中转 执行一个飞单后再执行第二个
    headerIndex(formNmae,game_id,index){
      setTimeout(() => {
        this.order(formNmae,this.header_times[index][this.header_index].periods,game_id)
        this.header_index++
        this.headerIndex(formNmae,game_id,index)
      }, this.header_times[index][this.header_index].header_time);
    },
    //飞单接口
    order(formNmae,periods,game_id){
      let platform = ''
      if(formNmae == 'site1'){
        platform = this.site1.login.platform
      }else if(formNmae == 'site2'){
        platform = this.site2.login.platform
      }else if(formNmae == 'site3'){
        platform = this.site3.login.platform
      }

      let postData = {
        platform,
        game_id,
        game_periods:periods,
        number:this.synchronization.number,
        is_duichong:this.synchronization.is_duichong ? 1:0
      }

      axios.post(api+'/my_game/order', qs.stringify(postData))
        .then(response=>{
          // this.$message(response.data.msg);
          this.orderResult(game_id)
        })
        .catch(error=> {
          console.log(error);
        });
    },
    //飞单结果
    orderResult(game_id){
      let postData = {
        game_id,
        number:this.synchronization.number,
      }
      axios.post(api+'/my_game/order_result', qs.stringify(postData))
        .then(response=>{
          if(response.data.status==1){
            let name = this.synchronization.game[game_id-1].obj
            this.tableData[game_id] = response.data.order_results
          }
          
        })
        .catch(error=> {
          console.log(error);
        });
    }
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
#app .bg {
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
