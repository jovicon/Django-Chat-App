<template>
  <div class="container">
    <h1 class="text-center">Welcome to Chat</h1>
    <div id="auth-container" class="row">
      <div class="col-sm-4 offset-sm-4">
        <ul class="nav nav-tabs nav-justified" id="myTab" role="tablist">
          <li class="nav-item">
            <a class="nav-link active" id="signup-tab" data-toggle="tab" href="#signup" role="tab"
              aria-controls="signup" aria-selected="true">Sign Up</a>
          </li>

          <li class="nav-item">
            <a id="signin-tab" class="nav-link" href="#signin"
                data-toggle="tab" role="tab" aria-controls="signin" aria-selected="false"
            >Sign In</a>
          </li>
        </ul>

        <div class="tab-content" id="myTabContent">

          <div  id="signup" class="tab-pane fade show active"
                role="tabpanel"
                aria-labelledby="signin-tab">

            <form @submit.prevent="signUp">
              <div class="form-group">

                <input  class="form-control" id="email" v-model="email"
                        type="email"  placeholder="Email Address" required>

              </div>
              <div class="form-row">
                <div class="form-group col-md-6">

                  <input    id="username" v-model="username"
                            type="text" class="form-control"
                            placeholder="Username"
                    required>

                </div>
                <div class="form-group col-md-6">

                    <input  id="password" v-model="password"
                            type="password" class="form-control"
                            placeholder="Password" required>

                </div>
              </div>
              <div class="form-group">
                <div class="form-check">
                  <input class="form-check-input" type="checkbox" id="toc" required>
                  <label class="form-check-label" for="gridCheck">
                    Accept terms and Conditions
                  </label>
                </div>
              </div>
              <button type="submit" class="btn btn-block btn-primary">Sign up</button>
            </form>
          </div>

          <div class="tab-pane fade" id="signin" role="tabpanel" aria-labelledby="signin-tab">
            <form @submit.prevent="signIn">
              <div class="form-group">
                <input  id="username" v-model="username"
                        type="text" class="form-control"
                        placeholder="Username" required>
              </div>
              <div class="form-group">

                <input id="password" v-model="password" type="password" class="form-control"
                  placeholder="Password" required>

              </div>
              <button type="submit" class="btn btn-block btn-primary">Sign in</button>
            </form>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
// const $ = window.jQuery;

export default {
  data() {
    return {
      email: '',
      username: '',
      password: '',
    };
  },
  methods: {
    signUp() {
      const url = 'http://localhost:8000/auth/users/';
      const headers = { 'Content-Type': 'multipart/form-data' };

      this.axios.post(url, this.data, headers)
        .then(() => {
          alert('Your account has been created. You will be signed in automatically');
          this.signIn();
        })
        .catch((response) => {
          alert(response.responseText);
        });
    },
    signIn() {
      const credentials = { username: this.username, password: this.password };
      const url = 'http://localhost:8000/auth/token/login/';
      const headers = { 'Content-Type': 'multipart/form-data' };
      const self = this;

      this.axios.post(url, credentials, headers)
        .then((response) => {
          sessionStorage.setItem('authToken', response.data.auth_token);
          sessionStorage.setItem('username', self.username);
          self.$router.push('/chats');
        })
        .catch((response) => {
          alert(response.responseText);
        });
    },
  },
};
</script>

<style>
  #auth-container {
    margin-top: 50px;
  }

  .tab-content {
    padding-top: 20px;
  }
</style>
