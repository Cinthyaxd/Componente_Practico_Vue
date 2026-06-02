<template>
  <section id="registro" class="form-section">
    <div class="form-wrapper">

      <div v-if="exito" class="success-msg">
        <span class="success-icon">✓</span>
        <strong>¡Registro exitoso!</strong>
        <p>Bienvenido al TechClub. Revisa tu correo para confirmar tu cuenta.</p>
        <button @click="resetear" class="btn-secondary">Registrar otro</button>
      </div>

      <template v-else>
        <div class="form-header">
          <h2>Regístrate</h2>
          <p>Completa el formulario y forma parte de nuestra comunidad.</p>
        </div>
        <form @submit.prevent="enviar" novalidate>

          <div class="form-group">
            <label for="nombre">Nombre completo <span class="req">*</span></label>
            <input type="text" id="nombre" v-model="form.nombre" placeholder="Ej. Ana Torres"
              :class="{ 'input-error': errores.nombre }" />
            <span class="error-msg">{{ errores.nombre }}</span>
          </div>

          <div class="form-group">
            <label for="email">Correo electrónico <span class="req">*</span></label>
            <input type="email" id="email" v-model="form.email" placeholder="tucorreo@ejemplo.com"
              :class="{ 'input-error': errores.email }" />
            <span class="error-msg">{{ errores.email }}</span>
          </div>

          <div class="form-group">
            <label for="password">Contraseña <span class="req">*</span></label>
            <input type="password" id="password" v-model="form.password" placeholder="Mínimo 6 caracteres"
              :class="{ 'input-error': errores.password }" />
            <span class="error-msg">{{ errores.password }}</span>
          </div>

          <div class="form-group">
            <span class="label-group">Nivel de experiencia <span class="req">*</span></span>
            <div class="radio-group">
              <label class="radio-label" v-for="n in niveles" :key="n">
                <input type="radio" name="nivel" :value="n" v-model="form.nivel" />
                {{ n.charAt(0).toUpperCase() + n.slice(1) }}
              </label>
            </div>
            <span class="error-msg">{{ errores.nivel }}</span>
          </div>

          <div class="form-group">
            <span class="label-group">Áreas de interés</span>
            <div class="checkbox-group">
              <label class="check-label" v-for="[val, lbl] in intereses" :key="val">
                <input type="checkbox" :value="val" v-model="form.intereses" />
                {{ lbl }}
              </label>
            </div>
          </div>

          <div class="form-group">
            <label for="fecha">Fecha de nacimiento <span class="req">*</span></label>
            <input type="date" id="fecha" v-model="form.fecha"
              :class="{ 'input-error': errores.fecha }" />
            <span class="error-msg">{{ errores.fecha }}</span>
          </div>

          <div class="form-group">
            <label for="telefono">Teléfono (opcional)</label>
            <input type="tel" id="telefono" v-model="form.telefono" placeholder="Ej. 099-123-4567"
              :class="{ 'input-error': errores.telefono }" />
            <span class="error-msg">{{ errores.telefono }}</span>
          </div>

          <div class="form-group terms-group">
            <label class="check-label">
              <input type="checkbox" v-model="form.terminos" />
              Acepto los <a href="#" class="link">términos y condiciones</a> <span class="req">*</span>
            </label>
            <span class="error-msg">{{ errores.terminos }}</span>
          </div>

          <button type="submit" class="btn-submit">Crear cuenta →</button>
        </form>
      </template>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref } from 'vue'

const niveles = ['principiante', 'intermedio', 'avanzado']
const intereses = [['web','Desarrollo Web'], ['ia','Inteligencia Artificial'], ['seguridad','Ciberseguridad']]

const form = reactive({ nombre: '', email: '', password: '', nivel: '', intereses: [], fecha: '', telefono: '', terminos: false })
const errores = reactive({})
const exito = ref(false)

function validar() {
  Object.keys(errores).forEach(k => delete errores[k])
  if (!form.nombre.trim() || form.nombre.trim().length < 3) errores.nombre = 'El nombre debe tener al menos 3 caracteres.'
  if (!form.email.trim() || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) errores.email = 'Ingresa un correo electrónico válido.'
  if (!form.password || form.password.length < 6) errores.password = 'La contraseña debe tener al menos 6 caracteres.'
  if (!form.nivel) errores.nivel = 'Selecciona tu nivel de experiencia.'
  if (!form.fecha) {
    errores.fecha = 'La fecha de nacimiento es obligatoria.'
  } else {
    const edad = new Date().getFullYear() - new Date(form.fecha).getFullYear()
    if (edad < 10 || new Date(form.fecha) > new Date()) errores.fecha = 'Ingresa una fecha de nacimiento válida.'
  }
  if (form.telefono && !/^[0-9\-]{7,15}$/.test(form.telefono)) errores.telefono = 'Formato de teléfono inválido.'
  if (!form.terminos) errores.terminos = 'Debes aceptar los términos y condiciones.'
  return Object.keys(errores).length === 0
}

function enviar() {
  if (validar()) exito.value = true
}

function resetear() {
  Object.assign(form, { nombre: '', email: '', password: '', nivel: '', intereses: [], fecha: '', telefono: '', terminos: false })
  Object.keys(errores).forEach(k => delete errores[k])
  exito.value = false
}
</script>