<template>
  <div id="pokemon">
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ num }} - {{ name | upper }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>

        <div class="content">
          <p class="subtitle is-5">Habilidades</p>

          <p class="title is-6">
            {{ pokemon.ability1 }}
            <br />
            {{ pokemon.ability2 }}
          </p>
          <button class="button is-medium is-fullwidth" @click="mudarSprite">
            Mudar Sprite
          </button>
          <button
            class="button is-medium is-fullwidth"
            id="btn"
            @click="mudarForma"
          >
            Mudar para Shiny
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.pokemon.shinyFront = res.data.sprites.front_shiny;
      this.pokemon.shinyBack = res.data.sprites.back_shiny;
      this.pokemon.ability1 = res.data.abilities[0].ability.name;
      this.pokemon.ability2 = res.data.abilities[1].ability.name;

      this.currentImg = this.pokemon.front;

      console.log(res);
    });
  },

  data() {
    return {
      isNormal: true,
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
        shinyFront: "",
        shinyBack: "",
        ability1: "",
        ability2: "",
      },
    };
  },

  props: {
    num: Number,
    name: String,
    url: String,
    abilities: String,
  },
  filters: {
    upper: function (value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  methods: {
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
    mudarForma: function () {
      if (this.isNormal) {
        this.isNormal = false;
        this.currentImg = this.pokemon.shinyFront;
      } else {
        this.isNormal = true;
        this.currentImg = this.pokemon.shinyBack;
      }
    },
  },
};
</script>

<style>
#pokemon {
  margin-top: 1%;
  float: left;
  margin: 1%;
  margin-left: 4%;
}

#btn {
  margin-top: 2%;
}
</style>