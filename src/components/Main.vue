 <template>     
  <v-container>
    <v-toolbar>
      <v-toolbar-side-icon></v-toolbar-side-icon>
      <v-toolbar-title>Herramientas</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-menu offset-y>
          <template v-slot:activator="{ on }">
            <v-btn
              color="primary"
              flat
              v-on="on"
            >
              Orientación
            </v-btn>
          </template>
          <v-list>
            <v-list-tile @click="orientation = 'h'">
              <v-list-tile-title >Horizontal</v-list-tile-title>
            </v-list-tile>
            <v-list-tile @click="orientation = 'v'">
              <v-list-tile-title >Vertical</v-list-tile-title>
            </v-list-tile>
          </v-list>
        </v-menu>
        <v-menu offset-y>
          <template v-slot:activator="{ on }">
            <v-btn
              color="primary"
              flat
              v-on="on"
            >
              Tipo de pantalla
            </v-btn>
          </template>
          <v-list>
            <v-list-tile @click="setTv(1)">
              <v-list-tile-title >Tipo 1</v-list-tile-title>
            </v-list-tile>
            <v-list-tile @click="setTv(2)">
              <v-list-tile-title >Tipo 2</v-list-tile-title>
            </v-list-tile>
          </v-list>
        </v-menu>
        <v-btn flat>Link Three</v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <v-toolbar >
      <v-btn-toggle >
        <v-btn flat v-for="cbutton in toolbarbuttons" @click="handlerActions(cbutton)">
          <v-icon>{{ cbutton.icon }}</v-icon>
        </v-btn>
        <v-btn flat >
          {{ numberFotogram }}/{{ animation.length }}
        </v-btn>
      </v-btn-toggle>
      <v-divider vertical></v-divider>
      <v-toolbar-items>
        <v-text-field
          name="Frames x segundo" @click:append="setTime()"
          label="Frames x segundo" type="number" v-model="fps"
          id="id" append-icon="fas fa-stopwatch" :hint="String(interval.time)"
        ></v-text-field>
        <v-divider vertical></v-divider>
        <v-divider vertical></v-divider>
        <v-divider vertical></v-divider>
        <v-select
          :items="opacityValues"
          v-model="prevFotogram.opacity"
          label="Opacidad"
        ></v-select>
      </v-toolbar-items>
    </v-toolbar>
    <v-slider v-model="numberFotogram" @change="setFotogram(numberFotogram)" step="1" :max="animation.length"></v-slider>
    <v-card>
      <v-layout row wrap>

        <v-flex :xs12="orientation == 'h'" :xs6="orientation == 'v'" pa-1 pd-1> 
          <div class="container-render">
            <div :class="prevFotoClass" 
              :style="'opacity:' + prevFotogram.opacity" 
              v-html="prevFotogram.fotogram" 
              v-if="prevFotogram.active"
              ></div>
            <div :class="renderClass" 
              :style="renderStyle" 
              v-html="currentFotogram"></div>
          </div>
        </v-flex>
        <v-flex :xs12="orientation == 'h'" :xs6="orientation == 'v'" pa-1 pd-1>

          <v-textarea
            label="" solo 
            ref="textEditor"  reverse @keyup.ctrl.enter="saveFotogram()"
            textarea v-model="currentFotogram"
          ></v-textarea>
        </v-flex>
      </v-layout>
    </v-card>
    <v-speed-dial
      v-model="fab"
      bottom
      right
      direction="top"
      :open-on-hover="true"
      fixed
    >
      <template v-slot:activator>
        <v-btn
          v-model="fab"
          color="blue darken-2"
          dark
          fab
        >
          <v-icon>palette</v-icon>
          <v-icon>close</v-icon>
        </v-btn>
      </template>
      <v-btn
        fab
        dark
        small
        color="green"
      >
        <v-icon>edit</v-icon>
      </v-btn>
      <v-btn
        fab
        dark
        small
        color="indigo"
      >
        <v-icon>add</v-icon>
      </v-btn>
      <v-btn
        fab
        dark
        small
        color="red"
      >
        <v-icon>delete</v-icon>
      </v-btn>
    </v-speed-dial>
    <v-dialog
      v-model="seedingModal"
      scrollable  
      :overlay="false"
      max-width="950px"
      transition="dialog-transition"
    >
      <v-card>
        <v-card-text>
          <Seeding/>   
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-snackbar
      v-model="saveSnack"
    >
      Frame Guardado exitosamente
      <v-btn flat color="primary" @click.native="saveSnack = false">Close</v-btn>
    </v-snackbar>
    <v-btn color="success" @click="testAxios()">APUCHARRALE WE</v-btn>
    <v-dialog
      v-model="modal"
      scrollable fullscreen 
      persistent :overlay="false"
      max-width="500px"
      transition="dialog-transition"
    >
      <v-card>
        {{ response }}
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import Seeding from './Seeding.vue'
  export default{
    components: {
      Seeding
    },
    created() {

    },
    mounted(){

    },
    props: [
      
    ],
    data(){
      return {
        modal: false,
        seedingModal: false,
        saveSnack: false,
        orientation: 'v',
        currentFotogram: '',
        renderStyle: '',
        prevRenderStyle: 'opacity: 0.5;',
        fab: false,
        inputValue: 0,
        position: 0,
        toolbarbuttons: [
          {
            icon: 'fas fa-undo',
            action: 'undo'
          },
          {
            icon: 'fas fa-eye',
            action: 'eye'
          },
          {
            icon: 'fas fa-fast-backward',
            action: 'firstFotogram'
          },
          {
            icon: 'fas fa-backward',
            action: 'prevFotogram'
          },
          {
            icon: 'fas fa-save',
            action: 'save'
          },
          {
            icon: 'fas fa-play',
            action: 'play',
            metadata: 'play'
          },
          {
            icon: 'fas fa-stop',
            action: 'stop'
          },
          {
            icon: 'fas fa-trash',
            action:'delete'
          },
          {
            icon: 'fas fa-forward',
            action: 'nextFotogram'
          },
          {
            icon: 'fas fa-fast-forward',
            action: 'lastFotogram'
          },
          {
            icon: 'fas fa-expand',
            action: 'expand'
          },
          {
            icon: 'fas fa-seedling',
            action: 'seed'
          },
          {
            icon: 'fas fa-code',
            action: 'code'
          },
          {
            icon: 'fas fa-history',
            action: 'lastView'
          }
        ],
        prevFotogram: {
          active: true,
          opacity: 0.5,
          fotogram: ''
        },
        opacityValues: [0.0,0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1],
        animation: [],
        interval: {
          active: {},
          time: 1000
        },
        numberFotogram: 0,
        deleteLastFotogram: true,
        fps: '',
        prevFotoClass: 'opacityPrevFoto-1',
        renderClass: 'render-1',
      }
    },
    methods: {
      testAxios(){
        axios.get('https://lpdaacsvc.cr.usgs.gov/appeears/api/product').then((res) => {
          this.response = res.data;
          this.modal = true;
        }).carch((error) => {
          this.response = error
        })
      }
      addTab(){
        this.currentFotogram = this.currentFotogram * "\t";
      },
      setTime(){
        let time = 1000 / this.fps;
        this.interval.time = Math.round(time);
      },
      setTv(type){
        this.prevFotoClass = 'opacityPrevFoto-' + type;
        this.renderClass = 'render-' + type;
      },
      insertTab(){
        console.log("Ejecutnado")
        this.currentFotogram = this.currentFotogram + "\t";
        this.$refs.textEditor.focus();
      },
      inputValues(e){
        this.inputValue = e
        this.position = this.$refs.textEditor.selectionStart;
      },
      playAnimation(){
        if(this.numberFotogram < this.animation.length){
          this.currentFotogram = this.animation[this.numberFotogram];
          this.numberFotogram++;
        }else{
          this.pauseAnimation(this.animation.length - 1);
          this.toolbarbuttons[5].icon = 'fas fa-play'
        }
      },
      pauseAnimation(fotogram){
        clearInterval(this.interval.active);
        this.prevFotogram.active = true;
        this.currentFotogram = this.animation[fotogram];
      },
      stopAnimation(){
        clearInterval(this.interval.active);
        this.prevFotogram.active = true;  
        this.numberFotogram = 0;
        this.currentFotogram = this.animation[0];
        this.toolbarbuttons[5].icon = 'fas fa-play'  
      },
      switchAnimation(button){
        if(button.metadata == 'play'){
          button.metadata = 'stop';
          button.icon = 'fas fa-pause'
          this.prevFotogram.active = false;
          this.interval.active = window.setInterval(function(){this.playAnimation();}.bind(this), this.interval.time);
        }else{
          button.metadata = 'play';
          button.icon = 'fas fa-play';
          this.prevFotogram.active = true;
          this.pauseAnimation(this.numberFotogram);
        }
      },
      setFotogram(fotogram){
        if(fotogram >= 0 && fotogram <= this.animation.length){
          this.numberFotogram = fotogram;
          this.currentFotogram = this.animation[fotogram];
        }
      },
      saveFotogram(){
        this.animation.push(this.currentFotogram);
        this.numberFotogram = this.animation.length;
        this.prevFotogram.fotogram = this.animation[this.animation.length - 1];
        if(this.deleteLastFotogram){
          this.currentFotogram = '';
        }
        this.saveSnack = true;
      },
      deleteFotogram(){
        this.animation.splice(this.numberFotogram, 1);
      },
      handlerActions(action){
        if(action.action == 'play'){
          this.switchAnimation(action);
        }else if(action.action == 'save'){
          this.saveFotogram()
        }else if(action.action == 'stop'){
          this.stopAnimation();
        }else if(action.action == 'undo'){
          this.currentFotogram = this.animation[this.animation.length - 1];
        }else if(action.action == 'eye'){
          this.prevFotogram.fotogram = this.animation[this.numberFotogramñ];
        }else if(action.action == 'firstFotogram'){
          this.setFotogram(0);
        }else if(action.action == 'prevFotogram'){
          let foto = this.numberFotogram - 1
          this.setFotogram(foto);
        }else if(action.action == 'nextFotogram'){
          let foto = this.numberFotogram + 1
          this.setFotogram(foto);
        }else if(action.action == 'lastFotogram'){
          this.setFotogram(this.animation.length - 1);
        }else if(action.action == 'delete'){
          this.deleteFotogram();
        }else if(action.action == 'lastView'){
          this.deleteLastFotogram = !this.deleteLastFotogram;
        }else if(action.action == 'seed'){
          this.seedingModal = true;
        }
      }
    },
    watch: {

    },
    computed: {
     
    },
  }
</script>
<style>
  .container-render {
    padding: 20px 0;
    position: relative;

  }
  .render-1 {
    height: 400px;
    width: 800px;
    margin: auto;
    z-index: 50;
    border: solid 1px;
    word-break: break-all;
  }

  .opacityPrevFoto-1 {
    height: 400px;
    width: 800px;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    position: absolute;
    border: solid 1px transparent;
    word-break: break-all;

  }
  .render-2 {
    height: 400px;
    width: 400px;
    margin: auto;
    z-index: 50;
    border: solid 1px;
    word-break: break-all;
  }

  .opacityPrevFoto-2 {
    height: 400px;
    width: 400px;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    position: absolute;
    border: solid 1px transparent;
    word-break: break-all;
  }
</style>