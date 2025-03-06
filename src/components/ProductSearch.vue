<script setup lang="ts">
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['update:search']);
const searchQuery = ref('');

const updateSearch = () => {
    //https://stackoverflow.com/questions/68114611/vuejs-update-parent-values-from-emit-with-parameters
    emit('update:search', searchQuery.value);
};
</script>


<template>
    <div class="mb-3">
        <input v-model="searchQuery" @input="updateSearch" type="text" class="form-control"
            placeholder="Rechercher un ski par nom ou par marque" />
    </div>
</template>
