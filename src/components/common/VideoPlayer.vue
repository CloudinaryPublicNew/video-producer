<template>
<div class="video-container">
  <v-layout align-center justify-center-around>
    <video class="playerContainer" ref="videoplayer" v-on:timeupdate="updateDisplay($event)" controls="yes" name="media" id="videoPlayerPreview">
    </video>
  </v-layout>

  <v-toolbar class="playerContainer" dense dark color="secondary">
    <v-btn dark small fab color="primary" @click="trimStart" class="rotateLeft">
	      <v-icon>vertical_align_bottom</v-icon>
	  </v-btn>
		<v-spacer></v-spacer>
  	<div class="caption">
  		<span v-text="playheadPosition"></span> of <span v-text="duration"></span>
 	</div>
 	<v-spacer></v-spacer>

	  
	   <v-btn dark small fab color="primary" @click="trimEnd" class="rotateRight">
	      <v-icon>vertical_align_top</v-icon>
	  </v-btn>
  </v-toolbar> 
  <v-container align-center justify-space-around>
    <v-btn dark small fab color="primary" @click="previewClip">
      <v-icon>play_circle_outline</v-icon>
    </v-btn>
    <v-btn dark small fab color="primary" @click="generateURL">
      <v-icon>add_to_queue</v-icon>
    </v-btn>
    <v-btn dark small fab color="success" @click="openClip">
      <v-icon>open_in_new</v-icon>
    </v-btn>
	</v-container>

</div>
</template>

<script>
export default {
  props: ['source', 'clip'],
  name: 'video-player',
  data: () =>({
      playheadPosition: 0,
      duration: 0,
      startoffset: 0,
      endoffset: 0,
      src: null,
      clips: [],
      currentClip: {}
  }),

  methods: {
  	updateDisplay(event) {
  		this.playheadPosition = event.target.currentTime;
      this.duration = event.target.duration;
      this.currentClip.trim_info.duration = event.target.duration;
    },
    /*
    10 sec
    @2
    duration: 8
    startoffset: 2
    */
  	trimStart(event) {
     // this.duration = parseFloat(this.duration - this.playheadPosition).toFixed(2);
      this.startoffset = parseFloat(this.playheadPosition).toFixed(2);
      this.endoffset = parseFloat(this.duration + this.playheadPosition).toFixed(2);
    },
    /*
    10 sec
    @2
    duration: 8
    endoffset: 2
    */
   
  	trimEnd(event) {
      if (this.startoffset === 0) {
        // this.duration = parseFloat(this.duration - this.playheadPosition).toFixed(2);
        this.endoffset = parseFloat(this.playheadPosition).toFixed(2);
        /*
        10
        start: 2
        end: 2
        duration: 6
        endoffset: 2
        */
      } else {
        // this.duration = parseFloat(this.duration - this.playheadPosition - this.startoffset).toFixed(2);
        this.endoffset = parseFloat(this.playheadPosition).toFixed(2);
        //this.duration = parseFloat(this.endoffset - this.startoffset).toFixed(2);
      }
    },
    generateURL() {
      this.currentClip.trim_info.start_offset = this.startoffset;
      this.currentClip.trim_info.end_offset = this.endoffset;
    
      const cl = new cloudinary.Cloudinary({ cloud_name: 'de-demo' });
      const public_id = this.currentClip.asset_info.public_id;
      const options = {
        endOffset: this.endoffset,
        startOffset: this.startoffset,
        resource_type: 'video',
        format: 'mp4',
        // width: 400,
        crop: 'fill',
        flags: 'splice'
      };

      const url = cl.url(public_id, options);

      this.currentClip.transformations = options; //[];
     // this.currentClip.transformations.push(options);
      this.currentClip.transformationVideoURL = url;
      console.log('ADD TO TRACK', this.currentClip);
      this.$emit('add-track', this.currentClip);            
    },
  	previewClip(event) {
      const player = this.$refs.videoplayer;
      player.src = `${this.currentClip.video_url}#t=${this.startoffset},${this.endoffset}`;
      player.play();
    },
    openClip() {
      this.generateURL();
      const url = this.currentClip.transformationVideoURL;
      console.log(url);
      window.open(url, '_blank');
    }
  
  },
  
  watch: {
    source: function(newUrl, oldUrl) {
      console.log('Watching:', newUrl, oldUrl);
      this.$refs.videoplayer.src = newUrl;

    },
    clip: function(newClip, oldClip) {
      this.currentClip = newClip;
    }
  },

  mounted() {
    this.seektime = 0;
    this.duration = 0;
  }

};

</script>
   
<style>
 .rotateLeft{
  -webkit-transform: rotate(-90deg);
    -moz-transform: rotate(-90deg);
    -o-transform: rotate(-90deg);
    -ms-transform: rotate(-90deg);
    transform: rotate(-90deg);
}

 .rotateRight{
  -webkit-transform: rotate(-90deg);
    -moz-transform: rotate(-90deg);
    -o-transform: rotate(-90deg);
    -ms-transform: rotate(-90deg);
    transform: rotate(-90deg);
}
.playerContainer {
  width: 400px;
}

</style>