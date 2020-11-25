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
      subNet: "10.18.1.0/24,10.18.19.0/23",
      result: "",
    };
  },
  methods: {
    // 获取子网 
    // 求差价 let difference = Array.from(new Set(a.concat(b).filter(v => !aSet.has(v) || !bSet.has(v))))
    _getSubNet(network) {
      var net = network.split(",");
      console.log(net);
      this._printSubNet(this._splitNetWork(net));
    },
    _printSubNet(subNetwork) {
      for(let i=0;i<subNetwork.length;i++){
        console.log(this._getBrocastAddr(subNetwork[i].net[3],subNetwork[i].mask))
      }
    },
    _splitNetWork(network) {
      var arr = [];
      for (var i = 0; i < network.length; i++) {
        var tmp = network[i].split("/");
        var networkHost = tmp[0].split(".");
        arr.push({
          net: networkHost.map(Number),
          // mask: this._convertMask(tmp[1]),
          mask: tmp[1]
        });
      }
      console.log(arr[0])
      return arr;
    },
    _getBrocastAddr(net, mask){
      console.log(mask)
      return (Math.pow(2, 32-mask)-1 + net)
    }
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
