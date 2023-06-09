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
                <router-link :to="`/actions/${action.name}`">{{ action.title }}</router-link>
                <Button @click="completeAction(action.name)" icon="check" />
                
              </li>
          </ul>

          <Button @click="addActionDialogeShown = true" icon-left="plus">New Action</Button>
        </div>
      </Card>
    </div>

    <Dialog :options="{title: 'Add New Action', actions: [
        {
          label: 'Add Action',
          appearance: 'primary',
          handler: ({ close }) => {
            // updateRole()
            addAction()
            close() // closes dialog
          },
        },
        { label: 'Cancel' },
      ],
    }" 
    v-model="addActionDialogeShown">
    <template #body-content>
      <p class="mt-4 text-lg">
        <div class="space-y-2">
          <Input v-model="action.title" type="text" required placeholder="title" lable="Title" />
          <Input v-model="action.category" label="List" type="select" :options="categoryOptions" />
        </div>
      </p>
    </template></Dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Dialog, createListResource, Card, Button, Input } from 'frappe-ui'
import { createResource } from 'frappe-ui'
import { session } from '../data/session'
import { reactive } from 'vue'
import { computed } from '@vue/reactivity'

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


const categories = createListResource({
  doctype: 'Category',
  fields: ["name"],
  transform(categories) {
    return categories.map((c) => c.name)
  },
  cache: 'actions', //cache test comment
})

categories.reload()
console.log(categories)

const categoryOptions = computed(() => {
  if(categories.list.loading || !categories.data){
    return []
  }
  //console.log(categories.data)
  return categories.data
})

const completeAction = (actionName) => {
  //console.log(actionName)
  actions.setValue.submit({
    name: actionName,
    status: 'Completed'
  })
}

const addAction = () => {
  actions.insert.submit(action)
  //console.log(action)
}

const showDialog = ref(false)
</script>
