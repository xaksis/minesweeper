<template>
  <div>
    <div class="heading">
      <h2>YieldMo Mine-Sweeper</h2>
      <div v-if="lost" class="try-again-block lost">
        Sorry! you lost. <a class="btn btn-try-again" href="" @click.prevent.stop="reset">Try again?</a>
      </div>
      <div v-if="won" class="try-again-block won">
        Yay! you won. <a class="btn btn-try-again" href="" @click.prevent.stop="reset">Try again?</a>
      </div>
    </div>
    <div v-if="board" class="board-wrap" :style="styleObject">
      <spot 
        v-for="(s, index) in board.spots" 
        :ref="'spot' + index"
        :key="index" 
        :id="index"
        :spotObject="s"
        :board="board"
        @spotClicked="spotClicked"
        @spotMarkedSafe="spotMarkedSafe">
        
      </spot>
    </div>
  </div>
</template>

<script>
import { Board } from './board';
import Spot from './Spot';
export default {

  name: 'Minesweeper',

  data () {
    return {
      board: null,
      spotColCount: 30,
      spotRowCount: 15,
      mineCount: 20, 
      lost: false,
      won: false,
    };
  },
  computed: {
    styleObject() {
      const spotsize = 30;
      return {
        width: (spotsize * this.spotColCount) + 'px',
        height: (spotsize * this.spotRowCount) + 'px',
        'margin-left': -(spotsize * this.spotColCount)/2 + 'px',
      }
    }
  },

  methods: {
    
    spotClicked(spotId) {
      console.log(spotId);
      this.board.clearSpot(spotId);
      if(this.board.lost) {
        //game is lost, do you want to play again?
        this.board.clearAll();
        this.lost = true;
      }else if(this.board.won()){
        this.board.clearAll();
        this.won = true;
      }
    },
    spotMarkedSafe(spotId) {

    },
    resetSpots() {
      for (const [index, spot] of this.board.spots.entries()) {
        this.$refs['spot'+index][0].reset();
      }
    },
    reset() {
      this.won = false;
      this.lost = false;
      if(this.board)
        this.resetSpots();
      this.board = new Board(this.spotColCount, this.spotRowCount, this.mineCount);  
    }
  },

  mounted() {
    this.reset();
  },

  components: {
    spot: Spot,
  }
};
</script>

<style lang="css" scoped>
.heading{
  margin-bottom:  20px;
  margin-left:  50px;
  margin-right:  50px;
  color:  #4b5768;
  height:  100px;
}
.try-again-block{
  padding:  15px;
  color:  white;
  font-weight: bold;
  border-radius: 5px;
}
.try-again-block.lost{
  background-color: #E26F57;
}
.try-again-block.won{
  background-color: #75B968;
}
.board-wrap{
  box-shadow: 0px 0px 20px #bbb;
  padding: 20px;
  position:  absolute;
  left:  50%;
}
.btn.btn-try-again{
  padding:  5px 15px;
  border:  1px solid white;
  border-radius: 3px;
  text-decoration: none;
  font-weight: bold;
  color:  white;
  -webkit-transition: all 0.3s;
  -o-transition: all 0.3s;
  transition: all 0.3s;
}
.btn.btn-try-again:hover{
  background-color: white;
  color: #E26F57;
}
</style>