<template>
  <div>
    <img class="logoPoke" src="./assets/pokemon-logo.jpg" alt="Logo-Pokemon" />
    <h1>PokeGuía</h1>
    <div class="form">
      <label for="">Nombre: </label>
      <input @keydown.enter="cargar" v-model="name" type="text" />
      <button @click="cargar">Buscar</button>
    </div>
    <div v-if="condicion" class="poke-data">
      <img :src="pokefoto" />
      <h2>Movimientos</h2>
      <ul>
        <li v-for="(movimiento, index) in movimietosPokemon" :key="index">
          {{ movimiento.move.name }}
        </li>
      </ul>

      <h2>Habilidades</h2>
      <ul>
        <li v-for="(habilidad, index) in habilidadesPokemon" :key="index">
          {{ habilidad.ability.name }}
        </li>
      </ul>
    </div>
    <p class="error" v-else>Pokemon no encontrado...</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "pikachu",
      pokedata: {},
      condicion: true,
    };
  },
  methods: {
    cargar() {
      fetch(`https://pokeapi.co/api/v2/pokemon/${this.name}`)
        .then((response) => response.json())
        .then((data) => {
          this.condicion = true;
          this.pokedata = data;
        })
        .catch((error) => {
          this.condicion = false;
          this.error = error;
        });
    },
  },
  created() {
    this.cargar();
  },
  computed: {
    pokefoto() {
      if (!Object.prototype.hasOwnProperty.call(this.pokedata, "sprites")) {
        return;
      }

      return this.pokedata.sprites.front_shiny;
    },
    
    movimietosPokemon() {
      if (!Object.prototype.hasOwnProperty.call(this.pokedata, "moves")) {
        return;
      }

      let five = this.pokedata.moves.slice(0, 5);
      return five;
    },

    habilidadesPokemon() {
      if (!Object.prototype.hasOwnProperty.call(this.pokedata, "abilities")) {
        return;
      }

      return this.pokedata.abilities;
    },
    
  },
};
</script>

<style>
body {
  font-family: "Montserrat", sans-serif;
}

.logoPoke {
  width: 30%;
  display: block;
  margin: 0 auto;
}

h1,
h2 {
  text-align: center;
}

h2 {
  margin-top: 0;
}

.form {
  text-align: center;
}

button {
  margin-left: 20px;
}

.poke-data {
  text-align: center;
}

ul {
  list-style: none;
  padding: 0;
}

ul li {
  margin-bottom: 0.5rem;
}

.poke-data img {
  width: 200px;
}

.error {
  margin: 3rem;
  text-align: center;
  font-size: 18px;
}
</style>