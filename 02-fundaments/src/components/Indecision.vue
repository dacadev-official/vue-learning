<template>
  <h1>Indecision App</h1>
  <img v-if="image" :src="image" alt="background">
  <div class="bg-dark"></div>

  <div class="indecision-container">
    <input
      type="text"
      placeholder="Hazme una pregunta"
      v-model="question"
      @keypress.enter="askQuestion"
    />
    <p>Recuerda terminar con un signo de interrogación (?)</p>

    <div v-if="isValidQuestion">
        <h2>{{ question }}</h2>
        <h1>{{ answer }}</h1>
    </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            question: null,
            answer: null,
            image: null,
            isValidQuestion: false,
        };
    },
    methods: {
        async getAnswer() {
            try {

                this.answer = '...pensando';
                const { answer, image } = await fetch('https://yesno.wtf/api').then(r => r.json());

                this.answer = answer === 'no' ? 'No' : 'Si';
                this.image = image;
            } catch (e) {
                this.answer = 'No se pudo cargar del API'
                this.image = undefined
            }

        },
    },
    // pendiente de otros cambios
    watch: {
        question(value, oldValue) {

            console.log({ value });

            this.isValidQuestion = false;
            if ( value.includes('?') ) {
                this.getAnswer();
                this.isValidQuestion = true;
            }
        },
    },

}
</script>

<style>
    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .bg-dark {
        background-color: rgba(0, 0, 0, 0.4);
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }
    
    input {
        width: 250px;
        padding: 10px 15px;
        border-radius: 5px;
        border: none;
        margin-bottom: 10px;
    }
    input:focus {
        outline: none;
    }

    p {
        color: white;
        font-size: 20px;
        margin-top: 0px;
    }

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }
</style>