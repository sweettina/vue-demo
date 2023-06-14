<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    
    <CustomInput :value.sync="test"  />
    <div>test的值{{test}}</div>
  </div>
</template>

<script>
// @ is an alias to /src

import CustomInput from '@/components/CustomInput'
export default {
  name: 'HomeView',
  components: {
    CustomInput
  },
  data() {
    return {
      
      test:1
    }
  },
  methods: {
    

  },
}
</script>
