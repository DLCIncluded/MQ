<script>
import noUiSlider from 'nouislider';
import wNumb from 'wnumb';
import { ref,computed } from 'vue';
import { RouterLink, RouterView } from 'vue-router'
import 'nouislider/dist/nouislider.css';
export default {
	data() {
		return {
			questions : [
				{question: "After a period of instability, I need a quiet and predictable environment.",reverse: false, val: 0},
				{question: "I need a quiet and predictable environment for me to switch from one task to another easily. ",reverse: false, val: 0},
				{question: "I often struggle to concentrate in busy and/or unpredictable environments. ",reverse: false, val: 0},
				{question: "I find sudden unexpected disruptions to my attention startling. ",reverse: false, val: 0},
				{question: "It's distressing to be unexpectedly pulled away from something I'm engaged in.",reverse: false, val: 0},
				{question: "I rarely find simultaneously holding eye contact and making a verbal conversation with another person uncomfortable.",reverse: true, val: 0},
				{question: "I often notice details that others do not.",reverse: false, val: 0},
				{question: "Involvement in an activity of interest often reduces my anxiety level.",reverse: false, val: 0},
				{question: "I find social interactions more comfortable if communicating about atopic of interest to me.",reverse: false, val: 0},
				{question: "I am often totally focused on activities I am passionate about, to the point I am unaware of other events. ",reverse: false, val: 0},
				{question: "I can get quite good at something even if I'm not especially interested in it.",reverse: true, val: 0},
				{question: "I often lose sense of time when engaging in activities I am passionate about.",reverse: false, val: 0},
				{question: "I sometimes avoid talking because I cannot reliably predict how others will react, especially strangers. ",reverse: false, val: 0},
				{question: "I tend to do activities because I find them interesting, instead of due to societal expectations.",reverse: false, val: 0},
				{question: "I rarely find social situations chaotic.",reverse: true, val: 0},
				{question: "I don't mind if someone interrupts me when I'm in the middle of an activity.",reverse: true, val: 0},
				{question: "When I'm working on something, I'm open to helpful suggestions.",reverse: true, val: 0},
				{question: "I often find it difficult to switch topics after engaging in an activity for a longtime.",reverse: false, val: 0},
				{question: "I often engage in activities I am passionate about to escape from anxiety.",reverse: false, val: 0},
				{question: "Routines provide an important source of stability and safety. ",reverse: false, val: 0},
				{question: "I manage uncertainty by creating routines. ",reverse: false, val: 0},
				{question: "I often experience anxiety over matters I have little certainty over. ",reverse: false, val: 0},
				{question: "I find it difficult to engage in a task of no interest to me even if it is important. ",reverse: false, val: 0},
				{question: "I often find engaging in stimming (e.g.,fidgeting, rocking) to be relaxing. ",reverse: false, val: 0},
				{question: "I am usually passionate about a few topics at any one time in my life. ",reverse: false, val: 0},
				{question: "I have trouble filtering out sounds when I am not doing something I'm focusedon. ",reverse: false, val: 0},
				{question: "I usually mean what I say and no more than that.",reverse: false, val: 0},
				{question: "I often engage in lengthy discussions on topics I find interesting even though my conversational partner(s) do not. ",reverse: false, val: 0},
				{question: "I sometimes accidentally say something others find offensive/ rude when I am focused on a task. ",reverse: false, val: 0},
				{question: "I can sometimes be very distressed by a topic that others think of as trivial. ",reverse: false, val: 0},
				{question: "I find it easy to keep up with group discussions where everyone is speaking.",reverse: true, val: 0},
				{question: "Often when I am focused on activities, I do not notice I am thirsty or hungry. ",reverse: false, val: 0},
				{question: "Often when I am focused on activities, I do not notice I need the bathroom. ",reverse: false, val: 0},
				{question: "When there is a lot of information to consider, I often struggle to make adecision.  ",reverse: false, val: 0},
				{question: "Sometimes making a decision is so hard I get physically stuck.  ",reverse: false, val: 0},
				{question: "I sometimes focus on an incident for a substantial time (days) after the event.",reverse: false, val: 0},
				{question: "I sometimes become highly anxious by focusing on the many possible situations that might occur at a future event.",reverse: false, val: 0},
				{question: "Sometimes when I am focused on an activity, I do not recall all the information I might need to make good decisions. ",reverse: false, val: 0},
				{question: "People tell me I get fixated on things. ",reverse: false, val: 0},
				{question: "I find a problem I can't solve distressing and/or hard to put down. ",reverse: false, val: 0},
				{question: "I tend to feel quite self-conscious unless I'm deeply absorbed in a task.  ",reverse: false, val: 0},
				{question: "I often get stuck thinking about all the possibilities that might come out of a decision. ",reverse: false, val: 0},
				{question: "When I am interested in something, I tend to be passionate about it. ",reverse: false, val: 0},
				{question: "When I am interested in a topic, I like to learn everything I can about that topic.  ",reverse: false, val: 0},
				{question: "I am still fascinated by many of the things I was interested in when I was much younger.",reverse: false, val: 0},
				{question: "I rarely find myself getting stuck in loops of thought. ",reverse: true, val: 0},
				{question: "I often loop back to previous thoughts. ",reverse: false,val: 0}
				],
				scorebox: false,
				score: 0,
				avg: 0
		}
	},
	methods: {
		checkscore() {
			this.scorebox=true
			let l = this.questions.length;
			let score = 0;
			for (var i = 0; i < l; i++) {

				var x = parseInt(this.questions[i].val)
				score = score + x
			}
			this.score = score
			this.avg = score / 47
			console.log(this.avg)

			var slider = document.getElementById('slider');
			if(!slider.noUiSlider){
				noUiSlider.create(slider, {
					start: [20,100, 211],
					connect: [false,false,true, true],
					tooltips: [
						{
							to: function ( value ) {
							return "<span class='score_slider'>Your Score</span>";
							}
						},
						{
							to: function ( value ) {
							return "<span class='under'>Low</span>";
							}
						},
						{
							to: function ( value ) {
							return  "<span class='under'>High</span>";
							}
						}
					],
					behaviour: 'unconstrained-tap',
					range: {
						'min': 0,
						'max': 235
					}
				});
			} 
			

			// // The display values can be used to control the slider
			// valuesSlider.noUiSlider.set(['7', '28']);
			console.log(score)
			slider.noUiSlider.set([score,'178', '211']);
			slider.noUiSlider.disable();
		}
	}
}

