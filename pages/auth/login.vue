<template>
  <div class="container">
    <div class="row justify-content-md-center">
      <div
        class="h-100 d-flex align-items-center justify-content-center card col col-lg-5"
      >
        <form class="card-body">
          <h1 class="text-center mb-5">Iniciar sesion</h1>
          <!-- Email input -->
          <div class="form-outline mb-4">
            <label class="form-label" for="form2Example1">Email:</label>
            <input
              type="email"
              id="form2Example1"
              class="form-control"
              v-model="email"
            />
          </div>

          <!-- Password input -->
          <div class="form-outline mb-4">
            <label class="form-label" for="form2Example2">Password:</label>
            <input
              type="password"
              id="form2Example2"
              class="form-control"
              v-model="password"
            />
          </div>

          <!-- 2 column grid layout for inline styling -->
          <div class="row mb-4">
            <div class="col">
              <!-- Simple link -->
              <a href="#!">Forgot password?</a>
            </div>
          </div>

          <!-- Submit button -->
          <button
            type="button"
            class="btn btn-primary btn-block mb-4"
            @click="onSubmit()"
          >
            Iniciar sesion
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const token = useCookie("token");
definePageMeta({
  layout: "1-column",
});

const router = useRouter();

const email = ref("");
const password = ref("");

const onSubmit = () => {
  axios
    .post("http://127.0.0.1:8000/api/login", {
      email: email.value,
      password: password.value,
    })
    .then((res) => {
      token.value = res.data.token;
      if (res.data.user.is_admin == 1) {
        router.push("/");
      } else {
        router.push("/user");
      }
    })
    .catch((err) => {
      console.log(err);
    });
};
</script>

<style></style>
