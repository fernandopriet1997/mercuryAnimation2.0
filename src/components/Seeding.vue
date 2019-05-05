<template>	   
	<div>
			<v-tabs
	    centered
	    color="grey"
	    dark
	    icons-and-text
	  >
	    <v-tabs-slider color="black"></v-tabs-slider>

	    <v-tab href="#tab-1">
	      Caras
	      <v-icon>fas fa-grin-beam</v-icon>
	    </v-tab>

	    <v-tab href="#tab-2">
	      Piernas
	      <v-icon>fas fa-drumstick-bite</v-icon>
	    </v-tab>

	    <v-tab href="#tab-3">
	      Manos
	      <v-icon>fas fa-hands</v-icon>
	    </v-tab>

			<v-tab href="#tab-4">
	      Paisajes
	      <v-icon>fas fa-mountain</v-icon>
	    </v-tab>
	    <v-tab href="#tab-5">
	      Otros
	      <v-icon>fas fa-box-open</v-icon>
	    </v-tab>
	    <v-tab-item v-for="(item, key) in resources"
	      :value="'tab-' + (key + 1)" :key="key"
	    >
	      <v-card flat>
	        <v-card-text>
						<v-layout row wrap>
						  <v-flex xs1 v-for="img in item.data">
						    <v-btn fab small @click="copyData(img.value)" color="success">{{ img.value }}</v-btn>
				      </v-flex>
						</v-layout>
	        </v-card-text>
	      </v-card>
	    </v-tab-item>
	  </v-tabs>
	  <input type="hidden" id="testing-code" :value="copyValu">
	  <v-snackbar
	    v-model="showTooltip.active"
	  >
	    {{ showTooltip.message }}
	    <v-btn flat color="primary" @click.native="showTooltip.active = false">Cerrar</v-btn>
	  </v-snackbar>
	</div>
</template>
<script>
	export default{
		created() {

		},
		mounthed(){

		},
		props: [
			
		],
		data(){
			return {
				tabs: '',
				copyValu: '',
				showTooltip:{
					message: '',
					active: false,
				},
				resources: [
					{
						title: 'Caras',
						active: true,
						data: [
							{title: '1', value: 'ͽ ͼ'},
							{title: '2', value: ' ͌°  ͌°'},
							{title: '2', value: ' ̽°̽  ̽°̽ 	'},
							{title: '2', value: ' ° ̅  ° ̅ '},
							{title: '2', value: ' ̅ ° ° ̅ '},
							{title: '2', value: ' ̅ ° ̅ °'},
							{title: '2', value: '¯ ¯'},
							{title: '2', value: ' ̃  ̃  '},
							{title: '2', value: ' ʘ ʘ'},
							{title: '2', value: ' ɤ ɤ'},
							{title: '2', value: ' ಥ ಥ '},
							{title: '3', value: '  ̫ '},
							{title: '2', value: '  ̫˅ '},
							{title: '2', value: ' ˅'},
							{title: '2', value: ' ˄'},
							{title: '2', value: ' ˃'},
							{title: '2', value: ' <'},
							{title: '2', value: ' ='},
							{title: '2', value: ' ɯ'},
							{title: '3', value: '０'},
							{title: '2', value: '(°<°)'},
							{title: '2', value: '(°>°)'},
							{title: '2', value: '(°>¯)'},
							{title: '2', value: '(¯>°)'},
							{title: '2', value: '(¯<°)'},
							{title: '2', value: '(°<¯)'},
							{title: '2', value: '(°-°)'},
							{title: '2', value: '(°_°)'},
							{title: '2', value: '(°=°)'},
							{title: '2', value: '(°˄°)'},
							{title: '2', value: '(°˅°)'},
							{title: '2', value: '(°０°)'},
							{title: '2', value: '(°o°)'},
							{title: '2', value: '(°O°)'},
							{title: '2', value: '(°ɯ°)'},
							{title: '2', value: '(°v°)'},
							{title: '2', value: '(°u°)'},
						],
					},
					{
						title: 'Piernas',
						active: false,
						data: [
							{title: '', value: '|¯¯¯|'},
							{title: '', value: '|¯¯¯\\'},
							{title: '', value: '|¯¯¯)'},
							{title: '', value: '\\¯¯¯)'},
							{title: '', value: '\\¯¯¯|'},
							{title: '', value: ')¯¯¯|'},
						],
					},
					{
						title: 'Brazos',
						active: false,
						data: [
							{title: '', value: '┘'},
							{title: '', value: '┌'},
							{title: '', value: '─'},
							{title: '', value: '┐'},
							{title: '3', value: '└'},
							
							{title: '', value: '╝'},
							{title: '', value: '╔'},
							{title: '', value: '═'},
							{title: '', value: '╗'},
							{title: '3', value: '╚'},

							{title: '', value: '╛'},
							{title: '', value: '╒'},
							{title: '', value: '='},
							{title: '', value: '╕'},
							{title: '3', value: '╘'},

							{title: '', value: '╜'},
							{title: '', value: '╓'},
							{title: '', value: '─'},
							{title: '', value: '╖'},
							{title: '3', value: '╙'},

							{title: '', value: '٩'},
							{title: '', value: '۶'},
							{title: '', value: '/'},
							{title: '', value: '\\'},
							{title: '', value: '⌡'},
							{title: '', value: '⌠'},
							{title: '', value: '|'},
							{title: '3', value: '║'},

						],
					},
					{
						title: 'Paisaje',
						active: false,
						data: [
							{title: '', value: '☁'},
							{title: '', value: '★'},
							{title: '', value: '☆'},
							{title: '', value: '☾'},
							{title: '', value: '░'},
							{title: '', value: '▓'},
							{title: '', value: '▄'},
						],
					},
					{
						title: 'Otros',
						active: false,
						data: [
							{title: '', value: '<º))))><'},
							{title: '', value: '><((((º>'},
							{title: '', value: 'lıllı ((((̲̅̅●̲̲̅̅̅̅=̲̲̅̅̅̅●̲̅̅)))) ıllı '},
						],
					}
				],
			
			}
		},
		methods: {
			copyData(value){
				this.copyValu = value;
				this.copyTestingCode()
			 },
			 copyTestingCode () {

          let testingCodeToCopy = document.querySelector('#testing-code')
          testingCodeToCopy.setAttribute('type', 'text')    // 不是 hidden 才能複製
          testingCodeToCopy.select()

          try {
            var successful = document.execCommand('copy');
            var msg = successful ? 'successful' : 'unsuccessful';
            this.showTooltip.active = true;
            this.showTooltip.message = "Elemento copiado"
          } catch (err) {
            this.showTooltip.active = true;
            this.showTooltip.message = "Algo salio mal!"
          }

          /* unselect the range */
          testingCodeToCopy.setAttribute('type', 'hidden')
          window.getSelection().removeAllRanges()
        },
		},
		watch: {

		},
		computed: {

		},
	}
</script>