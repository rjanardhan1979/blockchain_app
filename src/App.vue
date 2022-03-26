<template>
  <button class="btn btn-secondary" @click="this.addBlock()">Add New Block</button>
  <navigate />  
  <div class="row">
    <block v-for="bID in blocks" :key="bID" :id="bID" :blockID="bID" :myData="datArray" :pHash="prevArray" :nonce="nonceArray" :calcHash="hashArray" :bgCol="this.bCol" @minedHash="mine($event, bID)"></block>
  </div>
</template>

<script>
import block from './components/Block.vue'
import navigate from './components/nav.vue'
var sha256 = require('js-sha256')

export default {
  name: 'App',
  components: {block, navigate},

  data() {
    return {
      n_comp:1,
      blocks: [0],      
      prevArray:['0'],
      datArray:[''],
      bCol : ['red'],
      nonceArray: ['0'],
      hashArray: [sha256('0' + '0' + '' + '0')]
    }
  },

  methods: {
      addBlock() {
        var add_index = this.n_comp
        this.n_comp++
        this.blocks.push(add_index)
        this.nonceArray.push('')     
        this.datArray.push('')
        this.prevArray.push(this.hashArray[add_index-1]) 
        this.hashArray.push(sha256(add_index.toString() + this.nonceArray[add_index] + this.datArray[add_index] + this.prevArray[add_index]))
        this.bCol.push('red')  
      },    

      mine(newBlockDat, index) {        
        this.datArray[index] = newBlockDat.dat        
        this.nonceArray[index] = newBlockDat.newNonce              
        for (var i=index; i<=this.n_comp; i++) {   
          if (i === 0) { this.prevArray[i] = 0 } else {this.prevArray[i] = this.hashArray[i-1]}               
          this.hashArray[i] = sha256((i).toString() + this.nonceArray[i] + this.datArray[i] + this.prevArray[i])
          if (this.hashArray[i].startsWith('0000')) {this.bCol[i] = 'green' } else {this.bCol[i] = 'red'}  
        }  
      },

  },
}
</script>

<style>

#app {
  padding: 1%;  
}

.btn {

  margin-bottom: 10px;

}

</style>
