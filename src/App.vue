<script setup>
import questionsData from './assets/data/questions.json';
import {ref} from 'vue'

//ค่า v-model ที่ user กรอก
const inputAnswer = ref('') 

//ค่าของ page นั้นๆ ใช้ v-show ในการ check
const currentPage = ref('Home') 

//ตั้งค่าเป็น 0 เพื่อเอาไปอ้างตำแหน่งของ questionsData
const currentQuestionIndex = ref(0) 

//How to play
const showHowToPlay = ref(false)

//คะแนนของผู้เล่น
const score = ref(0) // เก็บคะแนนเริ่มที่ 0

//ตั้งตัวแปรให้หัวใจ เพื่อโชว์และลดหัวใจ ใช้ v-show 
const heart = ref(3)

//เพลงจบเกม
const adoSuccess = ref(null)
const musicSuccess = "./success.mp3"

//เพลงหลัก
const letPlay = ref(false)
const music = './Pleasure.mp3'
const ado = ref(null)
const playMusic =()=>{
    letPlay.value =! letPlay.value
    if(letPlay.value){
      ado.value.volume=0.02
      ado.value.play()
    }else{
      ado.value.pause()
    }
}

//เปลี่ยนหน้า
const changePage = (page) =>{
    currentPage.value = page
    inputAnswer.value = ''
    if(currentQuestionIndex.value === questionsData.length || currentPage.value === 'End'){
      adoSuccess.value.play()
      adoSuccess.value.volume = 0.5
      return currentPage.value = 'End'
    }
}


//ปุ่ม Hint
const hint = ref(3) //ค่าเริ่มต้นของหลอดไฟเป็น 3
const hideInfo = ref(false) //กรอบ hint


//ปุ่ม hint หากกดครบ 3 ครั้งจะไม่สามารถใช้ hint ต่อได้
const decreaseHint = () => {
  if (hideInfo.value === false && hint.value > 0) {
    hideInfo.value = true
    hint.value--
  }
}


//เอาค่าคำตอบมาเพื่อเช็คว่า user ตอบถูกไหม
const filterAnswer=()=>{
 return questionsData[currentQuestionIndex.value].answer.find(
  (ans)=>ans === inputAnswer.value)
}


//ตรวจคำตอบ
const checkAnswer = () => {
  if (inputAnswer.value === filterAnswer()) {
    score.value++;
    changePage('TrueAnswer');  
  } else {
    heart.value--;
    changePage('FalseAnswer');

    if (heart.value <= 0) {
      return changePage('GameOver');
    }
  }

  currentQuestionIndex.value++;
  hideInfo.value = false

  setTimeout(() => {
    changePage('Question');
  }, 1200);
  
};

//Reset
const tryAgain = ()=> {
  heart.value = 3
  score.value = 0
  inputAnswer.value = ''
  currentPage.value = 'Home'
  hint.value = 3
  hideInfo.value = false
  currentQuestionIndex.value = 0
  ado.value.currentTime=0
  showHowToPlay.value = false
}

const restart = ()=>{
 currentQuestionIndex.value = 0
 tryAgain()
 changePage('Question')
}

</script>
 
<template>

<!-- Google ฟอนต์   -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Chakra+Petch:ital,wght@1,700&family=Charm&display=swap" rel="stylesheet">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@100;300&display=swap" rel="stylesheet">


