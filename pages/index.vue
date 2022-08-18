<template>
  <div class="main">
      <div v-if="currentStep === 1" class="quiz-block step-1">
        <div class="count-of-question">10 вопросов</div>
        <h1 class="main-question">Как хорошо вы знаете природу Северной Осетии?</h1>
        <p class="q-desc">Сейчас мы узнаем как хорошо вы разбираетесь в своем родном крае.</p>
        <button @click="nextStep()" class="start">Начать Тест</button>
      </div>
        <div  class="quiz-block" v-if="currentStep === 2">
        <div class="count-of-question">1/10</div>
          <h2>{{ questions[0].question }}</h2>
          <img :src="questions[0].img" alt="">
          <div class="variants-wrapper">
            <div v-for="(v, id) in questions[0].variants" ref="variant" :class="callSomeClass(v)" :key="key"  class="variant">
              <div class="variant-inner">
                <div class="input">
                  <input :id="id" type="radio" @change="chooseVariant(v)" :value="v" class="radio">
                </div>
                <label :for="id">{{ v.variant }}</label>
              </div>
              <div :class="{showdesc: v.selected === true}" class="descr">{{ v.descr }}</div>
            </div>
          </div>  
          <button v-if="showBtn" @click="nextStep()" type="reset" class="start">Следующий вопрос</button>
        </div>
        <div  class="quiz-block" v-if="currentStep === 3">
          <div class="count-of-question">2/10</div>
          <h2>{{ questions[1].question }}</h2>
          <div class="variants-wrapper">
            <div v-for="(v, id) in questions[1].variants" ref="variant" :class="callSomeClass(v)" :key="key"  class="variant">
              <div class="variant-inner">
                <div class="input">
                  <input :id="id"  type="radio" @change="chooseVariant(v)" :value="v" class="radio">
                </div>
                <label :for="id">{{ v.variant }}</label>
              </div>
              <div :class="{showdesc: v.selected === true}" class="descr">{{ v.descr }}</div>
            </div>
          </div>  
          <button @click="nextStep()" v-if="showBtn" class="start">Узнать результат</button>
        </div>
        <div  class="quiz-block" v-if="currentStep === 4">
          <h2>Правильных ответов {{ correctAnswers }} из {{ questions.length }}</h2>
          <ShareNetwork
              network="vk"
              url="https://ne404.ru"
              :title="vkTitle"
              description="This week, I’d like to introduce you to 'Vite', which means 'Fast'. It’s a brand new development setup created by Evan You."
              quote="The hot reload is so fast it\'s near instant. - Evan You"
              hashtags="vuejs,vite"
              media="https://user-images.githubusercontent.com/2951704/111610221-63fc5180-87db-11eb-8e66-22c1309a6a92.png"
            >
    Поделиться результатом в ВК
