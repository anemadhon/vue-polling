<template>
  <div class="container-emot">
    <button v-for="(emot, index) in emoticons"
            :key="index"
            :id="emot"
            :value="emot"
            :disabled="isDisable"
            :class="{active: emot == status}"
            @click="forVote"
            class="emot-btn"
        ></button>
  </div>
</template>

<script>

import moment from "moment";

export default {
    name: "Vote",
    props: {
        whenClick: {
            type: Function
        }
    },
    data: function () {
        return {
            emoticons: ['very-bad','bad','ok','good','very-good'],
            status: ''
        }
    },
    methods: {
        forVote(e){
            let voted = e.target.value
            
            this.store(voted)
            this.whenClick()
        },
        store(voted){
            let voted_at = moment().format('YYYY-MM-DD hh:mm:ss')
            let voted_at_desc = moment().format('a')
            let key_voted = moment().format('YYYYMMDDhhmmss')
            this.status = voted

            let data = {
                voted: voted,
                voted_at: voted_at,
                voted_at_desc: voted_at_desc
            }
            
            let data_string = JSON.stringify(data)

            localStorage.setItem(key_voted, data_string)
        }
    },
    computed:{
        isDisable:function(){
            return this.status.length === 0 ? false : true
        }

    },
    mounted(){
        this.$root.$on('emitProccessDone', () => {
            this.status = ''
        })
    }
};

</script>

<style scoped>
  .container-emot {
    display: flex;
  }

  .emot-btn {
    background: url('~@/assets/emoticon.png');
    width: 101px;
    height: 100px;
    outline: none;
    border: none;
    margin: 0px 10px;
    cursor: pointer;
  }

  .emoticons {
    display: flex;
    justify-content: center;
  }

  #very-bad {
    background-position: 0px 0px;
  }
  
  #very-bad:hover {
    background-position: 0px -100px;
  }
  
  #very-bad.active,
  #very-bad:active {
    background-position: 0px -200px;
  }
  
  #bad {
    background-position: -101px 0px;
  }
  
  #bad:hover {
    background-position: -101px -100px;
  }
  
  #bad.active,
  #bad:active {
    background-position: -101px -200px;
  }
  
  #ok {
    background-position: -202px 0px;
  }
  
  #ok:hover {
    background-position: -202px -100px;
  }
  
  #ok.active,
  #ok:active {
    background-position: -202px -200px;
  }
  
  #good {
    background-position: -303px 0px;
  }
  
  #good:hover {
    background-position: -303px -100px;
  }
  
  #good.active,
  #good:active {
    background-position: -303px -200px;
  }
  
  #very-good {
    background-position: -404px 0px;
  }
  
  #very-good:hover {
    background-position: -404px -100px;
  }
  
  #very-good.active,
  #very-good:active {
    background-position: -404px -200px;
  }
</style>
