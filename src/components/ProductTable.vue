<script setup lang="ts">
import { ref, computed, defineProps, defineEmits } from 'vue';
import type { Product } from '../types';
import ProductExport from './ProductExport.vue';
import ProductSearch from './ProductSearch.vue';

const props = defineProps<{ skis: Product[] }>();
const emit = defineEmits(['edit', 'duplicate', 'remove', 'export']);

const searchQuery = ref('');

const alertMessage = ref<string | null>(null);


const filteredSkis = computed(() => {
    return props.skis.filter((ski) =>
        ski.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        ski.brand.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
});

const getStockClass = (quantity: number) => {
    if (quantity === 0) {
        alertMessage.value = "Un produit est en rupture de stock ! Vérifiez vos stocks.";
        return 'bg-danger';
    }
    if (quantity < 5) return 'bg-warning';
    return 'bg-success';
};

</script>


<template>
    <div class="container mt-4 text-white">
        <h2>Liste des skis</h2>

        <ProductExport :skis="skis" />
        <ProductSearch @update:search="searchQuery = $event" />

        <p>Cliquez sur un ski pour le modifier.</p>
        <div v-if="alertMessage" class="alert alert-warning alert-dismissible show" role="alert">
            <strong>Attention !</strong> {{ alertMessage }}
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"
                @click="alertMessage = null"></button>
        </div>

        <table class="table table-dark table-striped">
            <thead>
                <tr>
                    <th>Marque</th>
                    <th>Nom</th>
                    <th>Prix</th>
                    <th>Quantité</th>
                    <th class="text-end">Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-if="filteredSkis.length === 0">
                    <td colspan="5" class="text-center text-white">
                        Aucun produit disponible ou ne correspond à votre recherche.
                    </td>
                </tr>

                <tr v-for="(ski, index) in filteredSkis" :key="ski.id" @click="emit('edit', index)"
                    style="cursor: pointer">
                    <td>{{ ski.brand }}</td>
                    <td>{{ ski.name }}</td>
                    <td>{{ ski.price }} $</td>
                    <td>
                        <div class="p-2 rounded text-white w-100" :class="getStockClass(ski.quantity)">
                            {{ ski.quantity }}
                        </div>
                    </td>
                    <td class="text-end">
                        <div>
                            <button class="btn btn-secondary btn-sm me-2" @click.stop="emit('duplicate', index)">
                                Dupliquer
                            </button>
                            <button class="btn btn-danger btn-sm" @click.stop="emit('remove', index)">
                                Supprimer
                            </button>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
