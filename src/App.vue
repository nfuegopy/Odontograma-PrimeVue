<template>
  <div id="app">
    <AppHeader />
    <main>
      <div class="patient-info">
        <div class="input-group">
          <label>Nombre del Paciente:</label>
          <InputText v-model="patient.name" />
        </div>
        <div class="input-group">
          <label>Edad:</label>
          <InputText type="number" v-model="patient.age" />
        </div>
        <div class="input-group">
          <label>Fecha:</label>
          <InputText v-model="currentDate" disabled />
        </div>
      </div>
      <div ref="odontogram">
        <Odontogram :teeth="teeth" @update-description="updateDescription" />
      </div>
      <Button label="Generar PDF" icon="pi pi-file-pdf" @click="generatePDF" />
    </main>
    <footer>
      Todos los derechos reservados Antonio Barrios.
    </footer>
  </div>
</template>

<script>
import jsPDF from 'jspdf';
import AppHeader from './components/AppHeader.vue';
import Odontogram from './components/OdontogramComponent.vue';
import Button from 'primevue/button';
import InputText from 'primevue/inputtext';

export default {
  components: {
    AppHeader,
    Odontogram,
    Button,
    InputText
  },
  data() {
    return {
      patient: {
        name: '',
        age: ''
      },
      teeth: Array.from({ length: 32 }, (_, i) => ({ id: i + 1, number: i + 1, description: '', sections: {} })),
      currentDate: new Date().toLocaleDateString()
    };
  },
  methods: {
    updateDescription({ id, description, sections }) {
      const tooth = this.teeth.find(t => t.id === id);
      if (tooth) {
        tooth.description = description;
        tooth.sections = sections;
      }
    },
    generatePDF() {
      const doc = new jsPDF();
      const toothImage = new Image();
      toothImage.src = require('@/assets/img/tooth.png');

      // Encabezado
      doc.setFontSize(22);
      doc.setFont('helvetica', 'bold');
      doc.setTextColor(40, 48, 77);
      doc.text('Diagnóstico Odontograma', 105, 20, null, null, 'center');
      doc.setFontSize(16);
      doc.setFont('helvetica', 'normal');
      doc.text('Dra. Lusmea Anahi Melgarejo de Barrios', 105, 30, null, null, 'center');

      // Información del paciente
      doc.setFontSize(14);
      doc.text(`Nombre del Paciente: ${this.patient.name}`, 10, 50);
      doc.text(`Edad: ${this.patient.age}`, 10, 60);
      doc.text(`Fecha: ${this.currentDate}`, 10, 70);

      let yPos = 90;

      // Dientes con descripciones
      this.teeth.forEach((tooth) => {
        if (tooth.description) {
          doc.addImage(toothImage, 'PNG', 10, yPos, 20, 20);
          doc.text(`Diente ${tooth.number}: ${tooth.description}`, 35, yPos + 10);
          yPos += 30;
        }
      });

      doc.save('odontograma.pdf');
    }
  }
};
</script>

<style>
#app {
  text-align: center;
  font-family: 'Helvetica', sans-serif;
}

main {
  padding: 20px;
}

.patient-info {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
}

.input-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 10px;
}

.input-group label {
  margin-bottom: 5px;
}

footer {
  margin-top: 20px;
  padding: 10px;
  background-color: #f1f1f1;
}
</style>
