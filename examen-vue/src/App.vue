<template>
  <div id="app">
    <nav class="navbar">
      <span>Programación Front End</span>
      <button @click="currentTab = 'calificaciones'">Cálculo de calificaciones</button>
      <button @click="currentTab = 'registro'">Formulario de Registro</button>
    </nav>

    <!-- Cálculo de calificaciones -->
    <div v-if="currentTab === 'calificaciones'">
      <h2>Cálculo de Calificaciones</h2>

      <label>Nota 1</label>
      <input type="number" v-model="nota1" placeholder="Nota 1" />

      <label>Nota 2</label>
      <input type="number" v-model="nota2" placeholder="Nota 2" />

      <label>Nota 3</label>
      <input type="number" v-model="nota3" placeholder="Nota 3" />

      <label>Asistencia %</label>
      <input type="number" v-model="asistencia" placeholder="Asistencia" />

      <button @click="calcularPromedio">Calcular</button>

      <div v-if="promedio !== null">
        <p>El promedio es: {{ promedio }}</p>
        <p>Tu estado es: {{ resultado }}</p>
      </div>

      <p class="error" v-if="resultado.includes('❌')">{{ resultado }}</p>
    </div>

    <!-- Formulario de registro -->
    <div v-if="currentTab === 'registro'">
      <h2>Formulario de Registro</h2>

      <label>Nombre completo</label>
      <input type="text" v-model="nombre" placeholder="Ingresa tu nombre" />

      <label>Correo electrónico</label>
      <input type="email" v-model="correo" placeholder="ejemplo@correo.com" />

      <label>Contraseña</label>
      <input type="password" v-model="password" placeholder="Contraseña" />

      <label>Repetir contraseña</label>
      <input type="password" v-model="repetirPassword" placeholder="Repite la contraseña" />

      <button @click="validarRegistro">Registrarse</button>

      <div v-if="errores.length">
        <p class="error" v-for="(error, index) in errores" :key="index">{{ error }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentTab: 'calificaciones',
      nota1: '',
      nota2: '',
      nota3: '',
      asistencia: '',
      promedio: null,
      resultado: '',
      nombre: '',
      correo: '',
      password: '',
      repetirPassword: '',
      errores: []
    }
  },
  methods: {
    calcularPromedio() {
      this.errores = [];
      const n1 = parseFloat(this.nota1);
      const n2 = parseFloat(this.nota2);
      const n3 = parseFloat(this.nota3);
      const asis = parseFloat(this.asistencia);

      if ([n1, n2, n3].some(n => isNaN(n) || n < 10 || n > 70) || isNaN(asis) || asis < 0 || asis > 100) {
        this.resultado = "❌ Por favor, ingrese valores válidos para las notas y la asistencia.";
        this.promedio = null;
        return;
      }

      const promedio = n1 * 0.35 + n2 * 0.35 + n3 * 0.30;
      this.promedio = promedio.toFixed(2);

      if (promedio >= 40 && asis >= 80) {
        this.resultado = "✅ Aprobado";
      } else {
        this.resultado = "❌ Reprobado";
      }
    },

    validarRegistro() {
      this.errores = [];

      if (!this.nombre.trim()) this.errores.push("⚠️ El nombre es requerido.");
      if (!/^\S+@\S+\.\S+$/.test(this.correo)) this.errores.push("⚠️ El correo es inválido.");
      if (!this.password) this.errores.push("⚠️ La contraseña es requerida.");
      if (this.password !== this.repetirPassword) this.errores.push("⚠️ Las contraseñas no coinciden.");

      if (this.errores.length === 0) {
        alert("✅ El registro se ha realizado correctamente.");
      }
    }
  }
}
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  padding: 1rem;
}

.navbar {
  background-color: #ffffff;
  padding: 1rem;
  display: flex;
  gap: 1rem;
  align-items: center;
  border-bottom: 1px solid #ddd;
}

.navbar span {
  font-weight: bold;
  margin-right: auto;
}

.navbar button {
  padding: 0.5rem 1rem;
  background-color: #e1e1e1;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

input {
  display: block;
  margin: 0.5rem 0;
  padding: 0.5rem;
  width: 100%;
  max-width: 300px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.error {
  color: red;
  font-size: 0.9rem;
}
</style>
