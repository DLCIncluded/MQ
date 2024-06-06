<script>
import noUiSlider from 'nouislider';
import wNumb from 'wnumb';
import { ref,computed } from 'vue';
import { RouterLink, RouterView } from 'vue-router';
import 'nouislider/dist/nouislider.css';
import { create, all } from "mathjs";
const math = create(all);
import questions from "./questions.json"
import answers from "./answers.json"
export default {
	data() {
		return {
			lang: "en",
			lang_options: [
				{ 
					"short": "en",
					"display": "English"
				},
				{ 
					"short": "pt",
					"display": "Portuguese"
				},
				{ 
					"short": "de",
					"display": "German"
				},
				{ 
					"short": "pl",
					"display": "Polish"
				},
				{ 
					"short": "rus",
					"display": "Russian"
				},
				{ 
					"short": "es",
					"display": "Spanish"
				},
				{ 
					"short": "fr",
					"display": "French"
				},
			],
			questions : questions,
			answers:answers,
			scorebox: false,
			errorbox: false,
			score: 0,
			avg: 0,
			scoreQuote: "",
			saveProgress:false,
			savedAt:'never',
			knav:false,
			progress: 0,
			point1: { x: 251, y: 337 }, 
			point5: { x: 1352, y: 337 },
		}
	},
	created:function(){
		this.loadValues() //Check for and load values if they were saved
		this.shuffleQuestions() 
	},
	methods: {
		saveValues(){ //save progress in browser memory
			if(this.saveProgress){
				localStorage.questions = JSON.stringify(this.questions)
				localStorage.saveProgress = true
				console.log("manual save")
				var today = new Date();

				var date = (today.getMonth()+1)  +'/'+ today.getDate()  +'/'+ today.getFullYear()

				var hours = today.getHours();
				var minutes = today.getMinutes();
				var seconds = today.getSeconds();
				var ampm = hours >= 12 ? 'pm' : 'am';
				hours = hours % 12;
				hours = hours ? hours : 12; // the hour '0' should be '12'
				minutes = minutes < 10 ? '0'+minutes : minutes; //add leading 0 to min/sec
				seconds = seconds < 10 ? '0'+seconds : seconds;
				var strTime = hours + ':' + minutes +':'+ seconds + ' ' + ampm;



				this.savedAt = date +" "+ strTime
				localStorage.savedAt = this.savedAt
			}
		},
		loadValues(){//if saved, go ahead and load to use, if not use the base questions
			if(localStorage.saveProgress){
				this.saveProgress = true
			}
			if(localStorage.questions){
				this.questions = JSON.parse(localStorage.questions)
			}			
			if(localStorage.savedAt){
				this.savedAt = localStorage.savedAt
			}
		},
		clearSavedData(){
			localStorage.clear()
			this.saveProgress = false
			this.savedAt = 'never'
		},
		shuffleQuestions(){
			if(!localStorage.questions){ //only shuffle if we're not pullling from stored questions
				this.questions = this.questions.sort(() => Math.random() - 0.5);
			}
		},
		checkscore() {
			this.errorbox=false

			let l = this.questions.length;
			for (var i = 0; i < l; i++) {
				var x = this.questions[i].val
				if (x === null) {
					this.errorbox=true
					console.log("found null")
					this.scoreQuote ="You must answer all questions before submitting, even if the question does not apply, please select N/A."
					return
				}
			}

			this.scorebox=true
			let num_questions = this.questions.length;
			
			console.log("num questions: "+num_questions)
			let score = 0;
			
			for (var i = 0; i < l; i++) {
				var x = parseInt(this.questions[i].val)
				if (x === 0) {
					num_questions=num_questions-1 //lower the number of questions to divide by if they use n/a - fixes #7
					// console.log("N/A choice - decrement num_questions, num_questions="+num_questions)
				}
				score = score + x
			}
			console.log("final num questions: "+num_questions)
			this.score = score
			this.avg = score / num_questions
			this.avg = this.avg.toFixed(2) //fixes #6
			console.log("score calc: "+score+ "/" +num_questions+"="+this.avg)

			//percentile math per #4 - user Pi2048 Thank you!
			var autism_mean = 4.15
			var autism_sd = 0.347
			var autismpercentile = math.ceil(100*(1 - math.erf((autism_mean - this.avg ) / (math.sqrt(2) * autism_sd))) / 2)

			var allistic_mean = 3.19
			var allistic_sd = 0.578
			var allisticpercentile = math.ceil(100*(1 - math.erf((allistic_mean - this.avg ) / (math.sqrt(2) * allistic_sd))) / 2)
			// this.scoreQuote ="This score falls in the "+autismpercentile+"th <a href='https://en.wikipedia.org/wiki/Percentile' target='_blank'>percentile</a> of the autistic population based on data from the initial validation study on the MQ."
			this.scoreQuote ="This score suggests that you are more Monotropic than about "+autismpercentile+"% of autistic people and about "+allisticpercentile+"% of allistic people based on data from the initial validation study."

			setTimeout(() => {
				this.drawArrow()
			}, 100);
			
			// console.log(this.avg)
			// console.log(this.scoreQuote)
			
			//for testing: var btns = document.querySelectorAll('[id$="5"]');for(var i=0;i<btns.length;i++){ btns[i].click(); }
			

			//removed slider for now, scoring issue #4
			// var slider = document.getElementById('slider');
			// if(!slider.noUiSlider){
			// 	noUiSlider.create(slider, {
			// 		start: [20,100, 211],
			// 		tooltips: [
			// 			{
			// 				to: function ( value ) {
			// 					if ((value > 190 && value <= 197)||(value > 144 && value <= 155)) 
			// 						return "<span class='score_slider move_up'>Your Avg="+(value/47).toFixed(2)+"</span>";
			// 					else if (value < 190 && value >= 155)
									
			// 						return "<span class='score_slider'>Your Avg="+(value/47).toFixed(2)+"</span>";
			// 					else 
			// 						return "<span class='score_slider'>Your Avg="+(value/47).toFixed(2)+"</span>";
			// 				}
			// 			},
			// 			{
			// 				to: function ( value ) {
			// 				return "<span class='under non_slider'>AL=~"+(value/47).toFixed(2)+"</span>";
			// 				}
			// 			},
			// 			{
			// 				to: function ( value ) {
			// 				return  "<span class='under aut_slider'>AU=~"+(value/47).toFixed(2)+"</span>";
			// 				}
			// 			}
			// 		],
			// 		behaviour: 'unconstrained-tap',
			// 		range: {
			// 			'min': 0,
			// 			'max': 235
			// 		},

			// 	});
		// 	} 
			
		// 	console.log(score)
		// 	slider.noUiSlider.set([score,'149.93', '195']);
		// 	slider.noUiSlider.disable();
		},
		updateProgress() {
			this.progress = this.completedQuestions;

		},
		calculateScorePosition() {
			const scoreRatio = (this.avg -1) /4; // Normalize score to range 0-1

			const x = this.point1.x + (this.point5.x - this.point1.x) * scoreRatio;
			const y = this.point1.y + (this.point5.y - this.point1.y) * scoreRatio;
			return { x, y };
		},
		drawArrow() {
			console.log("draw dot")
			const image = this.$refs.image;
			const arrow = this.$refs.arrow;

			const { scaleX, scaleY } = this.getScaleFactors(image);

			const scaledPoint1 = {
				x: this.point1.x * scaleX,
				y: this.point1.y * scaleY,
			};

			const scaledPoint5 = {
				x: this.point5.x * scaleX,
				y: this.point5.y * scaleY,
			};

			const scorePosition = this.calculateScorePosition(scaledPoint1, scaledPoint5);

			arrow.style.left = `${scorePosition.x}px`;
			arrow.style.top = `${scorePosition.y}px`;
		},
		getScaleFactors(image) {
			const naturalWidth = image.naturalWidth;
			const naturalHeight = image.naturalHeight;
			const clientWidth = image.clientWidth;
			const clientHeight = image.clientHeight;

			const scaleX = clientWidth / naturalWidth;
			const scaleY = clientHeight / naturalHeight;

			return { scaleX, scaleY };
		},
		calculateScorePosition(scaledPoint1, scaledPoint5) {
			const scoreRatio = (this.avg - 1) / 4;

			const scoreX = scaledPoint1.x + scoreRatio * (scaledPoint5.x - scaledPoint1.x);
			const scoreY = scaledPoint1.y + scoreRatio * (scaledPoint5.y - scaledPoint1.y);

			return { x: scoreX, y: scoreY };
		},
	},
	computed: {
		completedQuestions() {
			return this.questions.filter(item => item.val !== null).length;
		},
		progressWidth() {
			return `${(this.progress / this.questions.length) * 100}%`;
		},
	},
	mounted() {
		window.addEventListener('resize', this.drawArrow);
	},
	beforeDestroy() {
		window.removeEventListener('resize', this.handleResize);
	},

}

