<template>
    <section class="quiz">
        <div class="container">
            <div class="left">
                <div class="left-container">
                    <div class="progress-bar-container">
                        <div class="progress-bar" ref="progressBar">
                            <div class="progress-bar-fill" :style="{ width: progressBarWidth + '%' }"></div>
                        </div>
                    </div>
                    <div class="all-questions">
                        <p> {{ currentQuestionIndex + 1 }} / {{ questions.length }}</p>
                        <div class="question" v-if="currentQuestion">
                            <p>{{ currentQuestion.question }}</p>
                            
                            <div class="options">
                                <div v-for="(option, index) in currentQuestion.options" :key="index">
                                    <label>
                                        <input type="radio" :value="option" v-model="selectedOption">{{ option }}
                                    </label>
                                </div>
                            </div>
                        </div>
                        
                        <div class="below-btns">
                            <button class="check-btn" @click="checkAnswer" :disabled="!selectedOption">Жауап беру</button>
                            <button class="nextq-btn" @click="nextQuestion" :disabled="!isAnswerCorrect">Келесі &rarr;</button>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="right">
                <div class="right-container">
                  <div class="points">
                    
                    <div class="hearts">
                        <div v-for="heart in hearts" :key="heart">
                            <a>
                                <svg width="29" height="25" viewBox="0 0 29 25" fill="none" xmlns="http://www.w3.org/2000/svg">
<g filter="url(#filter0_i_1688_47894)">
<path d="M7.95361 0C4.08309 0 0.944336 3.10651 0.944336 6.93918C0.944336 10.0331 2.17096 17.376 14.2451 24.7988C14.4614 24.9304 14.7097 25 14.9629 25C15.216 25 15.4643 24.9304 15.6806 24.7988C27.7548 17.376 28.9814 10.0331 28.9814 6.93918C28.9814 3.10651 25.8427 0 21.9721 0C18.1016 0 14.9629 4.20556 14.9629 4.20556C14.9629 4.20556 11.8241 0 7.95361 0Z" fill="#FF3D32"/>
</g>
<defs>
<filter id="filter0_i_1688_47894" x="0.944336" y="0" width="28.0371" height="27" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
<feFlood flood-opacity="0" result="BackgroundImageFix"/>
<feBlend mode="normal" in="SourceGraphic" in2="BackgroundImageFix" result="shape"/>
<feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha"/>
<feOffset dy="2"/>
<feGaussianBlur stdDeviation="1.85"/>
<feComposite in2="hardAlpha" operator="arithmetic" k2="-1" k3="1"/>
<feColorMatrix type="matrix" values="0 0 0 0 0.960265 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"/>
<feBlend mode="normal" in2="shape" result="effect1_innerShadow_1688_47894"/>
</filter>
</defs>
</svg>
</a>
                        </div>
                    </div>
                    <p>1 мүмкіндік = 1 бонус</p>
                    
                </div>
                <div class="finish-quiz">
                    <p class="finish-txt">Барлық Сұрақтар</p>
                    <div class="question-buttons">
                        <button v-for="(question, index) in questions" :key="index"
                        :class="{ 'answered': index < currentQuestionIndex, 'correct': isCorrect(index) }"
                        @click="setCurrentQuestion(index)">
                        {{ index + 1 }}
                    </button>
                </div>
                <button class="finish-btn" 
                 @click="finishTest">Аяқтау</button>
            </div>
        </div>
                
            </div>
        </div>
    </section>
</template>


<script>
import questions from "../questions.json"

