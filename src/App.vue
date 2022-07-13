<script>
  export default {
    data() {
      return {
        generalTime: 5,
        pause: false,
        upTimer: {
          Time: {
            minutes: undefined,
            seconds: 0
          },
          style:[
            'linear-gradient(to top, #1D976C, #93F9B9)'
          ,
            '#111'
          ],
          Flag: false,
          styleStatus: 1
        },
        downTimer: {
          Time: {
            minutes: undefined,
            seconds: 0
          },
          style:[
            'linear-gradient(to top, #1D976C, #93F9B9)'
          ,
            '#111'
          ],
          Flag: false,
          styleStatus: 1
        }
      }
    },
    methods: {
      reStart(){
        this.upTimer.Time.minutes = this.generalTime;
        this.upTimer.Time.seconds = 0;
        this.downTimer.Time.minutes = this.generalTime;
        this.downTimer.Time.seconds = 0;
        this.upTimer.Flag = false;
        this.downTimer.Flag = false;
        this.upTimer.styleStatus = 1;
        this.downTimer.styleStatus = 1;
      }
    },
    mounted() {
      this.upTimer.Time.minutes = this.generalTime;
      this.downTimer.Time.minutes = this.generalTime;

      this.interval = setInterval(() => {

        if (
          !this.pause && 
          (this.upTimer.Time.minutes != 0 || this.upTimer.Time.seconds != 0 ) && 
          (this.downTimer.Time.minutes != 0 || this.downTimer.Time.seconds != 0)
        ){
          if (this.upTimer.Time.seconds == 0){
            this.upTimer.Time.minutes -= 1;
            this.upTimer.Time.seconds = 60
          }

          if (this.downTimer.Time.seconds == 0){
            this.downTimer.Time.minutes -= 1;
            this.downTimer.Time.seconds = 60
          }

          if ( this.upTimer.Flag ){
              this.upTimer.Time.seconds -= 1;
              this.upTimer.styleStatus = 0;
              this.downTimer.styleStatus = 1;
          }

          if ( this.downTimer.Flag ){
              this.downTimer.Time.seconds -= 1;
              this.upTimer.styleStatus = 1;
              this.downTimer.styleStatus = 0;
          }

        }
      }, 1000);
    }
  }

</script>

<template>
    <div class="timer"
      :style="{'background-image': upTimer.style[upTimer.styleStatus]}"
    >
      <h1 
      @click="downTimer.Flag = true; upTimer.Flag = false"
      >
      {{ upTimer.Time.minutes + ':' + ('0'+ upTimer.Time.seconds).slice(-2) }}
      </h1>
    </div>

    <div class="settings">
      <ion-icon name="play" v-show="pause" @click="pause = false"></ion-icon>
      <ion-icon name="pause" v-show="!pause" @click="pause = true"></ion-icon>
      <!-- <ion-icon name="cog"></ion-icon> -->
      <ion-icon name="refresh" @click="reStart()"></ion-icon>
    </div>
    
    <div class="timer"
      :style="{'background-image': downTimer.style[downTimer.styleStatus]}"
    >
      <h1 
      @click="downTimer.Flag = false; upTimer.Flag = true"
      >
      {{ downTimer.Time.minutes + ':' + ('0'+ downTimer.Time.seconds).slice(-2) }}
      </h1>
    </div>

</template>
