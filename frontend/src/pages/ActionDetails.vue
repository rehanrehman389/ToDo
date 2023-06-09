<template>
    <div class="mx-20 my-4" v-if="!action.get.loading">
        <div class="flex flex-row items-center justify-between">
            <h1 class="font-black text-5xl text-gray-900">{{ action.doc.title }}</h1>
            <Button icon-left="arrow-left" @click="router.back()">Go Back</Button>
        </div>
    </div>
    <LoadingIndicator v-else class="w-6 text-blue-500"/>
</template>

<script setup>
import { useRouter } from 'vue-router';
import { createDocumentResource, LoadingIndicator } from 'frappe-ui';

const router =  useRouter()
const props = defineProps(['name'])

const action = createDocumentResource({
    doctype: 'Action',
    name: props.name,
    onSuccess(data) {
        console.log(data)
    }
})
action.reload()
</script>