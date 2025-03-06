<script setup lang="ts">
import { ref, defineProps, defineEmits, watch } from 'vue';
import type { Product } from '../types';

const props = defineProps<{
    selectedSki: Product | null,
    isEditing: boolean
}>();

const emit = defineEmits(['submit', 'cancel']);

const newSki = ref<Product>({
    id: 0,
    brand: '',
    name: '',
    price: 0,
    description: '',
    quantity: 0
});

const errors = ref({
    name: '',
    brand: '',
    description: '',
    price: '',
    quantity: ''
});

watch(() => props.selectedSki, (newValue) => {
    if (newValue) {
        newSki.value = { ...newValue };
    } else {
        newSki.value = {
            id: 0, brand: '', name: '', price: 0, description: '', quantity: 0
        };
    }
});

const validateForm = () => {
    errors.value = {
        name: newSki.value.name.trim() ? '' : 'Le nom du produit est requis.',
        brand: newSki.value.brand.trim() ? '' : 'La marque est requise.',
        description: newSki.value.description.trim() ? '' : 'La description est requise.',
        price: newSki.value.price > 0 ? '' : 'Le prix doit être supérieur à 0.',
        quantity: newSki.value.quantity < 0 || newSki.value.quantity == null
            ? 'La quantité ne peut pas être vide ou négative.'
            : ''
    };

    return Object.values(errors.value).every(error => error.trim() === '');
};

const handleSubmit = () => {
    if (!validateForm()) return;
    emit('submit', { ...newSki.value });
    newSki.value = {
        id: 0, brand: '', name: '', price: 0, description: '', quantity: 0
    };
};
</script>

<template>
    <div class="container mt-4 text-white">
        <h2>{{ isEditing ? 'Modifier' : 'Ajouter' }} une paire de skis</h2>
        <form @submit.prevent="handleSubmit" novalidate>

            <!-- Nom du produit -->
            <div class="mb-3">
                <label class="form-label">Nom du produit</label>
                <input v-model="newSki.name" type="text" class="form-control" :class="{ 'is-invalid': errors.name }" />
                <div v-if="errors.name" class="invalid-feedback">{{ errors.name }}</div>
            </div>

            <!-- Marque -->
            <div class="mb-3">
                <label class="form-label">Marque</label>
                <input v-model="newSki.brand" type="text" class="form-control"
                    :class="{ 'is-invalid': errors.brand }" />
                <div v-if="errors.brand" class="invalid-feedback">{{ errors.brand }}</div>
            </div>

            <!-- Description -->
            <div class="mb-3">
                <label class="form-label">Description</label>
                <textarea v-model="newSki.description" class="form-control"
                    :class="{ 'is-invalid': errors.description }"></textarea>
                <div v-if="errors.description" class="invalid-feedback">{{ errors.description }}</div>
            </div>

            <!-- Prix -->
            <div class="mb-3">
                <label class="form-label">Prix</label>
                <input v-model.number="newSki.price" type="number" class="form-control"
                    :class="{ 'is-invalid': errors.price }" min="1" step="0.01" />
                <div v-if="errors.price" class="invalid-feedback">{{ errors.price }}</div>
            </div>

            <!-- Quantité -->
            <div class="mb-3">
                <label class="form-label">Quantité</label>
                <input v-model.number="newSki.quantity" type="number" class="form-control"
                    :class="{ 'is-invalid': errors.quantity }" min="0" />
                <div v-if="errors.quantity" class="invalid-feedback">{{ errors.quantity }}</div>
            </div>

            <!-- Boutons -->
            <button type="submit" class="btn btn-primary mt-2">{{ isEditing ? 'Modifier' : 'Ajouter' }}</button>
            <button v-if="isEditing" type="button" class="btn btn-secondary mt-2 ms-2"
                @click="emit('cancel')">Annuler</button>
        </form>
    </div>
</template>
