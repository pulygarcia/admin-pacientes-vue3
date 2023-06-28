<script setup>
    import { computed } from 'vue';

    const props = defineProps({
        alerta: Object,
        required: true
    })

    const isError = computed(() => {
        return props.alerta.tipo === 'error';
    })
</script>

<template>
  <div 
    :class="[isError ? 'bg-red-500' : 'bg-green-500']"
    class="text-center text-white p-3 font-bold"
    >
    {{ props.alerta.mensaje }}
  </div>
</template>