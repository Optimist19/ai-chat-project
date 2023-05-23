<template>
  <div class="hello">

    <div class="img-div">
      <img alt="Vue logo" src="../assets/logo.png" />
      <p class="plus">+</p>
      

      
      <div>
        <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/04/ChatGPT_logo.svg/1024px-ChatGPT_logo.svg.png" class="logo" alt="">
      </div>
    </div>

    <div id="chat-con">

      <div class="para">
        <p>Ask me anything!!!</p>
      </div>
      <!-- <div class="user-input">
        <p>Ask me anything!!!</p>
      </div> -->
      <!-- <div class="response">
        <p>Ask me anything!!!</p>
      </div>
      <div class="user-input">
        <p>Ask me anything!!!</p>
      </div> -->
    </div>
    
    <div>
      <!-- {{ message[1].content }} -->
    </div>
    <div class="t-b">
      <textarea name="" id="" cols="65" rows="5" v-model="input"></textarea>
      <button @click="btn"><i class="fa-brands fa-telegram"></i></button>
    </div>
  </div>
</template>

<script>
import {ref, reactive} from "vue"
import { Configuration, OpenAIApi } from "openai";

export default {
  name: 'HelloWorld',
  setup(){
    const input = ref("")
    

   let message = reactive([
          {
            "role": "system",
            "content":   "You are a highly knowledgeable assistant that is always happy to help."
          }
        ]
      )

    console.log(message, "up")
    
    const configuration = new Configuration({
      // Get your OpenAi api key and set it as a value to the apiKey(key)
        apiKey: "OpenAi_api-key"
    });
    const openai = new OpenAIApi(configuration);
    console.log(openai)

    const btn =  (e) =>{
      e.preventDefault();
      
      message.push({
        "role": "user",
        "content": input.value
      })
      let child = document.createElement("div")
      let parent = document.getElementById("chat-con")
      parent.appendChild(child)
      openAi()
      console.log(document.querySelector(".user-input"))
      child.innerText = input.value
      child.setAttribute("class", "user-input")
    }

    const openAi = async () =>{
    
      const response = await openai.createChatCompletion({
        model: "gpt-3.5-turbo",
        messages: message
      });
   

      let child = document.createElement("div")
      let parent = document.getElementById("chat-con")
      child.innerText = response.data.choices[0].message.content
      console.log(child)
      parent.appendChild(child)
      child.setAttribute("class","response")
      
      message.push(response.data.choices[0].message)
      console.log(response)

    }



    

    console.log(message, "down")
    return{
      input,
      btn,
      message
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.hello{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 20vh;
  margin-bottom: 20vh;
  margin-right: auto;
  margin-left: auto;
  max-width: 40vw; 
  background-color: rgb(195, 219, 164);
}




.img-div{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2vh 2vw;
  border-bottom: 4px solid rgb(107, 255, 174);
  margin-bottom: 2vh;
}

.plus{
  font-size: 24px;
  font-weight: bold;
  margin: 0 2vw;
}

img{
  max-width: 10vw;
}

.logo{
  border-radius: 50%;
  animation: rotate 2s linear infinite;
  margin-bottom: 2vh;
}

#chat-con{
  margin: 0 1vw;
}

.para{
  background-color: rgb(91, 109, 94);
  border-top-right-radius: 10px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  padding: 0.1vh 2vw;
  width: 20vw;
  float: left;
}

 .user-input{
  background-color: rgb(34, 41, 35);
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  padding: 0.1vh 2vw;
  margin: 2vh 0;
  width: 20vw;
  float: right;
}
.response{
  background-color: rgb(91, 109, 94);
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  padding: 0.1vh 2vw;
  margin: 2vh 0;
}


.t-b{
  display: flex;
  align-items: center;
  max-width: 40vw;
  background-color: rgb(71, 69, 69);
}

textarea{
  /* border-radius: 7px; */
  border-right: none;
  border-top-left-radius: 7px;
  border-bottom-left-radius: 7px;
  background-color: rgb(71, 69, 69);
}

button{
  padding: 0.5vh 1vw;
  border-top-right-radius: 7px;
  border-bottom-right-radius: 7px;
  border: none;
}

i{
  font-size: 20px;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