</script>

<template>
  <header>
    <!-- <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" /> -->

    <div class="wrapper">
	    <h1>Monotropism Questionnaire</h1>
		<p><strong>BEFORE YOU BEGIN!!! I am not a doctor</strong>. I am a bored webdev who thinks self-diagnosing is valid. This is not diagnosing anything; compare your scores to the results of the data from the initial validation study and continue to research. Thank you for the team involved in this study for putting together the questionnaire, and all those who have made suggestions and helped make this app better.</p> 
		
		<p>
			Questions taken directly from: <a href="https://osf.io/4wru2" target="_blank">https://osf.io/4wru2</a> and <a href="https://osf.io/g4kc9" target="_blank">scored based on their findings</a>.<br/>
			Original License info:  This questionnaire is published under a Creative Commons license, CC-BY-NC-SA.  Full text for this license can be found on the Creative Commons website <a href="https://creativecommons.org/licenses/by-nc-sa/2.0/" target="_blank">here</a>. 
		</p>
		<p>
			Original Credit: <a href="https://doi.org/10.17605/OSF.IO/WPX5G" target="_blank"><i>Garau, V., Woods, R., Chown, N., Hallett, S., Murray, F.,Wood, R.,Murray, A.& Fletcher-Watson, S. (2023). The Monotropism Questionnaire, Open Science Framework.</i> </a> This is a pre-print, meaning it is currently awaiting peer review.
		</p>

		<p>
			I <strong>do not own</strong> the questions, or content in the questions; I do not store or collect this information; please see the source code if you are concerned, it is <a href="https://github.com/DLCIncluded/MQ" target="_blank">open source</a>. <strong>All processing is done in <em>your</em> browser, in memory. No data will ever be sent to me, or anyone else.</strong> 
			If you have any questions, comments, or suggestions please go to the github repo <a href="https://github.com/DLCIncluded/MQ" target="_blank">here</a> and submit an issue, and I will be glad to assist! 
		</p>
		<p>
			All this site is for is to make taking the questionnaire easier, and I was bored, and too lazy to tally up the score myself... and yes I know that making this site was far more work than just doing a bit of adding... 
		</p>
			
		<p>
			If you have trouble finishing in one sitting, and would like to save your progress, please check this box. <br>
			NOTE: If you check this box, you are agreeing that it is okay to store your progress in your browser (think <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage" target="_blank" rel="noopener noreferrer">cookies</a>), this will only store in your browser's memory until you <a href="https://www.leadshook.com/help/how-to-clear-local-storage-in-google-chrome-browser/" target="_blank" rel="noopener noreferrer">clear local storage</a>, the site still will not collect, or store your data any where else.<br>


			<label for="save-progress">Accept and allow saving progress: </label><input type="checkbox" name="saveProgress" id="save-progress" v-model="saveProgress" > 
			<p v-if="saveProgress" class="savedBox">

				<button @click="saveValues" class='button green' :disabled='!saveProgress'>Save</button>
				
				<button @click="loadValues" class='button blue' :disabled='!saveProgress'>Load</button>

				<button class='button red' @click="clearSavedData" :disabled='!saveProgress'>Clear Saved Data</button>

				<p>Saved at: {{savedAt}}</p>
				
			</p>
			
			
		</p>
		<p>
			<a @click.prevent="knav= !knav" class="pointer">Keyboard Navigation Info</a>
			<ul v-if="knav">
				<li>Jump to the next or previous question using <strong>Tab</strong> or <strong>Shift+Tab</strong>.</li>
				<li>Select your answer using the arrow keys.</li>
				<li>To Submit - Either <strong>Enter</strong> or Select the button and <strong>Space Bar</strong></li>
			</ul>
			
		</p>
		<div>
		
			<div class='translations'>
				Community Translations <a href='https://github.com/DLCIncluded/MQ/wiki/Community-Translations' target='_blank'>ℹ️</a>:
				<!-- <ul>
					<li v-for="(l,index) in lang_options" :key="index">
						<input type="radio" name="lang" :id="'lang-'+l.short" v-model="lang" :value="l.short">
						<label class="lang" :for="'lang-'+l.short">{{l.display}}</label>
					</li>
				</ul>		 -->
				<select name="lang" id="lang" v-model="lang">
					<option v-for="(l,index) in lang_options" :key="index" :value="l.short">{{l.display}}</option>
				</select>
				<br/>
				<strong>Please note</strong>: these are community provided, I try my best to make sure they're correct however I only speak/read English, so if you notice anything incorrect please let me know!
			</div>
		</div>
    </div>
  </header>

	<div class="questions">

		<h2 class="qtitle">Questionnaire:</h2>
		<div v-for="(item,index) in questions" :key="index">
			<div class="question">
				<!-- {{ index+1 }}) -->
				{{item.question[lang]}} 

			</div>
			<div v-if="!item.reverse" class='selections'>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'0'" v-model="item.val" value="0" :checked="item.val == '0'" @change="updateProgress"><label :for="'r'+index+'0'">{{answers.na[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'1'" v-model="item.val" value="1" :checked="item.val == '1'" @change="updateProgress"><label :for="'r'+index+'1'">{{answers.sd[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'2'" v-model="item.val" value="2" :checked="item.val == '2'" @change="updateProgress"><label :for="'r'+index+'2'">{{answers.d[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'3'" v-model="item.val" value="3" :checked="item.val == '3'" @change="updateProgress"><label :for="'r'+index+'3'">{{answers.nad[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'4'" v-model="item.val" value="4" :checked="item.val == '4'" @change="updateProgress"><label :for="'r'+index+'4'">{{answers.a[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'5'" v-model="item.val" value="5" :checked="item.val == '5'" @change="updateProgress"><label :for="'r'+index+'5'">{{answers.sa[lang]}}</label></p>
			</div>
			<div v-else class='selections'>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'0'" v-model="item.val" value="0" :checked="item.val == '0'" @change="updateProgress"><label :for="'r'+index+'0'">{{answers.na[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'1'" v-model="item.val" value="5" :checked="item.val == '5'" @change="updateProgress"><label :for="'r'+index+'1'">{{answers.sd[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'2'" v-model="item.val" value="4" :checked="item.val == '4'" @change="updateProgress"><label :for="'r'+index+'2'">{{answers.d[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'3'" v-model="item.val" value="3" :checked="item.val == '3'" @change="updateProgress"><label :for="'r'+index+'3'">{{answers.nad[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'4'" v-model="item.val" value="2" :checked="item.val == '2'" @change="updateProgress"><label :for="'r'+index+'4'">{{answers.a[lang]}}</label></p>
				<p @click="updateSlider"><input type="radio" :name="index" :id="'r'+index+'5'" v-model="item.val" value="1" :checked="item.val == '1'" @change="updateProgress"><label :for="'r'+index+'5'">{{answers.sa[lang]}}</label></p>
			</div>
			
		</div>
		
		<div class="submit">
			<button @click="checkscore">Submit</button>
		</div>

		<div class="score_box" v-show="scorebox">

			<p>As a reminder this is an assessment for <a href="https://monotropism.org/" target="_blank" rel="noopener noreferrer">Monotropism</a>, a trait sometimes associated with Autism, not Autism directly. If you score high, please be sure to do further research into Monotropism, more information can be found: <a href="https://monotropism.org/" target="_blank" rel="noopener noreferrer">https://monotropism.org/</a></p>
			<br>
			<p>Monotropism Score: {{score}} / 235</p>
			<p>Your Average: {{avg}}</p>
			<br>
			
			<p v-if="avg>=3.91">
				<a href='https://monotropism.org/im-monotropic-now-what/' target="_blank" rel="noopener noreferrer">I'm Monotropic, Now What?</a>
			</p>
			<br>
			<p v-html="scoreQuote"></p>
			<br>
			<p>For visual reference based on the original study, the arrow points to where you would fall on their chart:</p>
			<div style="position:relative">
				<img ref="image" src="./assets/mq.png" alt="Scale Image" style="width: 100%" @load="drawArrow">
				<div ref="arrow" class="arrow"><i class="gg-arrow-long-up-c"></i></div>
			</div>
			
			
			<!-- <p>
				For visual reference, this is your average(blue) compared to the averages of AU(yellow)=autistic people, and AL(purple)=allistic people.
			</p>
			
			<div id="slider">
			</div>  -->
			
		</div>
		<div class="score_box" v-show="errorbox">
			<p v-html="scoreQuote"></p>
		</div>
	</div>
	<div class="progress" :style="{ width: progressWidth, backgroundColor: '#298e46!important'}"> </div>
	<!-- <div class="progress" style="width:100%">{{ progress }} / {{ questions.length }}</div> -->
	
</template>

<style scoped>
.question{
	padding: 2em 0;
}
.selections {
	display:flex;
	flex-wrap: nowrap;
}
.selections>p {
	flex-grow:1;
	display: flex;
	flex-direction: column-reverse;
	text-align: center;
}
.submit {
	display: flex;
	flex-direction: columns;
	justify-content: center;
	padding: 2em;
}
.submit>button{
	padding:1em;
	border-radius: 1em;
	border-style: none;
}
.submit>button:hover{
	background-color: aquamarine;

	cursor: pointer;
}
.pointer {
	cursor: pointer;
}
strong{
	font-weight: bolder;
}
.questions{
	margin-bottom:4em;
}
.wrapper>p {
	margin:1em 0;
}

#slider {
	margin-top:5em;
}


@media only screen and (max-width: 600px) {
	.selections {
		display:flex;
		flex-direction: column;
		flex-wrap: nowrap;
	}
	.selections>p {
		flex-grow:1;
		display: flex;
		flex-direction: row;
		text-align: center;
	}
	.savedBox {
		display:flex;
		flex-direction: column;
		align-items: stretch;
	}
	.savedBox p {
		text-align: center;
	}
	.arrow {
		position: absolute;
		transform: translate(-50%, -80%)!important;
	}

	.gg-arrow-long-up-c {
		transform: scale(1,.8) !important;
	}
}

/* CSS */
.button {
  appearance: none;
  
  border: 1px solid rgba(27, 31, 35, .15);
  border-radius: 6px;
  box-shadow: rgba(27, 31, 35, .1) 0 1px 0;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  display: inline-block;
  font-family: -apple-system,system-ui,"Segoe UI",Helvetica,Arial,sans-serif,"Apple Color Emoji","Segoe UI Emoji";
  font-size: 14px;
  font-weight: 600;
  line-height: 20px;
  padding: 6px 16px;
  position: relative;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: middle;
  white-space: nowrap;
  margin:1em;
}
.button.green{
	background-color: #2ea44f;
}
.button.red{
	background-color: #FF4742;
}
.button.blue{
	background-color: #0276FF;
}

.button:hover {
  /* background-color: rgba(44, 151, 75,.5); */
  filter: brightness(0.8);
}

.button:focus:not(:focus-visible):not(.focus-visible) {
  box-shadow: none;
  outline: none;
}

.button:focus {
  box-shadow: rgba(46, 164, 79, .4) 0 0 0 3px;
  outline: none;
}

.button:disabled {
  /* background-color: #94d3a2; */
  opacity: .5;
  border-color: rgba(27, 31, 35, .1);
  color: rgba(255, 255, 255, .8);
  cursor: default;
}

.button:active {
  background-color: #298e46;
  box-shadow: rgba(20, 70, 32, .2) 0 1px 0 inset;
}
.translations {
	margin-bottom:2rem;
}
.translations>ul>li {
	display:inline-block;
	margin-right:1rem;
}
.lang {
	padding-left: .2rem;
}


.qtitle {

	text-decoration: underline 2px solid;
}

.progress {
	position:fixed;
	left: 0;
	bottom:0;
	height: 1rem;
}

.arrow {
  position: absolute;
  transform: translate(-50%, -150%);
}

.gg-arrow-long-up-c {
	color:#8803fc;
    box-sizing: border-box;
    position: relative;
    display: block;
    transform: scale(3);
    border-right: 2px solid transparent;
    border-left: 2px solid transparent;
    border-bottom: 4px solid transparent;
    box-shadow: inset 0 0 0 2px;
    height: 24px;
    width: 6px
}
.gg-arrow-long-up-c::after,
.gg-arrow-long-up-c::before {
    content: "";
    display: block;
    box-sizing: border-box;
    position: absolute
}
.gg-arrow-long-up-c::after {
    width: 6px;
    height: 6px;
    border-top: 2px solid;
    border-left: 2px solid;
    transform: rotate(45deg);
    top: 0;
    left: -2px
}
.gg-arrow-long-up-c::before {
    width: 6px;
    height: 6px;
    border: 2px solid;
    border-radius: 8px;
    bottom: -4px;
    left: -2px
}
</style>
