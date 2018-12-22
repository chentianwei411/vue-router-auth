<template>
  <div>
    <h4>Register</h4>
    <form>
      <div>
        <label for="name">Name</label>
        <input id="name" type="text" v-model="name" v-validate="'required'" name="name" autofocus />
        <span class="small text-danger" v-show="errors.has('name')">name is required</span>
      </div>
      <div>
        <label for="email">E-mail address</label>
        <input id="email" type="email" v-model="email" v-validate="'required'" name="email" />
        <span class="small text-danger" v-show="errors.has('email')">email is required</span>
      </div>
      <div>
        <label for="password">Password</label>
        <input v-validate="'required'" name="password" type="password" :class="{'is-danger': errors.has('password')}" placeholder="Password" ref="password">
        <span v-show="errors.has('password')" class="help is-danger">{{ errors.first('password') }}</span>
      </div>
      <div>
        <label for="password-confirm">Confirm Password</label>
        <input v-validate="'required|confirmed:password'" name="password_confirmation" type="password" :class="{'is-danger': errors.has('password_confirmation')}" placeholder="Password, Again" data-vv-as="password">
        <span v-show="errors.has('password_confirmation')">{{errors.first('password_confirmation')}}</span>
      </div>
      <div>
        <label for="password-confirm">Is an administrator account?</label>
        <select v-model="is_admin">
          <option value="1">Yes</option>
          <option value="0">No</option>
        </select>
      </div>
      <div>
        <button type="submit" @click="handleSubmit">Register</button>
      </div>
    </form>
  </div>
</template>

<script>
  export default {
    props: ['nextUrl'],
    data() {
      return {
        name: '',
        email: '',
        password: '',
        password_confirm: '',
        is_admin: null
      }
    },
    methods: {
      handleSubmit(e) {
        e.preventDefault()
        this.$validator.validateAll().then((result) => {
          if (result) {
            let url = "http://localhost:3000/register"
            if (this.is_admin != null || this.is_admin == 1) {
              // ???
              url = "http://localhost:3000/register-admin"
            }
            this.$http.post(url, {
              name: this.name,
              email: this.email,
              password: this.password,
              is_admin: this.is_admin,
            })
            .then(response => {
              localStorage.setItem('user', JSON.stringify(response.data.user))
              localStorage.setItem('jwt', response.data.token)

              if (localStorage.getItem('jwt') != null) {
                this.$emit('loggedIn')
                if (this.$route.params.nextUrl != null) {
                  this.$router.push(this.$route.params.nextUrl)
                } else {
                  this.$router.push('/')
                }
              }
            })
            .catch(error => {
              console.error(error);
            })
          } else {
            console.log("Confirm the errors")
          }
        })
      },
    }
  }
</script>
