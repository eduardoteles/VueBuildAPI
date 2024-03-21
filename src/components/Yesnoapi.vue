<script setup>
    import { ref, watch } from 'vue'

    const loading = ref(false)
    const question = ref('')
    const tip = ref('Please finish question with ?')
    const answer = ref({value: '', forced: false, image: ''})

    watch(question, async (newQuestion, oldQuestion) => {
        tip.value = 'Please finish question with ?';
        if(newQuestion.endsWith('?')) {
            await getAnswer();
        }
    });

    const getAnswer = async () => {
        loading.value = true;
        tip.value = 'Thinking...';
        try {
            const response = await fetch('https://yesno.wtf/api');
            const apiResponse = await response.json();
            answer.value = {value: apiResponse.answer, forced: apiResponse.forced, image: apiResponse.image};
        } catch (error) {
            console.error(error);
            answer.value = {value: error.toString(), forced: false, image: 'error.png'};
        } finally {
            loading.value = false;
            tip.value = '';
        }
    }
</script>


<template>
    <div>
                Place your question
                <input v-model="question" v-bind:disabled="loading" />
                {{tip}}
            </div>
            <div class="row">
                <div class="column" style="background-color:#aaa;">
                  <h2>Answer</h2>
                  <p>{{answer.value}}</p>
                </div>
            </div>
            <div class="row">
                <div class="column" style="background-color:#bbb;">
                  <h2>Forced answer?</h2>
                  <p>{{answer.forced}}</p>
                </div>
            </div>
            <div class="row">
                <div class="column">
                    <img :src=answer.image>
                </div>
            </div>
</template>