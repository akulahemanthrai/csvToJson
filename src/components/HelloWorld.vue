<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input id="csv" type="file" ref="file" @change="readFile">
   <div v-if="csvToJson.length>0">{{csvToJson}}</div>
    
  </div>
</template>

<script>
export default {
  props: {
    msg: String
  },
  data () {
    return {
      csvToJson : []
    }
  },
  methods: {
    readFile () {
       let reader = new FileReader();
       let file = this.$refs.file, seriesCollection;
        reader.onload =  () => {
          seriesCollection = reader.result.replace(/\n/g, ' ').split(' ');
          seriesCollection.pop();
          this.csvToJson = seriesCollection.map(str => {
            return this.construnctObj(str)
          })
          console.log(this.csvToJson)
        };
        reader.readAsBinaryString(file.files[0]);
    },
    construnctObj (str) {
            let seriesArr = str.split(',');
            let obj = {
                [seriesArr[0]] : []
            };
            seriesArr.forEach((item,index)=>{
                if(index > 0){
                    let yearScore = item.split('|');
                    obj[seriesArr[0]].push({
                        year : yearScore[0],
                        score: yearScore[1]
                    })
                }
            })
            return obj
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
