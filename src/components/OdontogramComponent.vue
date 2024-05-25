<template>
  <div class="odontogram">
    <h2>Odontograma</h2>
    <div class="grid">
      <Tooth v-for="tooth in teeth" :key="tooth.id" :id="tooth.id" :number="tooth.number" @update-description="updateDescription" />
    </div>
  </div>
</template>

<script>
import Tooth from './AppTooth.vue';

export default {
  name: 'OdontogramComponent',
  components: {
    Tooth
  },
  props: ['teeth'],
  methods: {
    updateDescription({ id, description }) {
      const tooth = this.teeth.find(t => t.id === id);
      if (tooth) {
        tooth.description = description;
      }
    }
  }
};
</script>

<style scoped>
.odontogram {
  text-align: center;
  margin: 20px auto;
  padding: 20px;
  background-color: white;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  border-radius: 8px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(8, 70px);
  gap: 20px;
  justify-content: center;
}

@media (max-width: 768px) {
  .grid {
    grid-template-columns: repeat(4, 70px);
  }
}

@media (max-width: 480px) {
  .grid {
    grid-template-columns: repeat(2, 70px);
  }
}
</style>
