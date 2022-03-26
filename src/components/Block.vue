<template class="wrapper"> 
          <div class="block col-12 col-lg-3">            
              <div class="form_item mb-3">
                <label class="form-label" for="block_ID" v-bind:style="{ 'background-color': bgCol[blockID] }">Block ID</label>
                <input class="form-control form-text" type="text" name="block_ID" :value="blockID" disabled=true>
              </div>
              <div class="form_item mb-3">
                  <label class="form-label" for="nonce_ID" v-bind:style="{ 'background-color': bgCol[blockID] }">Nonce</label>
                  <input class="form-control form-text" type="text" name="nonce_ID" :value="nonce[blockID]" disabled=true>
              </div>
              <div class="form_item mb-3">
                  <label class="form-label" for="data" v-bind:style="{ 'background-color': bgCol[blockID] }">Data</label>
                  <input class="form-control form-text" type="text" name="data" :value="myData[blockID]" @keyup="$emit('minedHash', {newNonce: nonce[this.blockID], dat: $event.target.value}) ">
              </div>
              <div class="form_item mb-3">
                  <label class="form-label" for="hash" v-bind:style="{ 'background-color': bgCol[blockID] }">Hash</label>
                  <input class="form-control form-text" type="text" name="hash" placeholder="" :value="calcHash[blockID]" disabled>  
              </div>
              <div class="form_item mb-3">
                  <label class="form-label" for="prev_hash" v-bind:style="{ 'background-color': bgCol[blockID] }">Previous Hash</label>
                  <input class="form-control form-text" type="text" name="prev_hash"  :value="(pHash[blockID])">
              </div>
                  <a href="#" class="btn btn-primary" @click="mine()">MINE</a>  
          </div>
</template>

<script>
var sha256 = require('js-sha256') 

export default {

  name: 'Block',
  components: {  
  }, 
  props: {        
        blockID: Number,
        pHash: {
          default: () => ['0'],
          type: Array
        },
        bgCol: {
          default: () => ['red'],
          type: Array
        },
        myData: {
          default: () => [''],
          type: Array
        },
        nonce: {
          default: () => [''],
          type: Array
        },
        calcHash: {
          default: () => ['0'],
          type: Array
        },
    }, 

  methods: {  
    mine() {      
      var pattern = '0000';      
      var n = 0;
      var temp_hash = ''
      while (!temp_hash.startsWith(pattern) & n<500000) {            
            n++     
            temp_hash = sha256(this.blockID.toString() +  n.toString() +  this.myData[this.blockID] +  this.pHash[this.blockID])      
      }      
      this.$emit('minedHash', {newNonce: n, dat: this.myData[this.blockID]})  
    },
  }
}
</script>

<style>
.block {
  padding:none
}

.form_item {
  margin: 1%
}

.form-label {
  color: gainsboro;
  width: 100%;
  font-weight: bold;
}
</style>

