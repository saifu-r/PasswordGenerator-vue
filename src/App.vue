<template>
  <div class="app bg-secondary" >
    <div class="container1 " >
      <h3 class="title">Password Generator</h3>

      <div class="view-container">
        <div class="password-container">
          <h3 ref="textToCopy" class="password">{{ NGpass || ' ' }}</h3>
          <button @click="copyText" class="btn-copy"><img src="./assets/copy.png" alt="copy.image"></button>
        </div>

        <div class="length-container">
          <h5 class="length">Password length</h5>
          <input type="number" v-model="passLength" class="length-value" min="3" max="15"/>
        </div>

        <div class="checkbox-container">
          <label v-for="type in types" :key="type" class="checkbox-label">    
            <div>
              <h6 class="option" v-if="type === 'lower'">{{ "Include lowercase letters" }}</h6>
              <h6 class="option" v-else-if="type === 'upper'">{{ "Include uppercase letters" }}</h6>
              <h6 class="option" v-else-if="type === 'number'">{{ "Include numbers" }}</h6>
              <h6 class="option" v-else-if="type === 'symbol'">{{ "Include symbols" }}</h6>
            </div>

            <div class="checkbox-values">
              <input type="checkbox" class="checkbox-value" name="condition" :value="type" v-model="conditions"/>
            </div>
          </label>
        </div> 
      </div>
      
      <button class="btn-generate" @click="generatePass">Generate Password</button>
    </div>
</div>

</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";

export default defineComponent({
  name: "App",
  components: {},
  setup(){
    const password= ref("")
    const index= ref()
    const passLength= ref(3)
    const Gpass= ref("")
    const NGpass= ref("")
    const typeIndex= ref()
    const shuffle = (word: string): string => {
      const wordArray = word.split('');

      for (let i = wordArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [wordArray[i], wordArray[j]] = [wordArray[j], wordArray[i]];
      }

      return wordArray.join('');
    };
    const types= ref(['lower','upper','number','symbol'])
    const lower = ref([
      'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l',
      'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x',
      'y', 'z'
    ])
    const upper = ref([
      'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J',
      'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V',
      'W', 'X', 'Y', 'Z'
    ])
    const number= ref([
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'
    ])
    const symbol= ref([
    '!', '#', '$', '%', '&', '(', ')', '*', '+'
    ])

    const conditions= ref([])
    const generatePass= ()=>{
      Gpass.value= ""
      const newTypes= ref([])
      newTypes.value= conditions.value
   
      for(let i=0; i<passLength.value; i++){
        typeIndex.value= Math.floor(Math.random()*newTypes.value.length)
        switch(newTypes.value[typeIndex.value]){
          case 'lower': 
            index.value= Math.floor(Math.random()*lower.value.length) 
            password.value= lower.value[index.value]
            break;
          case 'upper': 
            index.value= Math.floor(Math.random()*upper.value.length) 
            password.value= upper.value[index.value]
            break;
          case 'number': 
            index.value= Math.floor(Math.random()*number.value.length) 
            password.value= number.value[index.value]
            break;
          case 'symbol': 
            index.value= Math.floor(Math.random()*symbol.value.length) 
            password.value= symbol.value[index.value]
            break;
        }
        Gpass.value+= password.value
      }
      NGpass.value= shuffle(Gpass.value)
    }

    // function to copy password to the clipcart

    const textToCopy = ref<HTMLElement | null>(null);

    onMounted(() => {
      // Set the reference to the h1 element after the component is mounted
      textToCopy.value = document.querySelector('h3');
    });

    const copyText = () => {
      if (textToCopy.value) {
        const text = textToCopy.value.innerText;

        // Copy text to clipboard
        navigator.clipboard.writeText(text)
          .then(() => {
            alert('Text copied to clipboard!');
          })
          .catch((error) => {
            console.error('Error copying text:', error);
            alert('Error copying text. Please try again.');
          });
      }
    };

    return {password, generatePass, passLength, Gpass, NGpass, types, conditions, textToCopy, copyText}
  }
});
</script>

<style>

.app{
  color: white;
                                                                
}

.title{
  margin-top: 20px;
  
}

.container1{                   
  position: absolute;                                          
  display: flex;
  border: 1px solid black ;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
  width: 30%;
  height: auto;
  top: 10%;
  left: 35%;

  border-radius: 20px;
  background: #003554;
  
}

.view-container{
  width: 100%;
}

.password-container{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 20px;
  padding: 10px;
  border-radius: 10px;
  border: 1px solid #D5DEEF ;

}

.length-container{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 20px;

  
}

.checkbox-label{
  display: flex;
  flex-direction: row;;
  justify-content: space-between;
  margin: 10px 20px;
  padding: 10px;
  border-radius: 10px;
  border: 1px solid #D5DEEF ;
}
.btn-copy{
  border-radius: 1rem;
  padding: 10px;
}
.btn-copy img{
  height: 30px;
  
}


.btn-generate{
  
  margin: 30px;

  font-weight: 600;
  transition: all 300ms ease;
  padding: 0.5rem;
  width: 60%;
  border-radius: 2rem;

}






</style>
