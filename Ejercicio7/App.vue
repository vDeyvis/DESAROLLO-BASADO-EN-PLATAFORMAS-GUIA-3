<script setup>
import { ref } from 'vue'
import BarraNavegacion from './components/BarraNavegacion.vue'
import PanelAviso from './components/PanelAviso.vue'
import DestinoCard from './components/DestinoCard.vue'
import ResumenReservas from './components/ResumenReservas.vue'
import PiePagina from './components/PiePagina.vue'

const destinos = ref([
  { id: 1, nombre: 'Islas flotantes de los Uros', ubicacion: 'Bahía de Puno',
    duracion: '3 h', precio: 25,
    descripcion: 'Visita a las islas de totora y paseo en balsa.' },
  { id: 2, nombre: 'Isla Taquile', ubicacion: 'Lago Titicaca',
    duracion: '8 h', precio: 60,
    descripcion: 'Textiles tradicionales y mirador del lago.' },
  { id: 3, nombre: 'Isla Amantaní', ubicacion: 'Lago Titicaca',
    duracion: '2 días', precio: 120,
    descripcion: 'Hospedaje con familias locales y templos en las cumbres.' },
  { id: 4, nombre: 'Península de Capachica', ubicacion: 'Capachica',
    duracion: '1 día', precio: 90,
    descripcion: 'Playas, miradores y gastronomía de la zona.' },
  { id: 5, nombre: 'Chullpas de Sillustani', ubicacion: 'Laguna Umayo',
    duracion: '4 h', precio: 40,
    descripcion: 'Torres funerarias preincaicas junto a la laguna.' },
  { id: 6, nombre: 'Isla Suasi', ubicacion: 'Lago Titicaca',
    duracion: '2 días', precio: 150,
    descripcion: 'Isla privada con senderos y vistas al atardecer.' }
])

const reservas = ref([])

function reservar(destino) {
  const existente = reservas.value.find(r => r.id === destino.id)
  if (existente) {
    existente.personas++
  } else {
    reservas.value.push({ ...destino, personas: 1 })
  }
}

function cancelar(id) {
  reservas.value = reservas.value.filter(r => r.id !== id)
}

function vaciar() {
  reservas.value = []
}
</script>

<template>
  <BarraNavegacion titulo="Reservas Titicaca"
    :enlaces="['Destinos', 'Reservas', 'Contacto']" />

  <main class="container my-4">
    <PanelAviso tipo="warning">
      <template #titulo>Precios referenciales</template>
      Los precios mostrados son ficticios y se usan solo con fines académicos.
      <template #pie>Cada clic en «Reservar paseo» suma una persona.</template>
    </PanelAviso>

    <div class="row">
      <section class="col-lg-8">
        <div class="row">
          <div class="col-sm-6 mb-4" v-for="destino in destinos" :key="destino.id">
            <DestinoCard :destino="destino" @reservar="reservar" />
          </div>
        </div>
      </section>
      <aside class="col-lg-4">
        <ResumenReservas :reservas="reservas"
          @cancelar="cancelar" @vaciar="vaciar" />
      </aside>
    </div>
  </main>

  <PiePagina />
</template>

<style>
body {
  background: #f5f5f5;
}
</style>