<!-- ALL PAGE -->
<div class="w-full h-full">
  
  <!-- HOME   -->
  <div v-show="currentPage==='Home'">
    <div class="hero min-h-screen" style="background-image: url(https://cdn.pixabay.com/photo/2017/08/27/23/29/kermit-2687975_640.jpg);">
      <div class="hero-overlay bg-opacity-70"></div>
        <div class="hero-content text-center text-neutral-content">
          <div class="max-w-md">
            <p class="text-8xl pb-5 pl-7">อะไรเอ่ย ?</p>
            <button class="button-30 rounded-full" @click="changePage('Question')">play</button>
            <br><br>
            <!-- How to play -->
            <div> <button class="hover:text-gray-400" @click="showHowToPlay = !showHowToPlay">How to play?</button> 
              <div v-show="showHowToPlay" class="wapper2 flex p-3 m-2" >เกมนี้เป็นเกมตอบคำถามปั่นๆกวนๆ คุณมีหัวใจ 3 ดวง <br>ถ้าตอบผิดครบ 3 จาก 10 ข้อ คุณก็จะถูกปรับแพ้ <br>ขอให้มีความสุขในการเล่นนะ</div>
            </div>
            
          </div>
        </div>
    </div>
  </div>

  <!-- Question   -->
  <div v-if="currentPage==='Question'">  
    <div class="hero min-h-screen flex items-center justify-center " style="background-image: url(https://cdn.pixabay.com/photo/2013/07/18/10/56/house-163526_1280.jpg);">
    <div class="hero-overlay bg-opacity-30 absolute inset-0"></div>
    <div class="hero-content text-center text-white">
      <div class="max-w-md mx-auto">
        
        <div class="w-full bg-white bg-opacity-80 p-20 rounded-lg shadow-md">
          
          <!-- heart -->
          <div class="flex flex-row absolute left-0 top-0 mt-4 ml-5 mb-5 space-x-1">
          <div v-show="heart>=1" class="w-10 h-10left-0 top-0 mt-4 ml-5 mb-5 animate-bounce" ><img src="./assets/images/heart.png"></div>
          <div v-show="heart>1" class="w-10 h-10 left-0 top-0 mt-4 ml-5 mb-5 animate-bounce" ><img src="./assets/images/heart.png"></div>
          <div v-show="heart>2" class="w-10 h-10 left-0 top-0 mt-4 ml-5 mb-5 animate-bounce"><img src="./assets/images/heart.png"></div>
          </div> 

          <!-- เริ่มจากคำถามที่ 1 -->
          <p class="text-2xl mb-4 text-black ">คำถามที่ {{ currentQuestionIndex + 1 }}</p>
          <p class="text-2xl mb-6 text-black font-semibold ">{{ questionsData[currentQuestionIndex].question }}</p>


          <!-- คำตอบ user -->
          <input type="text" placeholder="ป้อนคำตอบ" class="textarea textarea-bordered textarea-xl w-full max-w-xs text-white bg-slate-800" v-model.trim="inputAnswer"  
          @keyup.enter="checkAnswer" autofocus/>
          
         
          <!-- กดปุ่มแล้วจะไปทำฟังก์ชั่น CheckAnswer -->
          <button class="btn btn-success mt-4" @click="checkAnswer" :disabled="inputAnswer === ''">
            ตรวจคำตอบ
          </button>

          <button class="absolute right-80 top-0 mt-4 font-bold bg-[#FF7F50] hover:bg-orange-500 text-white w-fit rounded-full p-2 pl-3 pr-3 h-fit" @click="restart" role="button">เริ่มใหม่</button>

          <button class="absolute right-56 top-0 mt-4  font-bold bg-[#FF0000] hover:bg-red-700 text-white w-fit rounded-full p-2 pl-3 pr-3 h-fit" @click="changePage('End')" role="button">จบเกม</button>
          <!-- โชว์ score -->
          <div class="absolute  right-20 top-0 mt-4  font-bold bg-[#45C9A5] text-white w-fit rounded-full p-2 h-fit">SCORE : {{ score }} / 10</div>

          <!-- ปุ่ม Hint -->
          <div v-show="hint === 0 || hint <= 3" class="absolute right-0 top-0 mt-4 mr-5 mb-5 cursor-pointer transition-transform transform-gpu hover:translate-y-1 " >
            <button @click="decreaseHint"><img src="./assets/images/tips.png" class="w-10 h-10"> {{ hint }}</button>
            <!-- No hint -->
            <!-- <div v-show="hint === 0" class=" text-white text-lg  rounded-full w-[4cm] h-fit pl-3">No more hint</div> -->
          </div>  
        </div>
      </div>
    </div>
          <!-- Hint show -->
          <div class="flex items-center justify-center text-white">
          <div v-show="hideInfo" class="wapper flex">
                      <div class="form-box hint">
                          <h1><b>Hint For You</b></h1>
                          <h2 class="mt-3 p-1 ">คำถาม : {{questionsData[currentQuestionIndex].question}}</h2>
                          <h2 class="mb-6 p-1">คำใบ้ : {{questionsData[currentQuestionIndex].hint}}</h2>
                      </div>
                  </div>
            </div>
            
    </div>
  </div>


  <!-- CheckAns -->

  <!-- ตอบถูก -->
  <div v-show="currentPage==='TrueAnswer'">
      <div class="w-full  h-screen flex items-center justify-center bg-[#45C9A5]" >
        <div class="w-5/6 flex justify-center items-center">
        <div class="bg-[#F5ECDB] w-1/2 flex h-[10cm] ">
          <div class="w-full justify-center items-center">
          <div class="grid grid-rows-8 ">
            <div class="bg-[#FA6334] h-10 w-full"></div>
            <div class="flex items-center justify-center mt-5"><img src="./assets/images/tick-mark.png" class="w-[5cm] h-[5cm] "></div>
            <div class="text-[#FA6334] text-2xl flex items-center mt-5 justify-center font-bold">ถูกต้องแล้วค๊าบบบ!!</div>
          </div>
        </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ตอบผิด -->
  <div v-show="currentPage==='FalseAnswer'">
    <div class="w-full h-screen flex items-center justify-center bg-[#f87851]" >
      <div class="w-5/6 flex justify-center items-center">
       <div class="bg-[#F5ECDB] w-1/2 flex h-[10cm]">
        <div class="w-full justify-center items-center">
        <div class="grid grid-rows-8">
          <div class="bg-[#77ABE9] h-10 w-full"></div>
          <div class="flex items-center justify-center mt-5"><img src="./assets/images/declined.png" class="w-[5cm] h-[5cm] "></div>
          <div class="text-[#FA6334] text-2xl flex items-center mt-5 justify-center font-bold">เสียใจด้วย คุณตอบผิด!</div>
        </div>
       </div>
      </div>
    </div>
  </div>
  </div>

   
