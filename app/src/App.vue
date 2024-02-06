<script>
import noUiSlider from 'nouislider';
import wNumb from 'wnumb';
import { ref,computed } from 'vue';
import { RouterLink, RouterView } from 'vue-router';
import 'nouislider/dist/nouislider.css';
import { create, all } from "mathjs";
const math = create(all);
export default {
	data() {
		return {
			lang: "en",
			questions : [
				{question: {en:"After a period of instability, I need a quiet and predictable environment.",pt:"Após um período de instabilidade, preciso de um ambiente calmo e previsível."},reverse: false, val: 0},
				{question: {en:"I need a quiet and predictable environment for me to switch from one task to another easily.",pt:"Preciso de um ambiente calmo e previsível para poder passar facilmente de uma tarefa para outra."},reverse: false, val: 0},
				{question: {en:"I often struggle to concentrate in busy and/or unpredictable environments.",pt:"Tenho frequentemente dificuldade em concentrar-me em ambientes movimentados e/ou imprevisíveis."},reverse: false, val: 0},
				{question: {en:"I find sudden unexpected disruptions to my attention startling.",pt:"As interrupções súbitas e inesperadas da minha atenção assustam-me."},reverse: false, val: 0},
				{question: {en:"It's distressing to be unexpectedly pulled away from something I'm engaged in.",pt:"É angustiante ser inesperadamente afastado de algo em que estou empenhado."},reverse: false, val: 0},
				{question: {en:"I rarely find simultaneously holding eye contact and making a verbal conversation with another person uncomfortable.",pt:"Raramente me sinto desconfortável por manter simultaneamente o contacto visual e manter uma conversa verbal com outra pessoa."},reverse: true, val: 0},
				{question: {en:"I often notice details that others do not.",pt:"Reparo frequentemente em pormenores que os outros não reparam."},reverse: false, val: 0},
				{question: {en:"Involvement in an activity of interest often reduces my anxiety level.",pt:"O envolvimento numa atividade de interesse reduz frequentemente o meu nível de ansiedade."},reverse: false, val: 0},
				{question: {en:"I find social interactions more comfortable if communicating about a topic of interest to me.",pt:"Considero as interacções sociais mais confortáveis, se comunicar sobre um tópico do meu interesse."},reverse: false, val: 0},
				{question: {en:"I am often totally focused on activities I am passionate about, to the point I am unaware of other events.",pt:"Muitas vezes estou totalmente concentrado em atividades que me apaixonam, ao ponto de não me aperceber de outros acontecimentos. "},reverse: false, val: 0},
				{question: {en:"I can get quite good at something even if I'm not especially interested in it.",pt:"Posso tornar-me bastante bom em alguma coisa, mesmo que não esteja especialmente interessado nela."},reverse: true, val: 0},
				{question: {en:"I often lose sense of time when engaging in activities I am passionate about.",pt:"Perco frequentemente a noção do tempo quando estou envolvido em atividades que me apaixonam."},reverse: false, val: 0},
				{question: {en:"I sometimes avoid talking because I cannot reliably predict how others will react, especially strangers.",pt:"Por vezes evito falar porque não consigo prever com fiabilidade a reação dos outros, especialmente dos estranhos."},reverse: false, val: 0},
				{question: {en:"I tend to do activities because I find them interesting, instead of due to societal expectations.",pt:"Normalmente, faço atividades porque as acho interessantes, e não devido às expectativas sociais."},reverse: false, val: 0},
				{question: {en:"I rarely find social situations chaotic.",pt:"Raramente considero as situações sociais caóticas."},reverse: true, val: 0},
				{question: {en:"I don't mind if someone interrupts me when I'm in the middle of an activity.",pt:"Não me importo que alguém me interrompa quando estou a meio de uma atividade."},reverse: true, val: 0},
				{question: {en:"When I'm working on something, I'm open to helpful suggestions.",pt:"Quando estou a trabalhar em algo, estou aberto a sugestões úteis."},reverse: true, val: 0},
				{question: {en:"I often find it difficult to switch topics after engaging in an activity for a long time.",pt:"Muitas vezes tenho dificuldade em mudar de assunto depois de me envolver numa atividade durante muito tempo."},reverse: false, val: 0},
				{question: {en:"I often engage in activities I am passionate about to escape from anxiety.",pt:"Muitas vezes, envolvo-me em atividades que me apaixonam para fugir à ansiedade."},reverse: false, val: 0},
				{question: {en:"Routines provide an important source of stability and safety.",pt:"As rotinas são uma fonte importante de estabilidade e segurança."},reverse: false, val: 0},
				{question: {en:"I manage uncertainty by creating routines.",pt:"Eu controlo a incerteza criando rotinas."},reverse: false, val: 0},
				{question: {en:"I often experience anxiety over matters I have little certainty over.",pt:"Sinto-me frequentemente ansioso em relação a assuntos sobre os quais tenho poucas certezas."},reverse: false, val: 0},
				{question: {en:"I find it difficult to engage in a task of no interest to me even if it is important.",pt:"Tenho dificuldade em dedicar-me a uma tarefa que não me interessa, mesmo que seja importante."},reverse: false, val: 0},
				{question: {en:"I often find engaging in stimming (e.g.,fidgeting, rocking) to be relaxing.",pt:"Muitas vezes, considero relaxante o facto de me envolver em estímulos (por exemplo, mexer-me, balançar)."},reverse: false, val: 0},
				{question: {en:"I am usually passionate about a few topics at any one time in my life.",pt:"Normalmente, apaixono-me por um pequeno número de temas de cada vez, nos vários momentos da minha vida."},reverse: false, val: 0},
				{question: {en:"I have trouble filtering out sounds when I am not doing something I'm focused on.",pt:"Tenho dificuldade em filtrar os sons quando estou a fazer algo em que não estou focado."},reverse: false, val: 0},
				{question: {en:"I usually mean what I say and no more than that.",pt:"Normalmente, quero dizer  aquilo que disse, e não mais do que isso."},reverse: false, val: 0},
				{question: {en:"I often engage in lengthy discussions on topics I find interesting even though my conversational partner(s) do not.",pt:"Muitas vezes, discorro longamente sobre temas que considero interessantes, mesmo que não sejam do interesse do(s) meu(s) interlocutor(es)."},reverse: false, val: 0},
				{question: {en:"I sometimes accidentally say something others find offensive/rude when I am focused on a task.",pt:"Por vezes, quando estou concentrado numa tarefa, digo acidentalmente algo que os outros consideram ofensivo/rude."},reverse: false, val: 0},
				{question: {en:"I can sometimes be very distressed by a topic that others think of as trivial.",pt:"Posso, por vezes, ficar muito stressado com um assunto que os outros consideram trivial."},reverse: false, val: 0},
				{question: {en:"I find it easy to keep up with group discussions where everyone is speaking.",pt:"Tenho facilidade em acompanhar discussões em grupo em que todos estão a falar."},reverse: true, val: 0},
				{question: {en:"Often when I am focused on activities, I do not notice I am thirsty or hungry.",pt:"Muitas vezes, quando estou concentrado em atividades, não me apercebo que tenho sede ou fome."},reverse: false, val: 0},
				{question: {en:"Often when I am focused on activities, I do not notice I need the bathroom.",pt:"Muitas vezes, quando estou concentrado nas atividades, não me apercebo que preciso de ir à casa de banho."},reverse: false, val: 0},
				{question: {en:"When there is a lot of information to consider, I often struggle to make a decision. ",pt:"Quando há muita informação para considerar,  tenho muitas vezes dificuldade em tomar uma decisão."},reverse: false, val: 0},
				{question: {en:"Sometimes making a decision is so hard I get physically stuck. ",pt:"Por vezes, é tão difícil tomar uma decisão que fico fisicamente bloqueado."},reverse: false, val: 0},
				{question: {en:"I sometimes focus on an incident for a substantial time (days) after the event.",pt:"Por vezes, mantenho-me focado num incidente, muito tempo (dias) após o mesmo ter ocorrido."},reverse: false, val: 0},
				{question: {en:"I sometimes become highly anxious by focusing on the many possible situations that might occur at a future event.",pt:"Por vezes, fico muito ansioso ao focar-me nas várias situações possíveis que podem decorrer de um acontecimento futuro."},reverse: false, val: 0},
				{question: {en:"Sometimes when I am focused on an activity, I do not recall all the information I might need to make good decisions.",pt:"Por vezes, quando estou concentrado numa atividade, não me lembro de toda a informação de que preciso para tomar boas decisões."},reverse: false, val: 0},
				{question: {en:"People tell me I get fixated on things.",pt:"As pessoas dizem-me que fico fixado nas coisas."},reverse: false, val: 0},
				{question: {en:"I find a problem I can't solve distressing and/or hard to put down.",pt:"Considero que um problema que não consigo resolver é angustiante e/ou difícil de largar."},reverse: false, val: 0},
				{question: {en:"I tend to feel quite self-conscious unless I'm deeply absorbed in a task. ",pt:"Costumo sentir-me bastante consciente de mim próprio, a menos que esteja profundamente absorvido numa tarefa."},reverse: false, val: 0},
				{question: {en:"I often get stuck thinking about all the possibilities that might come out of a decision.",pt:"Fico frequentemente bloqueado a pensar em todas as possibilidades que podem resultar de uma decisão."},reverse: false, val: 0},
				{question: {en:"When I am interested in something, I tend to be passionate about it.",pt:"Quando estou interessado em alguma coisa, tenho tendência a apaixonar-me por ela."},reverse: false, val: 0},
				{question: {en:"When I am interested in a topic, I like to learn everything I can about that topic. ",pt:"Quando me interesso por um tema, gosto de aprender tudo o que posso sobre o mesmo."},reverse: false, val: 0},
				{question: {en:"I am still fascinated by many of the things I was interested in when I was much younger.",pt:"Continuo a sentir-me fascinado por muitas das coisas que me interessavam quando era muito mais novo."},reverse: false, val: 0},
				{question: {en:"I rarely find myself getting stuck in loops of thought.",pt:"Raramente dou comigo mesmo preso em pensamentos circulares."},reverse: true, val: 0},
				{question: {en:"I often loop back to previous thoughts.",pt:"Volto frequentemente a pensamentos anteriores."},reverse: false,val: 0}
				],
				scorebox: false,
				score: 0,
				avg: 0,
				scoreQuote: "",
				saveProgress:false,
				savedAt:'never',
				knav:false
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
			this.scorebox=true
			let num_questions = this.questions.length;
			let l = this.questions.length;
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
		}
	}
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
				Community Translations:
				<ul>
					<li><input type="radio" name="lang" id="lang-en" v-model="lang" value="en"><label for="lang-en">EN</label></li>
					<li><input type="radio" name="lang" id="lang-pt" v-model="lang" value="pt"><label for="lang-pt">PT</label></li>
				</ul>		
			</div>
		</div>
    </div>
  </header>

	<div>

		<h2 class="qtitle">Questionnaire:</h2>
		<div v-for="(item,index) in questions" :key="index">
			<div class="question">
				<span v-if="this.lang=='en'">
					{{item.question.en}} 
				</span>
				<span v-if="this.lang=='pt'">
					{{item.question.pt}} 
				</span>
				
				 <!-- - {{item.reverse}} -->
			</div>
			
			<div v-if="!item.reverse" class='selections'>
				<p><input type="radio" :name="index" :id="'r'+index+'0'" v-model="item.val" value="0" :checked="item.val == '0'"><label :for="'r'+index+'0'">N/A</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'1'" v-model="item.val" value="1" :checked="item.val == '1'"><label :for="'r'+index+'1'">Strongly Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'2'" v-model="item.val" value="2" :checked="item.val == '2'"><label :for="'r'+index+'2'">Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'3'" v-model="item.val" value="3" :checked="item.val == '3'"><label :for="'r'+index+'3'">Neither Agree or Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'4'" v-model="item.val" value="4" :checked="item.val == '4'"><label :for="'r'+index+'4'">Agree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'5'" v-model="item.val" value="5" :checked="item.val == '5'"><label :for="'r'+index+'5'">Strongly Agree</label></p>
			</div>
			<div v-else class='selections'>
				<p><input type="radio" :name="index" :id="'r'+index+'0'" v-model="item.val" value="0" :checked="item.val == '0'"><label :for="'r'+index+'0'">N/A</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'1'" v-model="item.val" value="5" :checked="item.val == '5'"><label :for="'r'+index+'1'">Strongly Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'2'" v-model="item.val" value="4" :checked="item.val == '4'"><label :for="'r'+index+'2'">Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'3'" v-model="item.val" value="3" :checked="item.val == '3'"><label :for="'r'+index+'3'">Neither Agree or Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'4'" v-model="item.val" value="2" :checked="item.val == '2'"><label :for="'r'+index+'4'">Agree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'5'" v-model="item.val" value="1" :checked="item.val == '1'"><label :for="'r'+index+'5'">Strongly Agree</label></p>
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
			
			
			<!-- <p>
				For visual reference, this is your average(blue) compared to the averages of AU(yellow)=autistic people, and AL(purple)=allistic people.
			</p>
			
			<div id="slider">
			</div>  -->
			
		</div>
	</div>
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

.qtitle {

	text-decoration: underline 2px solid;
}

</style>
