<template>
    <div id="header">
            <p style="text-align: right;">{{ user }}</p>
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