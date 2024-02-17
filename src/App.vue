<template>
  <router-view />
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App'
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
    { role: "user", content: "Can you recommend a comedy?" }
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

//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

</script>