<!-- จบเกม -->
  <!-- HOME   -->
  <div v-show="currentPage==='End'">
    <audio :src="musicSuccess" type="audio/mpeg" ref="adoSuccess"></audio>
    <div class="hero min-h-screen" style="background-image: url(https://cdn.pixabay.com/photo/2015/04/10/00/41/yellow-715540_1280.jpg);">
      <div class="hero-overlay bg-opacity-70"></div>
        <div class="hero-content text-center text-neutral-content">
          <div class="max-w-md">
            <div class="w-full flex flex-col justify-center items-center">
              <img src="./assets//images/best.png" class="w-52">
              <h1 class="text-8xl text-orange-100 pb-5">Congraturations!</h1>
              <span class="text-2xl rounded-full text-black font-medium bg-yellow-500 text border-8 border-orange-500 p-5"> you earn {{score}} points</span>
            </div>
            <button class="btn mt-5 hover:bg-gray-300" @click="tryAgain" role="button">เล่นอีกรอบ</button>
          </div>
        </div>
    </div>
  </div>
    

     <!-- Game Over -->
    <div v-show="currentPage==='GameOver'">
      <div class="w-full h-screen flex items-center justify-center bg-[#F4A7C1]"  >
      <div class="w-5/6 flex justify-center items-center">
       <div class="bg-[#F5ECDB] w-1/2 flex h-[10cm]">
        <div class="w-full justify-center items-center">
        <div class="grid grid-rows-8">
          <div class="bg-[#CCE2FE] h-10 w-full"></div>
          <div class="flex items-center justify-center"><img src="./assets/images/gameOver.png" class="w-72 h-72 "></div>          
          <div class=" flex items-center justify-center font-bold mb-8" @click="tryAgain" >
            <button class="w-[5cm] h-[1cm] bg-[#FA6334] text-[#F5ECDB]  rounded-full cursor-pointer hover:bg-orange-600">Try again</button>
          </div>
        </div>
       </div>
      </div>
    </div>
  </div>
    </div>

  <!-- Music button-->
  <div class="fixed bottom-5 right-5" v-show="currentPage==='Question' || currentPage==='Home'">
    <audio :src="music" type="audio/mpeg" ref="ado" loop></audio>
          <button :class="{ 'bg-red-500 animate-spin': letPlay, 'bg-green-500': !letPlay }" v-show="ado" class="rounded-full p-2 " @click="playMusic">
              <img class="w-5 h-5 " src="./assets/images/music.png" />
          </button>
        </div>

</div>
</template>
 
<style scoped>

p{
font-family: 'Chakra Petch', sans-serif;
font-family: 'Chakra Petch', cursive;
}



h1{
  text-shadow: 2px 4px black;
}


/* CSS */
.button-30 {
  align-items: center;
  appearance: none;
  background-color: #FCFCFD;
  border-radius: 4px;
  border-width: 0;
  box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px,rgba(45, 35, 66, 0.3) 0 7px 13px -3px,#D6D6E7 0 -3px 0 inset;
  box-sizing: border-box;
  color: #36395A;
  cursor: pointer;
  display: inline-flex;
  font-family: "JetBrains Mono",monospace;
  height: 48px;
  justify-content: center;
  line-height: 1;
  list-style: none;
  overflow: hidden;
  padding-left: 16px;
  padding-right: 16px;
  position: relative;
  text-align: left;
  text-decoration: none;
  transition: box-shadow .15s,transform .15s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: nowrap;
  will-change: box-shadow,transform;
  font-size: 18px;
}

.button-30:focus {
  box-shadow: #D6D6E7 0 0 0 1.5px inset, rgba(45, 35, 66, 0.4) 0 2px 4px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #D6D6E7 0 -3px 0 inset;
}

.button-30:hover {
  box-shadow: rgba(45, 35, 66, 0.4) 0 4px 8px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #D6D6E7 0 -3px 0 inset;
  transform: translateY(-2px);
}

.button-30:active {
  box-shadow: #D6D6E7 0 3px 7px inset;
  transform: translateY(2px);
}

.wapper{
  position: relative;
  width: 400px;
  height: 400px;
  background: transparent;
  border: 2px solid rgba(255, 255, 255, 1);
  border-radius: 20px;
  backdrop-filter: blur(10px);
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  transition: transform .5s ease, height .2s ease;
}

.wapper2{
  position: relative;
  width: 400px;
  height: 100px;
  background: transparent;
  border: 2px solid rgba(255, 255, 255, 1);
  border-radius: 20px;
  backdrop-filter: blur(10px);
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  transition: transform .5s ease, height .2s ease;
}

.form-box h1 {
  font-size: 2em;
  color: rgb(191, 250, 255);
  text-align: center;
}
</style>
