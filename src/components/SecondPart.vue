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
                            <a>❤️</a>
                        </div>
                    </div>
                    <p>1 мүмкіндік = 1 бонус</p>
                    
                </div>
                <div class="finish-quiz">
                    <p>Барлық Сұрақтар</p>
                    <div class="question-buttons">
                        <button v-for="(question, index) in questions" :key="index"
                        :class="{ 'answered': index < currentQuestionIndex, 'correct': isCorrect(index) }"
                        @click="setCurrentQuestion(index)">
                        {{ index + 1 }}
                    </button>
                </div>
                <button class="finish-btn" @click="finishTest">Аяқтау</button>
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
                this.isAnswerCorrect = false; 
            }
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
.question-buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}
.question-buttons button{
    border-radius: 15px;
    border: #cccccc solid 2px;
    width: 45px;
    height: 41px;
}

.correct {
    border: 2px solid rgb(120, 232, 120);
    background-color: rgb(120, 232, 120);
}
.progress-bar-container{
    width: 700px;
}
.progress-bar {
    margin: 0 auto;
    width: 100%;
    height: 20px;
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
    width: 800px;
    height: 400px;
    background-color: #fff;
    padding: 1.5rem;
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
                margin: 1rem 0;
            }
        }
    }
}
.points{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: spa;
    background-color: #fff;
    padding: 1rem;
    border-radius: 15px;
    width: 300px;
    height: 100px;
}
.hearts {
    display: flex;
    font-size: 2rem;
}
.finish-quiz{
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 300px;
    height: 270px;
    border-radius: 15px;
    margin-top: 1rem    ;
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
    justify-content: space-between;
}
.finish-btn{
    padding: 12px 8px;
    border-radius: 10px;
    font-size: 14px;
    border: solid 2px #64748B;
    background-color: #64748B;
    color: #fff;
    cursor: pointer;
}
</style>
