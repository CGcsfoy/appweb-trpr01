<script setup lang="ts">
import { defineProps } from 'vue';
import type { Product } from '../types';

const props = defineProps<{ selectedSki: Product | null, isEditing: boolean }>();
</script>

<template>
    <div v-if="props.selectedSki && props.isEditing" class="mt-4 p-3 bg-secondary text-white">
        <h3>Détails du ski</h3>
        <p><strong>Marque :</strong> {{ props.selectedSki.brand }}</p>
        <p><strong>Nom :</strong> {{ props.selectedSki.name }}</p>
        <p><strong>Prix :</strong> {{ props.selectedSki.price }} $</p>
        <p><strong>Quantité :</strong> {{ props.selectedSki.quantity }}</p>
        <p><strong>Description :</strong> {{ props.selectedSki.description }}</p>
    </div>
</template>
