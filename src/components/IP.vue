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
    _getSubNet(network) {
      // console.log(subNet);
      var net = network.split(",");
      console.log(net);
      this._splitNetWork(net);
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
        this._convertMask(obj[1])
        // res = REG.exec("192.168.1.0/24");
        console.log(this._convertIp(res));
        console.log(network);
        console.log(res);
      }

      console.log(arr);
    },
    _convertMask(netMask) {
      let mask = [];
      let n = parseInt(netMask / 8);
      let m = netMask % 8;
      let t = "0b";
      for (let i = 0; i < n; i++) {
        mask.push(0b11111111);
      }
      if (m == 0) {
        for (let k = 0; k < 4 - n; k++) {
          mask.push(0);
        }
      } else {
        for (let j = 0; j < m; j++) {
          t += "1";
        }
        t = t << (8 - m);
        mask.push(t);
        if (n < 3) {
          for (let k = 0; k < 3 - n; k++) {
            mask.push(0);
          }
        }
      }
      return mask;
    },
    _convertIp(ip) {
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
