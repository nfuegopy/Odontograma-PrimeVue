<template>
  <div class="tooth">
    <div class="quadrant top" :class="{selected: sections.top}" @click="toggleSection('top')"></div>
    <div class="quadrant left" :class="{selected: sections.left}" @click="toggleSection('left')"></div>
    <div class="quadrant right" :class="{selected: sections.right}" @click="toggleSection('right')"></div>
    <div class="quadrant bottom" :class="{selected: sections.bottom}" @click="toggleSection('bottom')"></div>
    <div class="tooth-number">{{ number }}</div>
    <div v-if="selected" class="diagnosis">
      <input type="text" v-model="diagnosis" @input="updateDescription" placeholder="Diagnóstico" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppTooth',
  props: ['id', 'number'],
  data() {
    return {
      selected: false,
      diagnosis: '',
      sections: {
        top: false,
        left: false,
        right: false,
        bottom: false
      }
    };
  },
  methods: {
    toggleSection(section) {
      this.sections[section] = !this.sections[section];
      this.selected = Object.values(this.sections).some(val => val);
      this.updateDescription();
    },
    updateDescription() {
      this.$emit('update-description', { id: this.id, description: this.diagnosis, sections: this.sections });
    }
  }
};
</script>

<style scoped>
.tooth {
  position: relative;
  width: 70px;
  height: 70px;
  margin: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
  border: 1px solid #ccc;
  border-radius: 50%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.quadrant {
  position: absolute;
  width: 35%;
  height: 35%;
  border-radius: 50%;
  transition: background-color 0.3s;
}

.top { top: 5%; left: 32.5%; }
.left { top: 32.5%; left: 5%; }
.right { top: 32.5%; left: 60%; }
.bottom { top: 60%; left: 32.5%; }

.selected {
  background-color: #000;
}

.tooth-number {
  position: absolute;
  bottom: 5px;
  font-size: 0.8rem;
}

.diagnosis {
  position: absolute;
  bottom: -35px;
  width: 100px;
  text-align: center;
}

.diagnosis input {
  width: 100%;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
