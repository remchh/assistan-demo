<template>
  <q-page class="column justify-end q-pa-md q-gutter-y-md">


     <div class="col-4 q-pa-md" style="width: 100%;">
        <q-chat-message class="text-subtitle1"
          :text="[question.content]"
          sent
          v-for="question in questions" :key="questions.id"
        />

        <q-chat-message class="text-subtitle1"

          :text="['Response from OPEN AI']"
        />
      </div> 

      
    
      <div class='col-4' style="width: 100%;" >
        <q-input outlined autogrow v-model="newQuestion" placeholder="How can I help?">
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
async function createMessage() {
  const threadMessages = await openai.beta.threads.messages.create(
    threadID,
    { role: "user", content: "Can you recommend an action movie?" }
  )

  console.log(threadMessages)
}

//createMessage()

//Run the assistan thread
async function runThread() {
  const run = await openai.beta.threads.runs.create(
    threadID,
    { assistant_id: asstID }
  )

  console.log(run)
}

//runThread()


//Get the current run
/*const currentRun = await openai.beta.threads.runs.retrieve(
    threadID,
    "run_im95Bfx0ksgMvo2gd8579icB"
  )

console.log("Run status: " + currentRun.status)*/

//List thread messages
async function listMessages() {
  const threadMessages = await openai.beta.threads.messages.list(
    threadID
  )

  console.log(threadMessages.data[0].content[0].text.value);
}

//listMessages()


</script>

<script setup>

import { ref } from 'vue'

const newQuestion = ref('')
const questions = ref([
  {id:0, content:'My question'}
])
const response = ref('')

const sendQuestion = () => {
  
  questions.value.push({
    id: questions.value.length + 1,
    content: newQuestion.value
  })
  newQuestion.value = ''
  console.log('Question sended', questions.value[1].content)
}

console.log(questions.value)

</script>