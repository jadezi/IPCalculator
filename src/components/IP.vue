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
      subNet: "10.18.1.0/24,10.18.19.0/26,10.18.19.128/26",
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
      let group = []
      for(let i=0;i<subNetwork.length;i++){
        group.push({tag:subNetwork[i].net[2]+"."+subNetwork[i].net[3],standard:subNetwork[i].net[2]+'.0',netAdd:subNetwork[i].net[3],broAdd:this._getBrocastAddr(subNetwork[i].net[3],subNetwork[i].mask)})
      }
      console.log(group);
      this._dffNetWork(group)
    },
    _dffNetWork(netWorkGroup){
      console.log('start');
      console.log(netWorkGroup);
      let resGroup = []
      for(let i = 0;i<netWorkGroup.length;i++){
        for(let j = 1;j<netWorkGroup.length;j++){
          if(netWorkGroup[i].standard == netWorkGroup[j].standard){
            resGroup.push(netWorkGroup[i])
          }
          if(j==netWorkGroup.length-1 && resGroup.length!=0){
            resGroup.push(netWorkGroup[i])
          }
        }
        netWorkGroup = netWorkGroup.filter(item=>{return item.standard != netWorkGroup[i].standard})
        console.log(netWorkGroup);
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
