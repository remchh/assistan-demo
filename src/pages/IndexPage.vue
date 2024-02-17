<template>
  <q-page class="column justify-end q-pa-md q-gutter-y-md">


     <div class="col-4 q-pa-md" style="width: 100%;">
        <q-chat-message
          :text="['hey, how are you?']"
          sent
        />
        <q-chat-message
          :text="['doing fine, how r you?']"
        />
      </div> 
      
    <!-- v-model='text '-->
      <div class='col-4' style="width: 100%;">
        <q-input outlined autogrow >
          <template v-slot:append>
            <q-avatar>
              <img src="https://cdn.quasar.dev/logo-v2/svg/logo.svg">
            </q-avatar>
          </template>

          <template v-slot:after>
            <q-btn round dense flat icon="send" />
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
</script>
