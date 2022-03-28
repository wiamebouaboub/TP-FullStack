<script setup>
defineProps(["ville"]);
</script>
<template>
    <tr>
        <td>{{ville.name}}</td>
        <td>{{ville.population}}</td>
        <td> <button class="btn" @click="$emit('eventSup',ville,index)">Supprimer</button></td>
    </tr>
</template>
