<template>
  <div class="playlist">
    <div class="header">
      <div class="title">{{ decode(title) }}</div>
      <div class="subtitle">{{ decode(subtitle) }}</div>
    </div>
    <div class="cards" ref="cards">
      <playlist-card v-for="card in cards" :id="card" :playlistid="playlistid" :key="card"></playlist-card>
    </div>
    <div class="arrow" :v-if="rightArrow"><i class="fa fa-chevron-right" aria-hidden="true"></i></div>
  </div>
</template>

<script>
import PlaylistCard from './PlaylistCard.vue';

export default {
  components: {
    'playlist-card': PlaylistCard
  },
  props: {
    title:{
      type: String
    },
    subtitle:{
      type: String
    },
    cards:{
      type: Array
    },
    playlistid:{
      type: Number
    }
  },
  data () {
    return {
    }
  },
  computed:{
    rightArrow() {
      if(this.$refs.cards !== undefined)
        return this.$refs.cards.scrollWidth> tthis.$refs.cards.scrollLeft + this.$refs.cards.getBoundingClientRect().width
      else return true;
    }
  },
  methods:{
    decode : function(str) {
      return str.replace(/&#(\d+);/g, function(match, dec) {
        return String.fromCharCode(dec);
      });
    }
  },
  mounted() {
    this.$refs.cards.addEventListener('wheel', (e) => {
      e.currentTarget.scrollLeft -= e.wheelDelta/5;
      e.preventDefault();
    });
  }//,
  // beforeDestroy() {
  //   this.$refs.cards.removeEventListener('wheel');
  // }
}
</script>

<style lang="scss" scoped>
@import "./src/styles/settings.scss";

.playlist{
  padding: 30px;
  &:nth-child(2n){
    background-color: lighten($main,35%);
  }

  .header{
    
    .title{
      font-size: 1.5em;
      font-weight: 300;
      color: $back;
      .trash{
        background-color: $back;
        color: lighten($main,35%);
        padding: 10px;
        font-size: .5em;
        position: absolute;
        right: 30px;
        display: none;
        border: 0;
        cursor: pointer;
      }
    }
    .subtitle{
      margin-top: 3px;
      font-size: .9em;
      font-weight: 400;
      color: lighten($back, 20%);
    }
  }
  .arrow{
    color: $back;
    position: absolute;
    right: 10px;
    margin-top: -100px;
  }
  .cards{
    display: flex;
    overflow-x: auto;
    padding: 15px 0;
    &::-webkit-scrollbar { 
      display: none;
    }

  }

}
</style>