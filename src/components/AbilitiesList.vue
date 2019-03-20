<template>
  <div id="abilities">
    <ol>
      <li v-for="object in abilities">{{object.ability.name}}</li>
    </ol>
  </div>
</template>

<script>
export default {
  name: 'AbilitiesList',
  props: ['abilities'],
}
</script>