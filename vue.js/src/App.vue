<script setup>
import { ref, computed } from 'vue'
const questions=ref([

        {question: "Why is AWS more economical than traditional data centers for applications with varying compute workloads?",
            answer: 2,
            options: [
              "Amazon EC2 costs are billed on a monthly basis.",
              "Users retain full administrative access to their Amazon EC2 instances.",
              "Amazon EC2 instances can be launched on demand when needed.",
              "Users can permanently run enough instances to handle peak workloads."
            ],
            description:`The ability to launch instances on demand when needed allows users to launch and terminate instances in
              response to a varying workload. This is a more economical practice than purchasing enough on-premises servers
              to handle the peak load.`,
            selected: null
        },
        {question: "Which AWS service would simplify the migration of a database to AWS?",
            answer: 1,
            options: [
              "AWS Storage Gateway",
              "AWS Database Migration Service (AWS DMS)",
              "Amazon EC2",
              "Amazon AppStream 2.0"
            ],
            description:`AWS DMS helps users migrate databases to AWS quickly and securely. The source database remains
              fully operational during the migration, minimizing downtime to applications that rely on the database. AWS DMS
              can migrate data to and from most widely used commercial and open-source databases.
              `,
            selected: null
        },
    ])
  
//    const $x=ref(4);
   
//    const $y=computed(()=>{
//     return $x;
//    });
//    const changeX =()=>{
//       $x.value=8;
//     }

    const chosenAnswers=[];
    const rightAnswers=[];
    function setResult(){
      
    }

    const quizCompleted = ref(false)

    const currentQuestion = ref (0)
    // alert(currentQuestion.value);

    // console.log(questions.value[1].question);
    const score = computed (() => {
      let value = 0
      questions.value.map(q => {
        if (q.selected == q.answer) {
            value++
          }
      })
      return value
    })  
    // console.log(score); 

    const getCurrentQuestion = computed (() => {
      let question = questions.value [currentQuestion.value]
      question.index = currentQuestion.value
    //   question.x=5;
    //   console.log(question.x);
      return question
    })
    
    const SetAnswer = evt => {
      questions.value [currentQuestion.value].selected = evt.target.value
      evt.target.value = null

      // if(getCurrentQuestion.selected!=getCurrentQuestion.answer){
      //     chosenAnswers.push(getCurrentQuestion.selected);
      //     rightAnswers.push([getCurrentQuestion.answer,getCurrentQuestion.description]);
      //     console.log(chosenAnswers);
      //     console.log(rightAnswers);
      // }
      console.log(getCurrentQuestion.value.answer);
      console.log(questions.value[0].answer);
    }

    const NextQuestion = () => {
      if (currentQuestion.value < questions.value.length-1) {
          currentQuestion.value++
      } else {
          quizCompleted.value = true
      }
    }

 

</script>

<template>
  <body>
    <main class="app">
      <h1>The Quiz</h1>
      <section class="quiz" v-if="!quizCompleted">
          <div class="quiz-info">
              <span class="question">{{ getCurrentQuestion.question }}</span>
              <span class="score">Score {{ score }}/{{ questions.length}}</span>
          </div>
          <div class="options">
              <label
                  v-for="(option, index) in getCurrentQuestion.options"
                  :key="index"
                  :class="`option ${
                        getCurrentQuestion.selected == index
                           ? index == getCurrentQuestion.answer
                              ? 'correct'
                              : 'wrong'
                        : ''
                  } ${
                        getCurrentQuestion.selected != null &&
                        index != getCurrentQuestion.selected
                        ? 'disabled'
                        :''
                    }`">
                  <input 
                      type="radio"
                      :name="getCurrentQuestion.index"
                      :value="index"
                      v-model="getCurrentQuestion.selected"
                      :disabled="getCurrentQuestion.selected"
                      @change="SetAnswer">
                  <span> {{ option }}</span>
              </label>
          </div>

          <button
              @click="NextQuestion"
              :disabled="! getCurrentQuestion.selected">
              {{
                  getCurrentQuestion.index == questions.length - 1
                      ? 'Finish'
                      : getCurrentQuestion.selected == null
                          ? 'Select an option'    
                          : 'Next Question'
              }}
          </button>
      </section>

      <section v-else>
        <h2>You have finished the quiz!</h2>
        <p>Your score is {{ score }} / {{ questions.length}}</p>
        </section>
  </main>
  </body>
    
</template>

<style scoped>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
  font-family: 'Montserrat',sans-serif;

}

body{
  overflow-x: hidden;
  background-color:#271C36;
  color:#FFF;
  width: 100vw;
}

.app{
  display: flex;
  flex-direction: column;
  align-items:center;
  padding:2rem;
  min-height:100vh;
  width: 100vw;
}

h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz{
  background-color: #382a4b;
  padding:1rem;
  width:100%;
  max-width:640px;
}


.quiz-info{
  display:flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question{
  color: #8F8F8F;
  font-size:1.25rem;
}

.quiz.info .score{
  color:#FFF;
  font-size: 1.25rem;
}

.options{
  margin-bottom: 1rem;
}

.option{
  padding: 1rem;
  display: block;
  background-color:#271C36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover{
  background-color:#2d213f;
}

.option.correct{
  background-color: #2cce7d;
}

.option.wrong{
  background-color: #ff5a5f;
}

.option:last-of-type{
  margin-bottom:0;
}

option.disabled{
  opacity:0.5;
}

.option input{
  display:none;
}

button{
  appearance: none;
  outline:none;
  border:none;
  cursor:pointer;

  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size:1.25rem;
  border-radius: 0.5rem;
}

button:disabled{
  opacity: 0.5;
}

h2{
  font-size: 2rem;
  margin-bottom:2rem;
  text-align:center;
}

p{
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align:center;
}
</style>



