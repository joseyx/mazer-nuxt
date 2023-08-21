<template>
  <suspense>
    <client-only>
      <div class="page-heading">
        <div class="page-title">
          <div class="row">
            <div class="col-12 col-md-6 order-md-1 order-last">
              <h3>Formulario</h3>
              <p class="text-subtitle text-muted">
                Registro o actualizacion de empresa.
              </p>
            </div>
            <div class="col-12 col-md-6 order-md-2 order-first">
              <nav
                aria-label="breadcrumb"
                class="breadcrumb-header float-start float-lg-end"
              >
                <ol class="breadcrumb">
                  <li class="breadcrumb-item">
                    <nuxt-link to="/">Dashboard</nuxt-link>
                  </li>
                  <li class="breadcrumb-item active" aria-current="page">
                    Regular-form
                  </li>
                </ol>
              </nav>
            </div>
          </div>
        </div>
        <section class="section">
          <div class="card">
            <div class="card-header">
              <h4 class="card-title">Formulario</h4>
            </div>
            <div class="card-body">
              <form>
                <div class="mb-3">
                  <label for="nombreEmpresa" class="form-label"
                    >Nombre de la empresa</label
                  >
                  <input type="text" class="form-control" id="nombreEmpresa" />
                </div>
                <div class="mb-3">
                  <label for="rif" class="form-label">Rif</label>
                  <input type="text" class="form-control" id="rif" />
                </div>
                <div class="mb-3">
                  <label for="email" class="form-label">Email</label>
                  <input type="email" class="form-control" id="email" />
                </div>
                <div class="mb-3">
                  <label for="emailSecondary" class="form-label"
                    >Email alternativo</label
                  >
                  <input
                    type="email"
                    class="form-control"
                    id="emailSecondary"
                  />
                </div>
                <div class="mb-3">
                  <label for="phone" class="form-label"
                    >Numero de telefono</label
                  >
                  <input type="text" class="form-control" id="phone" />
                </div>
                <div class="mb-3">
                  <label for="phoneSecondary" class="form-label"
                    >Numero de telefono alternativo</label
                  >
                  <input type="text" class="form-control" id="phoneSecondary" />
                </div>

                <div class="mb-3">
                  <label for="pais" class="form-label">Pais</label>
                  <select
                    class="form-select form-select-md mb-3"
                    aria-label=".form-select-lg example"
                    v-model="country"
                    v-bind="countries"
                  >
                    <option selected value="" disabled>Elige un pais</option>
                    <option
                      v-for="country in countries"
                      :key="country.id"
                      :value="country"
                    >
                      {{ country.name }}
                    </option>
                  </select>
                </div>

                <div class="mb-3">
                  <label for="estado" class="form-label">Estado</label>
                  <select
                    class="form-select form-select-md mb-3"
                    aria-label=".form-select-lg example"
                    v-model="state"
                    v-bind="states"
                  >
                    <option selected value="" disabled>Elige un estado</option>
                    <option
                      v-for="state in states"
                      :key="state.id"
                      :value="state"
                    >
                      {{ state.name }}
                    </option>
                  </select>
                </div>

                <div class="mb-3">
                  <label for="direccion" class="form-label">Direccion</label>
                  <input type="text" class="form-control" id="Direccion" />
                </div>

                <button type="submit" class="btn btn-primary">Registrar</button>
              </form>
            </div>
          </div>
        </section>
      </div>
    </client-only>
  </suspense>
</template>

<script>
import axios from "axios";

export default {
  name: "RegularForm",
  data() {
    return {
      countries: [],
      country: {},
      states: [],
      state: "",
      iso: "",
    };
  },
  async mounted() {
    const config = useRuntimeConfig();
    await axios
      .get("https://api.countrystatecity.in/v1/countries", {
        headers: {
          "X-CSCAPI-KEY": config.public.countriesKey,
        },
      })
      .then((response) => {
        this.countries = response.data;
      })
      .catch((error) => {
        console.log(error.response);
      });
  },
  methods: {
    async getStates() {
      const config = useRuntimeConfig();
      console.log(this.$config.API_KEY);
      await axios
        .get(
          `https://api.countrystatecity.in/v1/countries/${this.country.iso2}/states`,
          {
            headers: {
              "X-CSCAPI-KEY": config.public.countriesKey,
            },
          }
        )
        .then((response) => {
          this.states = response.data;
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
  watch: {
    country: function () {
      this.getStates();
    },
  },
};
</script>
