<template>
  <div class="box">
    <el-row>
      <el-col>
        <el-input v-model="netWork" type="text" />
      </el-col>
    </el-row>
    <br />
    <el-row>
      <el-col>
        <el-input
          type="textarea"
          autosize
          placeholder="请输入内容"
          v-model="subNet"
        >
        </el-input>
      </el-col>
    </el-row>
    <br />
    <el-row>
      <el-button type="primary" @click="_getSubNet(subNet)">计算</el-button>
    </el-row>
    <el-row>
      {{ result }}
    </el-row>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      netWork: "",
      subNet: "192.168.1.0/24,10.18.29.0/23",
      result: "",
    };
  },
  methods: {
    _getSubNet(subNet) {
      console.log(subNet);
      var network = subNet.split(",");
      console.log(network);
      this._splitNetWork(network);
    },
    _splitNetWork(network) {
      var REG = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/;
      var arr = [];
      var res;
      for (var i = 0; i < network.length; i++) {
        var obj = network[i].split("/");
        console.log(obj);
        arr.push({ net: obj[0], mask: obj[1] });
        res = REG.exec(obj[0]);
        console.log(this._covertIp(res));
        
      }

      console.log(arr);
    },
    _covertIp(ip) {
      if (!ip) return -1;
      return (
        (parseInt(ip[1]) << 24) |
        (parseInt(ip[2]) << 16) |
        (parseInt(ip[3]) << 8) |
        parseInt(ip[4])
      );
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box {
  width: 40%;
  margin: 0 auto;
  padding: 10px;
}
</style>
