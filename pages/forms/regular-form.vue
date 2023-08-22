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
              <div class="alert alert-danger" v-if="errors">
                {{ mensajeError }}
              </div>
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
              <form @submit.prevent="onSubmit" name="empresaRegistro">
                <div class="mb-3">
                  <label for="nombreEmpresa" class="form-label"
                    >Nombre de la empresa</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="nombreEmpresa"
                    v-model="nombreEmpresa"
                  />
                  <div class="text-danger">{{ errorNombre }}</div>
                </div>
                <div class="mb-3">
                  <label for="rif" class="form-label">Rif</label>
                  <input
                    type="text"
                    class="form-control"
                    id="rif"
                    v-model="rif"
                  />
                  <div class="text-danger">{{ errorRif }}</div>
                </div>
                <div class="mb-3">
                  <label for="email" class="form-label">Email</label>
                  <input
                    type="email"
                    class="form-control"
                    id="email"
                    v-model="email"
                  />
                  <div class="text-danger">{{ errorEmail }}</div>
                </div>
                <div class="mb-3">
                  <label for="emailSecondary" class="form-label"
                    >Email alternativo</label
                  >
                  <input
                    type="email"
                    class="form-control"
                    id="emailSecondary"
                    v-model="emailSecondary"
                  />
                  <div class="text-danger">{{ errorEmailSecondary }}</div>
                </div>
                <div class="mb-3">
                  <label for="phone" class="form-label"
                    >Numero de telefono</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="phone"
                    v-model="phone"
                  />
                  <div class="text-danger">{{ errorPhone }}</div>
                </div>
                <div class="mb-3">
                  <label for="phoneSecondary" class="form-label"
                    >Numero de telefono alternativo</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="phoneSecondary"
                    v-model="phoneSecondary"
                  />
                  <div class="text-danger">{{ errorPhoneSecondary }}</div>
                </div>

                <div class="mb-3">
                  <label for="pais" class="form-label">Pais</label>
                  <select
                    class="form-select form-select-md mb-3"
                    aria-label=".form-select-lg example"
                    v-model="country"
                  >
                    <!-- <option selected v-if="nombrePais != ''">
                      {{ nombrePais }}
                    </option> -->
                    <option
                      v-for="country in countries"
                      :key="country.id"
                      :value="country"
                    >
                      {{ country.name }}
                    </option>
                  </select>
                  <div class="text-danger">{{ errorPais }}</div>
                </div>

                <div class="mb-3" v-if="country.name != ''">
                  <label for="estado" class="form-label">Estado</label>
                  <select
                    class="form-select form-select-md mb-3"
                    aria-label=".form-select-lg example"
                    v-model="state"
                  >
                    <option
                      v-for="state in states"
                      :key="state.id"
                      :value="state.name"
                    >
                      {{ state.name }}
                    </option>
                  </select>
                  <div class="text-danger">{{ errorEstado }}</div>
                </div>

                <div class="mb-3">
                  <label for="direccion" class="form-label">Direccion</label>
                  <input
                    type="text"
                    class="form-control"
                    id="Direccion"
                    v-model="direccion"
                  />
                  <div class="text-danger">{{ errorDireccion }}</div>
                </div>

                <button
                  type="button"
                  class="btn btn-primary"
                  @click="onSubmit()"
                >
                  Registrar
                </button>
              </form>
            </div>
          </div>
        </section>
      </div>
    </client-only>
  </suspense>
</template>

<script lang="ts">
import axios from "axios";

export default {
  name: "RegularForm",
  data() {
    return {
      countries: [{ name: " ", iso2: String, id: Number }],
      country: { name: "", iso2: String, id: Number },
      states: [{ name: String, iso2: String, id: Number }],
      state: "Elige un estado",
      nombreEmpresa: "",
      rif: "",
      email: "",
      emailSecondary: "",
      phone: "",
      phoneSecondary: "",
      direccion: "",
      errors: false,
      mensajeError: " ",
      errorNombre: " ",
      errorRif: " ",
      errorEmail: " ",
      errorEmailSecondary: " ",
      errorPhone: " ",
      errorPhoneSecondary: " ",
      errorPais: " ",
      errorEstado: " ",
      errorDireccion: " ",
      nombrePais: " ",
    };
  },
  async mounted() {
    const config = useRuntimeConfig();
    const token = useCookie("token");
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
    try {
      const response = await axios.get(
        "http://127.0.0.1:8000/api/dashboard/empresa",
        {
          headers: {
            Authorization: `Bearer ${token.value}`,
          },
        }
      );
      this.nombreEmpresa = response.data.empresa.nombreEmpresa;
      this.rif = response.data.empresa.rif;
      this.email = response.data.empresa.email;
      this.emailSecondary = response.data.empresa.emailSecondary;
      this.phone = response.data.empresa.phone;
      this.phoneSecondary = response.data.empresa.phoneSecondary;
      this.country.name = response.data.empresa.pais;
      this.nombrePais = this.country.name;
      console.log(this.$refs.country);
      this.state = response.data.empresa.estado;
      this.direccion = response.data.empresa.direccion;
    } catch (error) {}
  },
  methods: {
    async getStates() {
      const config = useRuntimeConfig();
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
    async onSubmit() {
      const token = useCookie("token");

      console.log(token.value);
      try {
        const response = await axios.post(
          "http://127.0.0.1:8000/api/dashboard/form",
          {
            nombreEmpresa: this.nombreEmpresa,
            rif: this.rif,
            email: this.email,
            emailSecondary: this.emailSecondary,
            phone: this.phone,
            phoneSecondary: this.phoneSecondary,
            pais: this.country.name,
            estado: this.state,
            direccion: this.direccion,
          },
          {
            headers: {
              Authorization: `Bearer ${token.value}`,
            },
          }
        );
        if (response.data.empresa) {
          this.$router.push("/");
        } else {
          throw new Error(response.data);
        }
      } catch (error: any) {
        this.errors = true;
        console.log(error.response);
        this.mensajeError = error.response.data.message;
        this.errorNombre = error.response.data.errors.nombreEmpresa[0];
        this.errorRif = error.response.data.errors.rif[0];
        this.errorEmail = error.response.data.errors.email[0];
        this.errorEmailSecondary = error.response.data.errors.emailSecondary[0];
        this.errorPhone = error.response.data.errors.phone[0];
        this.errorPhoneSecondary = error.response.data.errors.phoneSecondary[0];
        this.errorPais = error.response.data.errors.pais[0];
        this.errorEstado = error.response.data.errors.estado[0];
        this.errorDireccion = error.response.data.errors.direccion[0];
      }
    },
  },
  watch: {
    country: function () {
      this.getStates();
    },
  },
};
</script>
