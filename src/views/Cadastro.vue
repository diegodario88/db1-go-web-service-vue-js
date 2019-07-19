<template>
  <div id="app">
    <form>
      <v-flex xs12 sm6 offset-sm3>
        <v-card>
          <v-card-title primary-title>
            <div>
              <div class="headline">Fill Stock Form</div>
              <span class="grey--text">send up some data for us...</span>
            </div>
          </v-card-title>
          <v-text-field
            v-model="name"
            v-validate="'required|max:15'"
            :counter="15"
            :error-messages="errors.collect('name')"
            label="Stock name"
            data-vv-name="name"
            required
          ></v-text-field>
          <v-text-field
            v-model="start"
            v-validate="'numeric|required'"
            :error-messages="errors.collect('start')"
            label="Price Start"
            data-vv-name="start"
            required
          ></v-text-field>
          <v-text-field
            v-model="end"
            v-validate="'numeric|required'"
            :error-messages="errors.collect('end')"
            label="Price End"
            data-vv-name="end"
            required
          ></v-text-field>

          <v-date-picker v-model="period" full-width landscape class="mt-3"></v-date-picker>
        </v-card>
        <v-btn @click="postStock">submit</v-btn>
        <v-btn @click="clear">clear</v-btn>
      </v-flex>
    </form>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import VeeValidate from "vee-validate";
import Axios from "axios";

Vue.use(VeeValidate);

export default {
  $_veeValidate: {
    validator: "new"
  },

  data: () => ({
    name: "",
    start: null,
    end: null,
    period: "",

    dictionary: {
      attributes: {
        start: "Stock start price",
        end: "Stock start price"
        // custom attributes
      },
      custom: {
        name: {
          required: () => "Name can not be empty",
          max: "The name field may not be greater than 15 characters"
          // custom messages
        },
        start: {
          required: () => "Start can not be empty"
        },
        end: {
          required: () => "End can not be empty"
        }
      }
    }
  }),

  mounted() {
    this.$validator.localize("en", this.dictionary);
  },

  methods: {
    submit() {
      this.$validator.validateAll();
    },
    postStock() {
      this.$validator.validateAll().then(result => {
        if (!result) {
          alert("error");
          return;
        }
        const stock = {
        "name": this.name,
        "value": {
            "start": this.start,
            "end": this.end
        },
        "date": this.period
    };
        const url = "https://damp-bayou-25011.herokuapp.com/api/v1/stock";
        Axios({
          method: "POST",
          url,
          data: stock,
          headers: { "Content-Type": "application/json" }
        })
          .then(response => {
            console.log(response);
          })
          .catch(error => {
            console.log(error.response);
          });
        alert("Success!");
      });
    },

    clear() {
      this.name = "";
      this.start = null;
      this.end = null;
      this.period = "";
      this.$validator.reset();
    }
  }
};
</script>

<style lang="sass" scoped>

</style>
