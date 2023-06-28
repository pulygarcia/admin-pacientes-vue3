<script setup>
    import { reactive } from 'vue';
    import  Alerta from './Alerta.vue';

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente']);


    const props = defineProps({
        nombre:{
            type: String,
            required: true
        },
        propietario:{
            type: String,
            required: true
        },
        email:{
            type: String,
            required: true
        },
        alta:{
            type: String,
            required: true
        },
        sintomas:{
            type: String,
            required: true
        },
        agregarBtn:{
            type: String,
            required: true
        }
    })


    const validar = () => {
        if(Object.values(props).includes('')){
            alerta.tipo = 'error';
            alerta.mensaje = 'Completa todos los campos';

            setTimeout(() => {
                alerta.mensaje = '';
                alerta.tipo = '';
            }, 3000);
            
            return;
        }

        emit('guardar-paciente');
        alerta.mensaje = 'Agregado correctamente';
        alerta.tipo = 'exito';

        setTimeout(() => {
            alerta.mensaje = '';
            alerta.tipo = '';
        }, 3000);
    }
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="text-center text-3xl mt-5 font-black">Seguimiento pacientes</h2>

    <p class="text-lg mt-5 text-center mb-10">
        Añade Pacientes y 
        <span class="text-violet-600 font-bold">Administralos</span>
    </p>

    <Alerta 
        v-if="alerta.mensaje"
        :alerta="alerta"
    />

    <form 
        class="bg-white shadow-xl rounded-md mt-5 py-10 px-5 mb-10"
        @submit.prevent="validar"
    >
        <div class="mb-5">
            <label for="mascota" class="block font-bold text-gray-600 uppercase">Nombre mascota</label>
            <input 
                class="p-2 border-2 mt-3 rounded-sm w-full placeholder-gray-400" 
                type="text" 
                id="mascota" 
                placeholder="Ingresa el nombre"
                :value="nombre"
                @input="$emit('update:nombre', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="propietario" class="block font-bold text-gray-600 uppercase">Propietario</label>
            <input 
                class="p-2 border-2 mt-3 rounded-sm w-full placeholder-gray-400" 
                type="text" 
                id="propietario" 
                placeholder="Ingresa el nombre del propietario"
                :value="propietario"
                @input="$emit('update:propietario', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="email" class="block font-bold text-gray-600 uppercase">Email</label>
            <input 
                class="p-2 border-2 mt-3 rounded-sm w-full placeholder-gray-400" 
                type="email" 
                id="email" 
                placeholder="Email"
                :value="email"
                @input="$emit('update:email', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="alta" class="block font-bold text-gray-600 uppercase">Alta</label>
            <input 
                class="p-2 border-2 mt-3 rounded-sm w-full placeholder-gray-400" 
                type="date" 
                id="alta"
                :value="alta"
                @input="$emit('update:alta', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="sintomas" class="block font-bold text-gray-600 uppercase">Síntomas</label>
            <textarea 
            id="sintomas" 
            class="w-full p-2 mt-3 rounded-sm border-2 placeholder-gray-400" 
            placeholder="Ingrese los sintomas"
            :value="sintomas"
            @input="$emit('update:sintomas', $event.target.value)"
            />
        </div>
        <div class="mb-5">
            <input 
            type="submit"
            class="bg-violet-600 w-full text-white font-bold rounded-sm py-3 uppercase hover:bg-violet-800 transition-colors cursor-pointer"
            :value="agregarBtn"
            >
        </div>
    </form>
  </div>
</template>