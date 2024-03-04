<template>
  <q-page class="column justify-end q-pa-md q-gutter-y-md">


     <div class="col-4 q-pa-md" style="width: 100%;">
        <q-chat-message 
          class="text-subtitle1"
          v-for="question in questions" :key="question.id" 
          v-show="question.content !== ''"
          :text="[question.content]"
          :sent="question.id % 2 == 0"
        />
        
        <q-chat-message
          bg-color="amber"
          v-show="loading === true"
        >
          <q-spinner-dots size="2rem" />
        </q-chat-message>
      </div> 

      
    
      <div class='col-4' style="width: 100%;" >
        <q-input outlined autogrow v-model="newQuestion" placeholder="How can I help?" @keyup.enter="sendQuestion()">
          <template v-slot:append>
            <q-avatar>
              <img src="https://cdn.quasar.dev/logo-v2/svg/logo.svg">
            </q-avatar>
          </template>

          <template v-slot:after>
            <q-btn round dense flat icon="send" @click="sendQuestion()" />
          </template>
        </q-input>
      </div>


  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage'
})

import { openai } from 'src/boot/config'


const asstID = 'asst_3vDsPjJJRBj1Iox6jxjFOpfP'
const threadID = 'thread_pNs87F9rEIG7FqSWb8bwNJcq'

// Upload a file with an "assistants" purpose
/*const file = await openai.files.create({
  file: await fetch('movies.txt'),
  purpose: "assistants",
})

console.log(file)*/


//create movie expert assistan
/*async function createAssistan(){
  const assistant = await openai.beta.assistants.create({
    name: "Movie Expert",
    instructions: "You are great at recommending movies. When asked a question, use the information in the provided file to form a friendly response. If you cannot find the answer in the file, do your best to infer what the answer should be.",
    tools: [{ type: "retrieval" }],
    model: "gpt-3.5-turbo-0125",
    file_ids: ['file-rQZJo9uWzxtCJlYfPD5tROA4']
  });

  console.log(assistant)
}

createAssistan()*/

//Create a thread --> new conversation
/*const thread = await openai.beta.threads.create()
console.log(thread)*/

//Create a message for the thread
async function createMessage(question) {
  const threadMessages = await openai.beta.threads.messages.create(
    threadID,
    { role: "user", content: question }
  )
  //console.log(threadMessages)
}

//createMessage()

//Run the assistan thread
async function runThread() {
  const run = await openai.beta.threads.runs.create(
    threadID,
    { assistant_id: asstID,
      instructions: `Please do not provide annotations in your reply. Only reply about movies in the provided file. If questions are not related to movies, respond with "Sorry, I don't know." Keep your answers short.` 
    }
  )
  return run
}

//runThread()


//List thread messages
async function listMessages() {
  return await openai.beta.threads.messages.list(
    threadID
  )

  //console.log(threadMessages.data[0].content[0].text.value);
}

//listMessages()

// Get the current run 2
async function retrieveRun(thread, run) {
  return await openai.beta.threads.runs.retrieve(thread, run)

//Get the current run 1
/*const currentRun = await openai.beta.threads.runs.retrieve(
    threadID,
    "run_im95Bfx0ksgMvo2gd8579icB"
  )*/

//console.log("Run status: " + currentRun.status)

}

</script>

<script setup>

import { ref } from 'vue'

const newQuestion = ref('')
const questions = ref([
  {id:1, content:''}
])

const loading = ref(false)

const sendQuestion = () => {
  
  questions.value.push({
      id: questions.value.length + 1,
      content: newQuestion.value 
    })
  newQuestion.value = ''
  console.log('Question sended:', questions.value[questions.value.length -1].content)
  aiResponse()
}



async function aiResponse() {
  //questions.value[0].content = 'Thinking...'
    loading.value = true
    
    //create a message
    await createMessage(questions.value[questions.value.length -1].content)

    //create a run
    const run = await runThread()
    console.log(run)

    // Retrieve the current run
    let currentRun = await retrieveRun(threadID, run.id);
  
    // Keep Run status up to date
    // Poll for updates and check if run status is completed    
    while (currentRun.status !== 'completed') {
      await new Promise(resolve => setTimeout(resolve, 1500));
      console.log(currentRun.status);
      currentRun = await retrieveRun(threadID, run.id);
    } 

    //get messages from the thread
    const { data } = await listMessages()

    //Display the last message for the current run
    //responses.value.response = data[0].content[0].text.value
   

  questions.value.push({
      id: questions.value.length + 1,
      content: data[0].content[0].text.value
    })

  //questions.value[0].content = ''
    loading.value = false
}

//console.log(questions.value)

</script>