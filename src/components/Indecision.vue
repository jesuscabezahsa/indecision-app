<script setup lang='ts'>
import { computed, ref, watch } from 'vue';

const question = ref('')
const myAnswer = ref('');
const image = ref('')
const isLoading = ref(true)

const isValidQuestion = computed(() => {
    return question.value.endsWith('?')
})

watch(question, (_newValue, _oldValue) => {
    if (isValidQuestion.value) {
        //const { answer, img, loading } = useGetAnwser()
        setTimeout(() => {
            fetch('https://yesno.wtf/api').then(r => r.json()).then(r => {
                console.log({ r })
                myAnswer.value = r.answer === 'yes' ? 'Sí' : 'No';
                image.value = r.image;
                isLoading.value = false;
            })
        }, 3000)
    } else {
        myAnswer.value =''
        image.value = ''
        isLoading.value = true
    }
})


</script>

<template>
    
    <h1>Respuesta corta (sí / no) App</h1>
    <h3>Recuerda que esto es solo un juego. Lo que responda el programa no tiene por qué ser verdad !!! Diviertete un rato </h3>

    <div class="indecision-container">
        <input
            v-model="question"
            type="text"
            placeholder="Hazme una pregunta"
            class="indecision-container_input"
        >
        <p class="indecision-container_remember">Recuerda terminar con un signo de interrogación (?)</p>
    </div>

    <div class="section-response" v-if="isValidQuestion">
        <p class="section-response_question">{{ question }}</p>
        <div class="section-thinking" v-if="isLoading">
            ...Pensando :)
        </div>
        <div v-else>
            <p  class="section-response_answer">{{ myAnswer }}</p>
            <img class="section-response_image" :src="image" alt="anwser image">
        </div>
        
    </div>
    
</template>


<style scoped>
    .indecision-container, .section-response {
        border: 2px solid #ccc;
        width: 90%;
        margin: 0 auto;
    }
    .indecision-container_input {
        width: 60%;
        padding: 0.5em;
        font-size: 1.2em;
        border: none;
        border-bottom: 2px solid #ccc;
        border-radius: 20px;
        margin: 1.3rem auto;
        outline: none;
        text-align: center;
    }
    .indecision-container_remember {
        font-size: 1.3em;
        color: #ccc;
    }
    .section-response {
        margin-top: 2rem;
        padding: 1rem;
        border-radius: 20px;
        width: 80%;
    }
    .section-response_question {
        font-size: 2.5rem;
        font-weight: bold;
        text-align: center;
    }
    .section-thinking {
        font-size: 1.5em;
        font-weight: bold;
        text-align: center;
        color: #ccc;
    }
    .section-response_answer {
        font-size: 3em;
        font-weight: bold;
        text-align: center;
        color: #42b883;
    }

    .section-response_image {
        width: 50%;
        height: 50%;
        border-radius: 20px;
        margin-top: 1rem;
    }

    @media screen and (max-width: 500px) {
        .indecision-container, .section-response {
            border: 2px solid #ccc;
            width: 96%;
            margin: 0 auto;
        }
        .indecision-container_input {
            width: 80%;
            padding: 0.2em;
            font-size: 0.8em;
            border: none;
            border-bottom: 2px solid #ccc;
            border-radius: 20px;
            margin: 0.5rem auto;
            outline: none;
            text-align: center;
        }
        .indecision-container_remember {
            font-size: 0.8em;
            color: #ccc;
        }
        .section-response {
            margin-top: 0.8rem;
            padding: 0.5rem;
            border-radius: 20px;
            width: 90%;
        }
        .section-response_question {
            font-size: 1.2rem;
            font-weight: bold;
            text-align: center;
        }
        .section-thinking {
            font-size: 0.7rem;
            font-weight: bold;
            text-align: center;
            color: #ccc;
        }
        .section-response_answer {
            font-size: 1.5em;
            font-weight: bold;
            text-align: center;
            color: #42b883;
        }

        .section-response_image {
            width: 50%;
            height: 50%;
            border-radius: 20px;
            margin-top: 1rem;
        }
    }

</style>