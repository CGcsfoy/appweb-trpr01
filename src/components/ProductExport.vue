<script setup lang="ts">
import { defineProps } from 'vue';
import type { Product } from '../types';
import Swal from 'sweetalert2';

const props = defineProps<{ skis: Product[] }>();


// Après la lecture de la documentation, j'ai fait une première version que j'ai fait corriger par AppWebGPT
const exportCSV = () => {
    if (props.skis.length === 0) {
        Swal.fire({
            title: "Aucun produit à exporter",
            text: "La liste des produits est vide.",
            icon: "warning",
            confirmButtonText: "OK"
        });
        return;
    }

    const csvHeader = "ID,Marque,Nom,Prix,Quantité,Description\n";
    const csvRows = props.skis.map(ski =>

        `${ski.id},"${ski.brand}","${ski.name}",${ski.price},${ski.quantity},"${ski.description.replace(/"/g, '""')}"`//Contre les guillmets dans description: https://stackoverflow.com/questions/46637955/write-a-string-containing-commas-and-double-quotes-to-csv
    ).join("\n");//Fait que chaque produit devient une ligne distincte.

    //Doc: https://medium.com/%40jeremy.w.barbara/how-to-download-a-csv-upon-button-click-in-javascript-e3ae1bae2178
    const csvContent = csvHeader + csvRows;
    //Doc: https://stackoverflow.com/questions/56154046/downloading-blob-with-type-text-csv-strips-unicode-bom
    const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
    const url = URL.createObjectURL(blob);
    const link = document.createElement("a");

    //Doc: https://www.restack.io/p/ai-dataset-creation-answer-download-csv-file-javascript-cat-ai
    link.href = url;
    link.setAttribute("download", "liste_ski.csv");
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
};
</script>

<template>
    <button @click="exportCSV" class="btn btn-primary mb-3">
        Exporter en CSV
    </button>
</template>