</ShareNetwork>
        </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return{
      vkTitle: 'Test',
      showInfo: false,
      isCorrect: false,
      inCorret: false, 
      correctAnswers: 0,
      showClass: false,
      showBtn: false,
      currentStep: 1,
      questions: [
        {
          number: 1,
          question: 'Что это за улица?',
          img: require('~/assets/images/mira.jpg'),
          variants: [
            {
              variant: 'Проспект мира',
              descr: 'Да, это именно она!',
              isCorrect: true,
              selected: false
            },
            {
              variant: 'Ленина',
              descr: 'Соберись! Нужно быть повнимательнее',
              isCorrect: false,
              selected: false
            },
            {
              variant: 'Гоголя',
              descr: 'Соберись! Нужно быть повнимательнее',
              isCorrect: false,
              selected: false
            },
            {
              variant: 'Маркова',
              descr: 'Соберись! Нужно быть повнимательнее',
              isCorrect: false,
              selected: false
            },
          ]
        },
                {
          number: 2,
          question: 'На какой реке стоит Владикавказ',
          img: require('~/assets/images/mira.jpg'),
          variants: [
            {
              variant: 'Терек',
              descr: 'Да, это наш бурный терек!',
              isCorrect: true,
              selected: false
            },
            {
              variant: 'Амазонка',
              descr: 'Ужас! Найди атлас, уточни',
              isCorrect: false,
              selected: false
            },
            {
              variant: 'Нил',
              descr: 'Ужас! Найди атлас, уточни',
              isCorrect: false,
              selected: false
            },
            {
              variant: 'Нева',
              descr: 'Ужас! Найди атлас, уточни',
              isCorrect: false,
              selected: false
            },
          ]
        }
      ]
    }
  },
  methods: {
    nextStep(){
      let inputs = document.querySelectorAll('.radio');
      inputs.forEach(input => {
        input.checked = false;
      });
      this.currentStep++
      this.showInfo = false,
      this.isCorrect = false,
      this.inCorret = false, 
      this.showClass = false,
      this.showBtn = false
    },
    chooseVariant(val){ 
      this.inCorret = true
      this.showClass = true
      val.selected = true
      this.showBtn = true
      if(val.isCorrect){
        this.correctAnswers++
      }
    },
    callSomeClass(v){
      if(this.showClass){
        if(v.isCorrect){
          let className = 'correct' 
          return className
        }
        if(!v.inCorret && v.selected){
          return 'incorrect'
        }
      }
    }
  }
}
</script>

<style>

*{
  margin: 0;
  padding: 0;
}

body{
  padding: 0;
  margin: 0;
  font-family: 'Philosopher', sans-serif;
}
.main{
  width: 100%;
  height: 100vh;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(-45deg, #ee7752, #972938, #23a6d5, #23d5ab);
  /* animation-name: 'color-change'; */
  animation-duration: 10s;
  animation-iteration-count: infinite;
  animation-delay: 2s;
}
@keyframes color-change{
  0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}

}
.quiz-block{
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, .8);
  padding: 16px;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
.quiz-block h2{
  margin-bottom: 16px;
}
.quiz-block img{
  max-width: 100%;
  margin-bottom: 20px;
}
.main-question{
  text-align: center;
  margin-bottom: 16px;
}

.q-desc{
  text-align: center;
  margin-bottom: 16px;
}

.start{
  background: orange;
  color: #fff;
  font-family: inherit;
  text-transform: uppercase;
  width: 300px;
  padding: 20px 30px;
  border: none;
  cursor: pointer;
  font-size: 20px;
}

.variants-wrapper{
  width: 100%;
  margin-bottom: 20px;
}
.variant{
  width: 100%;
  border-bottom: 1px solid grey;
}

.variant-inner{
  display: flex;
  padding: 15px 5px;
  align-items: center;
}

.variant-inner .input{
  width: 36px;
}

.descr{
  display: none;
  padding: 0 36px 10px 36px;
}
.showdesc{
  display: block;
}
label{
  display: block;
  width: 100%;
  cursor: pointer;
  font-size: 18px;
}
.radio{
  margin-right: 8px;
  width: 30px;
}
.correct{
  background: rgb(139 197 139);
  position: relative;
}
.input{
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
}
.correct input{
  display: none;
}
.correct:before{
  position: absolute;
  content: '';
  width: 30px;
  height: 30px;
  background-image: url(~/assets/images/correct.svg);
  background-size: cover;
  background-position: center;
  top: 18px;
  left: 5px;
}
.incorrect{
  background: rgb(235, 146, 146);
  position: relative;
}
.incorrect:before{
  position: absolute;
  content: '';
  width: 30px;
  height: 30px;
  background-image: url(~/assets/images/incorrect.svg);
  background-size: cover;
  background-position: center;
  top: 18px;
  left: 5px;
}
.incorrect input{
  display: none;
}
@media (min-width : 768px) {
  .main{
    padding: 32px;
  }
}
@media (min-width : 992px) {
.quiz-block{
  width: 530px;
  min-height: 500px;
 }
}
@media (min-width : 1200px) {
.quiz-block{
  width: 630px;
  height: fit-content;
 }
}
</style>
