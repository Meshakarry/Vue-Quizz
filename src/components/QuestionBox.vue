  <template>

  <div>
    <b-jumbotron>
      <template #header>QuestionBox</template>

      <template #lead>
      {{currentQuestion.question}}
      </template>

      <hr class="my-4">
      <b-list-group>
          <b-list-group-item v-for="(answer,index) in shuffledArray"
          :key="index"
            @click="selectAnswer(index)"
            :class="pickClass(index)"
          >
          
              {{answer}}
          </b-list-group-item>
            
      </b-list-group>

      <b-button
       variant="primary"
        @click="SumbitAnswer"
        :disabled="selected===null || answered"
      
      >Confirm</b-button>
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>

  </template>

  <script>
      import _ from 'lodash';
    export default{
      props:{
          currentQuestion:Object,
          next:Function,
          Counter:Function
      },
      data(){
        return{
              selected:null,
              shuffledArray:[],
              correctIndex:null,
              answered:false

            }

          },
      computed:{
          answers(){
              let array=[...this.currentQuestion.incorrect_answers];
              array.push(this.currentQuestion.correct_answer);
              return array;
          }
      },
      watch:{
        currentQuestion: {
          immediate:true,
          handler(){
            this.selected=null;
            this.answered=false;
            this.shuffleAnswers();
          }
        }
      },
      methods:{
            selectAnswer(index) {
                
                this.selected=index;
                },
            shuffleAnswers(){
              let array=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];

            this.shuffledArray=_.shuffle(array);
              this.correctIndex=this.shuffledArray.indexOf(this.currentQuestion.correct_answer);
            },
            SumbitAnswer(){
              let isCorrect=false;

              if(this.selected===this.correctIndex){
                isCorrect=true;
              }
              this.answered=true;
              this.Counter(isCorrect);
              
            },
          pickClass(index){
            let selectClass='';
            if(!this.answered && this.selected===index){
              selectClass='selected';
            }else if(this.answered && this.correctIndex===index){
              selectClass='success';
            }else if(this.answered && this.selected===index && this.correctIndex!==index){
              selectClass='false';
            }
            return selectClass;
          }
          },
    
    }

  </script>


  <style scoped>
    
    .list-group{

        margin-bottom: 15px;
        }
      .list-group-item:hover{
            background-color: #EEE;
            cursor: pointer;
          }
    .btn {
        margin: 0 5px;
        }
        .selected{
          background-color:lightblue;
          }
          .success{
          background-color:lightgreen;

          }
          .false{
          background-color:red;

          }

  </style>