<script setup lang="ts">
import { defineProps } from 'vue';
import type { Product } from '../types';

const props = defineProps<{ selectedSki: Product | null, isEditing: boolean }>();
</script>

<template>
    <div v-if="selectedSki && isEditing" class="mt-4 p-3 bg-secondary text-white">
        <h3>Détails du ski</h3>
        <p><strong>Marque :</strong> {{ selectedSki.brand }}</p>
        <p><strong>Nom :</strong> {{ selectedSki.name }}</p>
        <p><strong>Prix :</strong> {{ selectedSki.price }} $</p>
        <p><strong>Quantité :</strong> {{ selectedSki.quantity }}</p>
        <p><strong>Description :</strong> {{ selectedSki.description }}</p>
    </div>
</template>