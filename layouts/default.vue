<template>
  <div>
    <nuxt/>
    <UserTable />
  </div>
</template>

<style>

</style>
<script>
  import UserTable from "../components/ModalWindow";
  export default {
    name: 'default',
    components: {UserTable}
  }
</script>
