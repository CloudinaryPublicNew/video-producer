<template>
  <div class="track">
    <v-btn dark small fab color="primary" @click="generateURLv3">
      <v-icon>movie</v-icon></v-btn>
    <v-layout align-center justify-space-around>
      <v-layout row wrap class="trackGrid">
  
      <v-flex xs3 v-for="clip in clips" :key="clip.asset_info.public_id" draggable>
        <a @click="previewAsset(clip)">
          <img :src="createThumbnailImage(clip.asset_info.secure_url)" :alt="clip.asset_info.public_id" :title="clip.asset_info.public_id">
        </a>
      </v-flex>
      </v-layout>



    </v-layout>
  </div>
  
</template>

<script>
export default {
  name: 'storyboard',
  props: ['editedClip'],
  data: () => ({
    clips:[]
  }),
  mounted() {
    
  },
  watch:{
    editedClip(newData, oldData){
      this.clips.push(newData);
    },

  },
  methods: {

    createThumbnailImage(secure_url) {
      const url =  secure_url.replace('.mp4','.png').replace('.jpg','.png').
      replace('.mov','.png').replace('upload/','upload/w_160,h_90,c_thumb,ar_16:9,r_8/');
      return url;
    },
    previewAsset(clip) {
      console.log(clip);
      this.$emit('preview-click', clip);
    },

    generateURLv3 (){

      const video1 = new Object();
      const video2 = new Object();

      console.log('>>>> ', this.clips[0]);

      video1_public_id = this.clips[0].asset_info.public_id;
      video2_public_id = this.clips[1].asset_info.public_id;

      // const clip0_option = this.clips[0].transformations;
      // console.log(clip0_option);

      const transformations = [];
      // const clip0_public_id = this.clips[0].asset_info.public_id;
      // const clip0_option = this.clips[0].transformations;
      // console.log(clip0_option);

      const cl = new cloudinary.Cloudinary({ cloud_name: 'de-demo' });
      const url = cl.video(video1_public_id, {
        transformation: [
            {width: 300, height: 200, crop: "fill"},
            {overlay: `video:${video2_public_id}`, flags: "splice", width: 300, height: 200, crop: "fill"}
            ]})


      // const url = cl.url(clip0_public_id, clip0_option);
       console.log(url);
       window.open(url, '_blank');

    },

    generateURLv2(){

      const transformations = [];
      const video1 = {};
      const video2 = {};

      console.log('>>>> ', this.clips[0]);

      const video1.public_id = this.clips[0].asset_info.public_id;
      const clip0_option = this.clips[0].transformations;
      console.log(clip0_option);

      let nextSO = 0;

      function = build

       let transArr =  this.clips.forEach((clip) => { 
      console.log(clip)
      const layer = clip.asset_info.public_id.replace(/\//g,":");
      let vlayer = `video:${layer}`


    let line1 =  {overlay: vlayer, startOffset: clip.trim_info.start_offset, endOffset: clip.trim_info.end_offset};
    let line2 =  {startOffset: "0", flags: ["layer_apply", "splice"], width: 400, crop: "scale"};

    const nextSO = (clip.trim_info.end_offset - clip.trim_info.start_offset)


      const tranform = { 
                        width:400, 
                        overlay: vlayer, 
                        startOffset:clip.trim_info.start_offset,  
                        endOffset:clip.trim_info.end_offset 
                      }
            .push(tranform);
  });




      const cl = new cloudinary.Cloudinary({ cloud_name: 'de-demo' });

      const options =  {
          transformation: [
          {width: 400, duration: "12", crop: "scale"},
          {overlay: "video:MLB2", startOffset: "2", endOffset: "6"},
          {startOffset: "0", flags: ["layer_apply", "splice"], width: 400, crop: "scale"},
          {overlay: "video:MLB2", startOffset: "20", endOffset: "24"},
          {startOffset: "4", flags: ["layer_apply", "splice"], width: 400, crop: "scale"},
          {overlay: "video:MLB2", startOffset: "31", endOffset: "35"},
          {startOffset: "8", flags: ["layer_apply", "splice"], width: 400, crop: "scale"},
          {audioCodec: "none"}
          ]}

      cl.url('clip0_public_id', options);
       console.log(url);
       window.open(url, '_blank');
    },

    generateURL() {
      
      const cl = new cloudinary.Cloudinary({ cloud_name: 'de-demo' });

      const transformations = [];
      const clip0_public_id = this.clips[0].asset_info.public_id;
      const clip0_option = this.clips[0].transformations;
      console.log(clip0_option);

      
  let t = [];
  
  let transArr =  this.clips.forEach((clip) => { 
  // if(i === 0){
        //     return;
        // }
      console.log(clip)
      const layer = clip.asset_info.public_id.replace(/\//g,":");
      let vlayer = `video:${layer}`
      const tranform = { 
                        width:400, 
                        overlay: vlayer, 
                        startOffset:clip.trim_info.start_offset,  
                        endOffset:clip.trim_info.end_offset 
                      }
                  // if(i === this.clips.length -1){
                  //   tranform.flags = "splice"
                  // }
            t.push(tranform);
  });

  console.log(`index `, t);      
      clip0_option.transformation = t;
      clip0_option.width = '400';
      clip0_option.crop = 'fill';
        

       const url = cl.url(clip0_public_id, clip0_option);
       console.log(url);
       window.open(url, '_blank');
               
    },
  

  }

};
</script>

<style>

.track {
  display: inline-block;
  text-align: center;
  vertical-align: center;
  float: left;
  border: dashed 2px #0071BA;
  min-height: 90px;
  min-width: 98vw;
}

</style>