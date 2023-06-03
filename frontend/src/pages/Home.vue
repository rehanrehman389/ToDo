<template>
  <div class="mx-20">

    <div class="flex flex-row items-center justify-between">
      <h2 class=" text-5xl font-black text-gray-900">Lists</h2>
      <Button icon-left="plus">New List</Button>
    </div>

    <div class="mt-2">
      <Card title="General">
        <div>
          <ul>
              <!-- {{ actions.data }} -->
              <li class="flex flex-row space-y-1 items-center justify-between" v-for="action in actions.data" :key="action.title">
                <span>{{ action.title }} - {{ action.status }}</span>
                <Button @click="completeAction(action.name)" icon="check" />
                
              </li>
          </ul>

          <Button @click="addActionDialogeShown = true" icon-left="plus">New Action</Button>
        </div>
      </Card>
    </div>

    <Dialog :options="{title: 'Add New Action'}" v-model="addActionDialogeShown"/>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Dialog, createListResource, Card, Button } from 'frappe-ui'
import { createResource } from 'frappe-ui'
import { session } from '../data/session'
import { reactive } from 'vue'

// const ping = createResource({
//   url: 'ping',
//   auto: true, 
// })

const action  = reactive({
  title: '',
  category: 'General'
})


const addActionDialogeShown = ref(false)

const actions = createListResource({
  doctype: 'Action', 
  fields: ["name", "title", "status", "description", "category", "date", "due_date"],
  filters: {
    status: 'ToDo'
  },
  limit: 10
})

actions.reload()

const completeAction = (actionName) => {
  //console.log(actionName)
  actions.setValue.submit({
    name: actionName,
    status: 'Completed'
  })
}

const addAction = (action) => {
  actions.insert.submit(action)
}

const showDialog = ref(false)
</script>
