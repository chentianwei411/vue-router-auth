<template>
  <div>
    <h4>Login</h4>
    <form>
      <label for="email" >E-Mail Address</label>
      <div>
        <input id="email" type="email" v-model="email" required autofocus></input>
      </div>
      <div>
        <label for="password">Password</label>
        <div>
          <input id="password" type="password" v-model="password" required></input>
        </div>
      </div>
      <div>
        <button type="submit" @click="handleSubmit">Login</button>
      </div>
    </form>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        email: '',
        password: ''
      }
    },
    methods: {
      handleSubmit(e) {
        e.preventDefault()
        if ( this.password.length > 0 ) {
          this.$http.post('http://localhost:3000/login', {
            email: this.email,
            password: this.password
          })
          .then(response => {

          })
          .catch(function(error){
            console.error(error.response)
          })
        }
      }
    }
  }
</script>
