<template>
  <div id="container">
    <button class="themeButton" @click="changeTheme">{{ theme }}</button>
    <img v-bind:src="image" v-bind:alt="name" />
    <h1>{{ name }}{{ lastName }}</h1>
    <h2>{{ mail }}</h2>
    <h3>{{ phone }}</h3>
    <button v-bind:class="gender" @click="getUser">New random user</button>
  </div>
</template>

<script>
export default {
  data() {
    const savedCurrentTheme = localStorage.getItem('currentTheme')
    const savedTheme = localStorage.getItem('theme')
    return {
      image: 'https://terminalroot.com.br/assets/img/about/author.jpg',
      name: 'Angelo',
      lastName: 'Danrley',
      mail: 'angelodanrley@example.com',
      phone: '+55 (82) 123456-4074',
      gender: 'male',
      currentTheme: savedCurrentTheme ? savedCurrentTheme : 'default-theme',
      theme: savedTheme ? savedTheme : 'Dark'
    }
  },
  methods: {
    async getUser() {
      // o fetch acessa a api
      const res = await fetch('https://randomuser.me/api')
      // Estamos convertendo em json.
      // A variável não tem nome, mas ela já está retornando o objeto.
      const { results } = await res.json()
      // console.log(resultado)
      this.image = results[0].picture.large
      this.name = results[0].name.first
      this.lastName = results[0].name.last
      this.mail = results[0].email
      this.phone = results[0].phone

      results[0].gender == 'male'
        ? (this.gender = 'male')
        : (this.gender = 'female')
    },
    changeTheme() {
      if (this.currentTheme === 'default-theme') {
        this.currentTheme = 'dark'
        this.theme = 'Light'
      } else {
        this.currentTheme = 'default-theme'
        this.theme = 'Dark'
      }
    },
    validatingClass() {
      const getBody = document.getElementById('default')

      if (this.theme === 'Light') {
        getBody.classList.toggle('dark')
      } else {
        getBody.classList.remove('dark')
      }
    }
  },
  watch: {
    currentTheme(val) {
      localStorage.setItem('currentTheme', val)
      this.validatingClass()
    },
    theme(val) {
      localStorage.setItem('theme', val)
    }
  },
  created() {
    this.validatingClass()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  border: none;
}
html,
body {
  width: 100vw;
  height: 100vh;
}

div#container {
  width: 400px;
  height: 100vh;
  margin: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: Arial, Helvetica, sans-serif;
  color: #333;
}
div#container img {
  border-radius: 50%;
  border: 5px solid silver;
  margin-bottom: 1rem;
  width: 128px;
  height: 128px;
}
h1,
h2 {
  margin-bottom: 4px;
}
h1 {
  font-size: 32px;
}
h2 {
  font-size: 24px;
}
h3 {
  font-size: 16px;
  margin-bottom: 15px;
}
div#container button,
div#container button .themeButton {
  cursor: pointer;
  display: inline-block;
  color: #fff;
  font-size: 16px;
  bottom: 1px solid silver;
  padding: 20px;
}
div#container .themeButton {
  font-weight: bold;

  background-color: #333;
  border-radius: 8px;
  position: absolute;
  top: 16px;
}
.male {
  background-color: steelblue;
}
.female {
  background-color: pink;
}
body.dark,
body.dark #container {
  background-color: #333;
  color: #fff;
}
body.dark #container .themeButton {
  background-color: #fff;
  color: #333;
}
</style>
