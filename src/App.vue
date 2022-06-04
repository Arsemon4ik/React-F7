<template>
  <h1>Шифр Цезаря Vue</h1>
  <div class="wrapper">
    <div class="input">
      <label for="n">Текст для шифрования</label>
      <input v-model="inputText" type="text" placeholder="Привет, как дела?" class="input-text">
      <input v-model="outputCipher" type="text" readonly class="output-cipher">
      <button @click="cezarCipher" class="cipher">Зашифровать</button>
    </div>
    <div class="input">
      <label for="n">Шифр для разшифрования</label>
      <input v-model="inputCipher" type="text" placeholder="аав ыфвоаи ывроиа.." class="input-cipher">
      <input v-model="outputText" type="text" readonly class="output-text">
      <button @click="cezarUnCipher" class="unCipher">Разшифровать</button>
    </div>
    <div class="input">
      <label for="n">Число итераций: 1,2..</label>
      <input id="counter" v-model="inputN" type="number">
    </div>
  </div>

</template>

<script>
import {isCapital, arr_ru, arr_RU, symbols} from "@/utils";

export default {
  data() {
    return {
      inputText: '',
      inputCipher: '',
      inputN: 1,
      outputCipher: '',
      outputText: ''
    }
  },
  methods: {
    cezarCipher() {
      const arr = this.inputText
      const n = this.inputN
      const arrayLength = arr_ru.length - 1
      let new_arr = ''
      for (let i in arr) {
        const value = arr[i]

        //если элемент относится к заглавным то ищем среди заглавных..
        const indexArr = isCapital(value) ?
            arr_RU.indexOf(value) :
            arr_ru.indexOf(value)

        //если элемент относится к символам
        if (symbols.includes(value))
          new_arr += value

        // если нашли букву
        else if (indexArr >= 0) {
          // если привышаем длину массива то берем сначала
          if (indexArr + n > arrayLength) {
            new_arr += isCapital(value) ?
                arr_RU[arrayLength - indexArr + n - 1] :
                arr_ru[arrayLength - indexArr + n - 1]
            continue
          }
          //иначе берем как обычно
          new_arr += isCapital(value) ?
              arr_RU[indexArr + n] :
              arr_ru[indexArr + n]
        }

      }

      this.outputCipher = new_arr
    },
    cezarUnCipher(){
      const arr = this.inputCipher
      const n = this.inputN
      const arrayLength = arr_ru.length
      let new_arr = ''
      for (let i in arr) {
        const value = arr[i]

        //если элемент относится к заглавным то ищем среди заглавных..
        const indexArr = isCapital(value) ?
            arr_RU.indexOf(value) :
            arr_ru.indexOf(value)

        //если элемент относится к символам
        if (symbols.includes(value))
          new_arr += value

        // если индекс < 0 берем с конца
        else if (indexArr >= 0) {
          if (indexArr - n < 0) {
            const startIndex = arrayLength - n + indexArr
            console.log(startIndex)
            new_arr += isCapital(value) ?
                arr_RU[startIndex] :
                arr_ru[startIndex]
            continue
          }
          //иначе берем как обычно
          new_arr += isCapital(value) ?
              arr_RU[indexArr - n] :
              arr_ru[indexArr - n]
        }

      }

      this.outputText = new_arr
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
.wrapper {
  margin-top: 20px;
  padding: 40px;
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;
  font-size: 24px;
  border: 1px solid burlywood;
}

.input {
  display: flex;
  flex-direction: column;
}

input, button {
  padding: 10px 8px;
  margin: 20px;
  font-size: 20px;

}

.input{
  display: flex;
  flex-direction: column;
  align-items: center;

}

button {
  cursor: pointer;
  background-color: white;
  border: 1px solid cadetblue;
  /*transition: 0.3s ease-in-out;*/
  margin-bottom: 20px;
  margin-top: 40px;
}

.output-cipher,.output-text {
  outline: none;
  opacity: 0;

}

button:hover {
  background-color: lightblue;
}

#counter{
  width: 20%;
}

h1{
  text-align: center;
}


@media screen and (max-width: 938px) {
  .wrapper{
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}
</style>
