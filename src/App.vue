<template>
  <div id="app">
    <el-row>
      <el-col :span="4">
        <div class="title">
          <i class="el-icon-setting"></i>
          <p>同步设置</p>
        </div>
      </el-col>
      <el-col :span="20">
        <el-form :model="synchronization" label-width="80px">
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
    <el-row>
      <el-col :span="4">
        <div class="title site">
          <i class="el-icon-guide"></i>
          <p>站点1</p>
        </div>
      </el-col>
      <el-col :span="20">
        <el-form :model="site1" label-width="80px">
          <el-form-item label="站点类型">
            <el-checkbox v-for="(item,index) in site1.type" :key="index" :label="item.name" v-model="item.checked"></el-checkbox>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="4">
        <div class="title site">
          <i class="el-icon-guide"></i>
          <p>站点2</p>
        </div>
      </el-col>
      <el-col :span="20">2</el-col>
    </el-row>
    <el-row>
      <el-col :span="4">
        <div class="title site">
          <i class="el-icon-guide"></i>
          <p>站点3</p>
        </div>
      </el-col>
      <el-col :span="20">2</el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      synchronization: {
        number: "",
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
        ]
      }
    };
  },
};
</script>

<style lang="less">
#app {
  width: 100%;
  height: 100%;
}
#app > .el-row {
  margin-top: 15px;
}
.title {
  padding: 25px;
  text-align: center;
  background: #F56C6C;
  color: #fff;
}
.title.site {
  background: #67C23A;
}
.title i {
  font-size: 40px;
}
.title p {
  margin-top: 10px;
}

</style>