</script>

<template>
  <header>
    <!-- <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" /> -->

    <div class="wrapper">
	    <h1>Monotropism Questionnaire</h1>
		<p><strong>BEFORE YOU BEGIN!!! I am not a doctor</strong>. I am a bored webdev who thinks self diagnosing is valid.</p> 
		
		<p>
			This is not diagnosing anything. Questions taken directly from: <a href="https://osf.io/4wru2" target="_blank">https://osf.io/4wru2</a> and <a href="https://osf.io/g4kc9" target="_blank">scored based on their findings</a>.<br/>
			Original License info:  This questionnaire is published under a Creative Commons license, CC-BY-NC-SA.  Full text for this license can be found on the Creative Commons website <a href="https://creativecommons.org/licenses/by-nc-sa/2.0/" target="_blank">here</a>. 
		</p>
		<p>
			Original Credit: <a href="https://doi.org/10.17605/OSF.IO/WPX5G" target="_blank"><i>Garau, V., Woods, R., Chown, N., Hallett, S., Murray, F.,Wood, R.,Murray, A.& Fletcher-Watson, S. (2023). The Monotropism Questionnaire, Open Science Framework.</i> </a>
		</p>

		<p>
			I <strong>do not own</strong> the questions, or content in the questions; I do not store or collect this information please see the source code if you are concerned it is <a href="https://github.com/DLCIncluded/MQ" target="_blank">open source</a>. <strong>All processing is done in YOUR browser in memory, no data will ever be sent to me, or anyone else.</strong>
		</p>
		<p>
			All this site is for is to make taking the questionnaire easier, and I was bored, and too lazy to tally up the score myself... and yes I know that making this site was far more work than just doing a bit of adding... 
		</p>
			

    </div>
  </header>

	<div>
		<div v-for="(item,index) in questions" :key="index">
			<div class="question">
				{{item.question}}
				 <!-- - {{item.val}} -->
			</div>
			
			<div v-if="!item.reverse" class='selections'>
				<p><input type="radio" :name="index" :id="'r'+index+'0'" v-model="item.val" value="0"><label :for="'r'+index+'0'">N/A</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'1'" v-model="item.val" value="1"><label :for="'r'+index+'1'">Strongly Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'2'" v-model="item.val" value="2"><label :for="'r'+index+'2'">Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'3'" v-model="item.val" value="3"><label :for="'r'+index+'3'">Neither Agree or Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'4'" v-model="item.val" value="4"><label :for="'r'+index+'4'">Agree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'5'" v-model="item.val" value="5"><label :for="'r'+index+'5'">StronglyAgree</label></p>
			</div>
			<div v-else class='selections'>
				<p><input type="radio" :name="index" :id="'r'+index+'0'" v-model="item.val" value="0"><label :for="'r'+index+'0'">N/A</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'1'" v-model="item.val" value="5"><label :for="'r'+index+'1'">Strongly Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'2'" v-model="item.val" value="4"><label :for="'r'+index+'2'">Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'3'" v-model="item.val" value="3"><label :for="'r'+index+'3'">Neither Agree or Disagree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'4'" v-model="item.val" value="2"><label :for="'r'+index+'4'">Agree</label></p>
				<p><input type="radio" :name="index" :id="'r'+index+'5'" v-model="item.val" value="1"><label :for="'r'+index+'5'">StronglyAgree</label></p>
			</div>
		</div>
		
		<div class="submit">
			<button @click="checkscore">Submit</button>
		</div>

		<div class="score_box" v-show="scorebox">
			<p>{{score}} / 235</p>
			<p>{{avg}}</p>
			<p v-if="avg >= 4.16">
				Autistic Likely -- disclaimer NOT A DOCTOR
			</p>
			<p v-else-if="avg<4.15 && avg>3.7">
				Autisim possible -- disclaimer NOT A DOCTOR
			</p>
			<p v-else>
				Allistic Likely -- disclaimer NOT A DOCTOR
			</p>
			<br>
			<p>Average autistic score: ~4.15 with standard deviation: .347</p>
			<p>Average allistic score: ~3.19 with standard deviation: .578</p>

			<p>If your score falls anywhere to the right of "low" on the chart below, it is possible you fall on the autism spectrum. </p>
			<div id="slider">
			</div>
			
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
}
</style>