export default {
    data() {
        return {
            questions: questions,
            currentQuestionIndex: 0,
            selectedOption: null,
            hearts: [1, 2, 3],
            answeredQuestions: [], 
            isAnswerCorrect: false 
        };
    },
    computed: {
        currentQuestion() {
            return this.questions[this.currentQuestionIndex] || null;
        },
        progressBarWidth() {
            return ((this.currentQuestionIndex + 1) / this.questions.length) * 100;
        },
        isAnswered() {
            return !!this.selectedOption;
        }
    },
    methods: {
        checkAnswer() {
            const correctAnswer = this.currentQuestion.answer;
            this.isAnswerCorrect = this.selectedOption === correctAnswer;
            if (this.isAnswerCorrect) {
                
                this.answeredQuestions.push(this.currentQuestionIndex);
                this.selectedOption = null; 
                this.nextQuestion(); 
            } else {
                
                if (this.hearts.length > 0) {
                    this.hearts.pop();
                }
            }
        },
        nextQuestion() {
            if (this.currentQuestionIndex < this.questions.length - 1) {
                this.currentQuestionIndex++;
            }
            this.selectedOption = null; 
        },
        setCurrentQuestion(index) {
            this.currentQuestionIndex = index;
            this.selectedOption = null;
        },
        isCorrect(index) {
           
            return this.answeredQuestions.includes(index);
        },
        finishTest() {
            
            console.log('Test finished!');
        }
    }
};
</script>




<style>
.quiz{
    width: 1200px;
    margin: 0 auto;
}
.quiz>.container{
    width: 100%;
    display: flex;
    justify-content: space-between;
    margin-top: 1.2rem;
}

.progress-bar-container{
    width: 700px;
}
.progress-bar {
    margin: 0 auto;
    width: 100%;
    height: 16px;
    margin-bottom: 32px;
    background-color: #ccc;
    border-radius: 1rem;
}

.progress-bar-fill {
    height: 100%;
    background-color: #609AF8; 
    border-radius: 1rem;

}

.left  {
    display: flex;
    justify-content: center;
    width: 900px;
    height: 522px;
    background-color: #fff;
    padding: 2rem;
    border-radius: 15px;
}

.check-btn{
    padding: 8px 32px;
    background-color: #609AF8;
    color: #fff;
    border-radius: 8px;
    font-size: .8rem;
    cursor:pointer;
}
.nextq-btn{
    padding: 8px 32px;
    border: #609AF8 solid 2px;
    color: #609AF8;
    border-radius: 8px;
    font-size: .8rem;
    cursor:pointer;
}

.right {
    display: flex;
    flex-direction: column;
}
.right-container{
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.points{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content:center;
    background-color: #fff;
    padding: 1rem;
    border-radius: 15px;
    width: 270px;
    height: 100px;
}
.hearts {
    display: flex;
    font-size: 2rem;
}

.question-buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}

.question-buttons button{
    border-radius: 8px;
    border: rgba(0, 0, 0, 0.08) solid 1px;
    background-color: #fff;
    width: 45px;
    height: 41px;
}

.correct {
    border: 1px solid rgb(120, 232, 120);
    background-color: rgb(120, 232, 120) !important;
}

.all-questions {
    margin-top: 20px;
    p{
        text-align: center;
        margin-bottom: 1.5rem;
    }
}

.question p {
    font-weight: bold;
}

.options{
    div{
        label{
            input{
                margin: 1.5rem 0;
            }
        }
    }
}
.options label{
    display: flex;
    align-items: center;
}
.options input[type="radio"] {
    transform: scale(1.5);
    margin-right: 10px;
}

.finish-quiz{
    display: flex;
    flex-direction: column;
    text-align: start;
    justify-content: center;
    align-items: center;
    width: 270px;
    height: 270px;
    border-radius: 15px;
    margin-top: 1rem;
    gap: 1rem;
    padding: 1rem;
    background-color: #fff;
}
.hearts a {
    margin-right: 5px; /* Add spacing between hearts */
}

.current-question {
    margin-top: 20px; /* Add margin to separate from hearts section */
}
.below-btns{
    display: flex;
    margin: 2rem 0;
    justify-content: space-between;
}
.finish-btn{
    padding: 12px 8px;
    border-radius: 10px;
    font-size: 14px;
    width: 210px;
    border: solid 2px #64748B;
    background-color: #64748B;
    color: #fff;
    cursor: pointer;
}
</style>
