<script setup>
  import {ref, reactive, watch, onMounted} from 'vue';
  import { uid } from 'uid';

  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Paciente from './components/Paciente.vue';

  const pacientes = ref([]);
  let agregarBtn = ref('Resgistrar');

  watch(pacientes, () => {
    guardarStorage();
  },{
    deep: true
  })

  onMounted(() => {
    const pacientesStorage = JSON.parse(localStorage.getItem('pacientes'));
    if(pacientesStorage){
      pacientes.value = pacientesStorage;
    }
  })

  const datosPaciente = reactive({
    id: null,
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: '',
  })

  const guardarPaciente = () => {
    //Como la funcion actualizar llena datosPaciente de nuevo, si el objeto se lleno con un id, actualizalo con una copia de lo que tenga escrito.
    if(datosPaciente.id){
      const {id} = datosPaciente;

      const indice = pacientes.value.findIndex(pacienteState => pacienteState.id === id);

      pacientes.value[indice] = {...datosPaciente};
    }else{
      //push normal si datosPaciente estaba limpio (ya que se limpia al agregar un paciente)
      pacientes.value.push({...datosPaciente, id: uid()});
    }

    datosPaciente.nombre = '';
    datosPaciente.propietario = '';
    datosPaciente.email = '';
    datosPaciente.alta = '';
    datosPaciente.sintomas = '';
    datosPaciente.id = null;

    //Submit texto normal
    agregarBtn = 'Registrar';
  }

  const actualizarPaciente = (id) => {
    //traer el paciente a editar
    const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0];

    //llenar campos
    datosPaciente.nombre = pacienteEditar.nombre;
    datosPaciente.propietario = pacienteEditar.propietario;
    datosPaciente.email = pacienteEditar.email;
    datosPaciente.alta = pacienteEditar.alta;
    datosPaciente.sintomas = pacienteEditar.sintomas;
    datosPaciente.id = pacienteEditar.id;

    //Submit modo actualizar
    agregarBtn = 'Actualizar';
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(paciente => paciente.id !== id);
  }

  //LS
  const guardarStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
  }
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 md:flex">
      <Formulario 
          v-model:nombre="datosPaciente.nombre"
          v-model:propietario="datosPaciente.propietario"
          v-model:email="datosPaciente.email"
          v-model:alta="datosPaciente.alta"
          v-model:sintomas="datosPaciente.sintomas"
          @guardar-paciente="guardarPaciente"
          :agregarBtn="agregarBtn"
      />

      <div class="md:w-1/2 h-screen overflow-y-scroll">
        <h2 class="font-black text-3xl text-center mt-5">Listado pacientes</h2>

        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de 
            <span class="text-violet-600 font-bold">Pacientes</span>
          </p>

          <Paciente 
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
    
        <p v-else class="font-black text-4xl text-center mt-40 text-gray-500">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
