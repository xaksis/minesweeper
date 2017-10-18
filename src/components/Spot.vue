<template>
  <div>
    <div v-if="!spotObject.cleared"
      class="spot btn"
      @click="spotClicked"
      @contextmenu.prevent.stop="spotMarkedSafe">
      <span v-if="spotSafe" class="safe-mark"></span>
    </div>
    <div v-else-if="spotObject.cleared && !spotObject.mine"
      class="spot">
      <span v-if="mineCount" :class="mineCountClassObject">{{ mineCount }}</span>
    </div>
    <div v-else
      :class="mineClassObject">
      <span class="mine-icon"></span>
    </div>
  </div>
</template>

<script>
export default {

  name: 'Spot',

  // we will store index as id of this spot
  props: [
    'id', 
    'spotObject',
    'board',
  ],
  data () {
    return {
      clickedMine: false,
      spotSafe: false,
    };
  }, 
  computed: {
    mineCount() {
      return this.board.mineCount(this.id);
    },
    mineCountClassObject() {
      return {
        'mine-count': true,
        'mine-count--green': this.mineCount > 1,
        'mine-count--red': this.mineCount > 2, 
      };
    },
    mineClassObject() {
      return {
        'spot': true,
        'spot--mine': this.spotObject.mine,
        'spot--clicked-mine': this.clickedMine,
      }
    },
  },
  methods: {
    spotClicked() {
      if(this.spotSafe) {
        // we don't want to do anything if we've marked
        // spot safe. 
        return; 
      }

      if(this.spotObject.mine){
        this.clickedMine = true;
      }
      // every time a spot is clicked, we need 
      // to emit an event to let the parent know
      this.$emit('spotClicked', this.id);
    },

    spotMarkedSafe() {
      this.spotSafe = !this.spotSafe;
    },
    reset() {
      this.spotSafe = false;
      this.clickedMine = false;
    }
  },
};
</script>

<style lang="css" scoped>
.spot{
  box-sizing: border-box;
  width:  30px;
  height:  30px;
  float:  left;
  border:  1px solid #F4F6F8;
  position:  relative;
}
.spot.btn{
  background:  linear-gradient(45deg, #FBFBFC, #D7E0E7);
  border:  1px solid #C2CBD8;
}

.mine-icon{
  position:  absolute;
  display: block;
  left:  50%;
  margin-left:  -5px;
  width:  8px;
  height:  8px;
  background-color:  #4b5768;
  border-radius: 50%;
  top:  10px;
}

.spot--clicked-mine::before{
  content:  '';
  position:  absolute;
  left:  0px;
  right:  0px;
  top:  0px;
  bottom:  0px;
  background-color: #E26F57;
}

.mine-count{
  font-weight: bold;
  color:  #2196F3;
}
.mine-count--red{
  color:  #E26F57;
}
.mine-count--green{
  color:  #75B968;
}
.safe-mark{
  display:  block;
  position:  absolute;
  width:  2px;
  height:  20px;
  background-color: orange;
  left:  8px;
  top:  5px;
}
.safe-mark::after{
  content:  '';
  position:  absolute;
  width: 0; 
  height: 0; 
  border-top: 5px solid transparent;
  border-bottom: 5px solid transparent;
  
  border-left: 10px solid red;
}

</style>