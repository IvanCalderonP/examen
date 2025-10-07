<template>
  <div id="app">
    <form class="formulario" @submit.prevent="checkForm">
      <h2>Registro de Usuario</h2>
      <div v-if="errors.length" class="errors">
        <ul>
          <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
        </ul>
      </div>

      <div class="campo">
        <label for="name">Nombre completo:</label>
        <input type="text" id="name" v-model="name" placeholder="Ej: Juan Pérez" />
      </div>

      <div class="campo">
        <label for="address">Dirección:</label>
        <input type="text" id="address" v-model="address" placeholder="Ej: carretera hacia santa ana casa #3" />
      </div>

      <div class="campo">
        <label for="age">Edad:</label>
        <input type="number" id="age" v-model="age" placeholder="Ej: 25" />
      </div>

      <button type="submit">Enviar</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      address: '',
      age: '',
      errors: []
    }
  },
  methods: {
    // Validación del nombre
    validateName() {
      if (this.name && this.name.trim().length > 0) {
        return this.name.trim().length >= 2 &&
          this.name.trim().length <= 50 &&
          /^[a-zA-ZáÁéÉíÍóÓúÚüÜñÑ\s]+$/.test(this.name);
      }
      return false;
    },

    // Validación de la dirección
    validateAddress() {
      if (this.address && this.address.trim().length > 0) {
        return this.address.trim().length >= 10 &&
          this.address.trim().length <= 100;
      }
      return false;
    },

    // Validación de la edad
    validateAge() {
      if (this.age) {
        const ageNum = Number(this.age);
        return Number.isInteger(ageNum) &&
          ageNum >= 18 &&
          ageNum <= 100;
      }
      return false;
    },

    // Validación general del formulario
    checkForm(e) {
      this.errors = [];

      // Validación del nombre
      if (!this.name) {
        this.errors.push('El nombre completo es obligatorio.');
      } else if (this.name.trim().length < 2) {
        this.errors.push('El nombre debe tener al menos 2 caracteres.');
      } else if (this.name.trim().length > 50) {
        this.errors.push('El nombre no puede exceder 50 caracteres.');
      } else if (!/^[a-zA-ZáÁéÉíÍóÓúÚüÜñÑ\s]+$/.test(this.name)) {
        this.errors.push('El nombre solo puede contener letras y espacios.');
      }

      // Validación de la dirección
      if (!this.address) {
        this.errors.push('La dirección es obligatoria.');
      } else if (this.address.trim().length < 10) {
        this.errors.push('La dirección debe tener al menos 10 caracteres.');
      } else if (this.address.trim().length > 100) {
        this.errors.push('La dirección no puede exceder 100 caracteres.');
      }

      // Validación de la edad
      if (!this.age) {
        this.errors.push('La edad es obligatoria.');
      } else if (Number(this.age) < 18) {
        this.errors.push('Debe ser mayor de edad (18 años o más).');
      } else if (Number(this.age) > 100) {
        this.errors.push('La edad no puede ser mayor a 100 años.');
      } else if (!Number.isInteger(Number(this.age))) {
        this.errors.push('La edad debe ser un número entero.');
      }

      if (this.errors.length) {
        e.preventDefault();
        return false;
      }
      return true;
    }
  }
}
</script>

<style>
body {
  background: #f4f6fb;
  margin: 0;
  padding: 0;
}

#app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.formulario {
  background: #fff;
  padding: 2.5rem 2rem;
  border-radius: 16px;
  box-shadow: 0 4px 24px rgba(44, 62, 80, 0.12);
  width: 100%;
  max-width: 400px;
  box-sizing: border-box;
}

.formulario h2 {
  margin-bottom: 1.5rem;
  color: #2c3e50;
  font-weight: 700;
  text-align: center;
}

.campo {
  margin-bottom: 1.2rem;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.campo label {
  margin-bottom: 0.4rem;
  color: #34495e;
  font-weight: 500;
}

.campo input {
  width: 100%;
  padding: 0.6rem 0.8rem;
  border: 1px solid #bfc9d1;
  border-radius: 6px;
  font-size: 1rem;
  transition: border 0.2s;
  background: #f8fafc;
}

.campo input:focus {
  border-color: #3498db;
  outline: none;
  background: #fff;
}

button[type="submit"] {
  width: 100%;
  padding: 0.8rem;
  background: #3498db;
  color: #fff;
  border: none;
  border-radius: 6px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
  margin-top: 0.5rem;
}

button[type="submit"]:hover {
  background: #217dbb;
}

.errors {
  background: #ffeaea;
  border: 1px solid #ffb3b3;
  color: #c0392b;
  border-radius: 6px;
  padding: 0.8rem 1rem;
  margin-bottom: 1.2rem;
  font-size: 0.98rem;
}

.errors ul {
  margin: 0;
  padding-left: 1.2rem;
}

@media (max-width: 500px) {
  .formulario {
    padding: 1.2rem 0.5rem;
    max-width: 95vw;
  }
}
</style>