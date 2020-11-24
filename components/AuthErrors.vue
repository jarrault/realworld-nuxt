<template>
  <div>
    <ul v-for="error in formattedErrors" :key="error" class="error-messages">
      <li>{{ error }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    errors: {
      type: Object,
      default: null
    }
  },
  computed: {
    formattedErrors () {
      if (this.errors) {
        const errors = []
        for (const [key, value] of Object.entries(this.errors)) {
          for (const message of value) {
            errors.push(`${key} ${message}`)
          }
        }
        return errors
      }

      return ''
    }
  }
}
</script>
