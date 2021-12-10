<template>
    <div id="header">
            <p style="display: flex; justify-content: flex-end;">{{ user }}</p>
    </div>
</template>

<script>
export default {
  name: 'HeaderBar',
  props: {
    user: String
  }
}
</script>