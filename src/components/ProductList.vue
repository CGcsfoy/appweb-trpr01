<script setup lang="ts">
import { ref } from 'vue';
import type { Product } from '../types';
//Doc: https://sweetalert2.github.io et https://github.com/avil13/vue-sweetalert2
import Swal from 'sweetalert2';

import ProductForm from './ProductForm.vue';
import ProductTable from './ProductTable.vue';
import ProductDetails from './ProductDetails.vue';

const armada: Product = {
  id: 0,
  brand: 'Armada',
  name: 'Edollo 2024',
  price: 829.99,
  description:
    "Développé pour la légende qui a révolutionné le ski freestyle, le EDOLLO est un déchireur de park conçu pour enchaîner les nose press et bénéficier d'un cambre sous le pied et jusqu'au talon. Unique, tout comme Henrik.",
  quantity: 7
}

const line: Product = {
  id: 1,
  brand: 'Line',
  name: 'Blend 2024',
  price: 489.99,
  description:
    'Les skis Line BLEND (2024) offrent un mélange parfait de polyvalence et de performance sur les pistes et hors-pistes. Avec leur construction légère et leur largeur au patin de 85mm.',
  quantity: 8
}

const skis = ref<Product[]>([armada, line])

const selectedSki = ref<Product | null>(null);
const isEditing = ref<boolean>(false);

const handleProductSubmit = (product: Product) => {
  if (isEditing.value && selectedSki.value) {
    Object.assign(selectedSki.value, product);
    isEditing.value = false;
    selectedSki.value = null;
  } else {
    skis.value.push({ ...product, id: skis.value.length });
  }
};

const editSki = (index: number) => {
  selectedSki.value = skis.value[index];
  isEditing.value = !isEditing.value;
  if (!isEditing.value) {
    selectedSki.value = null;
  }
};

const duplicateSki = (index: number) => {
  const newSki = { ...skis.value[index], id: skis.value.length };
  skis.value.push(newSki);
};

const removeSki = (index: number) => {
  Swal.fire({
    title: "Es-tu sûr ?",
    text: "La supression est irréversible !",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "Red",
    cancelButtonColor: "Blue",
    confirmButtonText: "Oui, supprimer",
    cancelButtonText: "Annuler"
  }).then((result) => {
    if (result.isConfirmed) {
      skis.value.splice(index, 1);
      Swal.fire("Supprimé !", "Le produit a été supprimé.", "success");
      isEditing.value = false;
    }
  });
};


const cancelEdit = () => {
  isEditing.value = false;
  selectedSki.value = null;
};
</script>

<template>
  <div class="container">
    <ProductForm :selectedSki="selectedSki" :isEditing="isEditing" @submit="handleProductSubmit" @cancel="cancelEdit" />
    <ProductTable :skis="skis" @edit="editSki" @duplicate="duplicateSki" @remove="removeSki" />
    <ProductDetails :selectedSki="selectedSki" :isEditing="isEditing" />
  </div>
</template>
