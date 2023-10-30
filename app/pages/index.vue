<template>
  <section class="home">
    <div class="py-24 md:py-36 mx-auto flex flex-wrap flex-col md:flex-row items-center">
      <div class="flex flex-col w-full xl:w-3/5 justify-center lg:items-start overflow-y-hidden">
        <div v-html="$md.render(welcomeText)" class="home__welcome markdown" />

        <div class="mb-12 xl:mb-0">
          <h4 v-if="isPasswordOK">Password correct</h4>


          <form
            v-else
            @submit.prevent="handleSubmit"
            name="passwordEntry"
            netlify
            class="flex items-center border-b border-b-2 border-blue-400 py-2"
          >
            <input
              ref="hoaPasswordInput"
              v-model="form.hoaPassword"
              class="appearance-none mb-36 bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
              type="text"
              name="hoaPassword"
              placeholder="HOA Password"
              aria-label="HOA Password"
            />

            <button
              class="flex-shrink-0 bg-blue-500 hover:bg-blue-700 border-blue-500 hover:border-blue-700 text-sm border-4 text-white py-1 px-2 rounded"
              type="submit"
            >
              Password
            </button>
          </form>



        </div>
      </div>
      <div class="flex flex-col w-full xl:w-2/5">
        <img
          alt="Hero"
          class="rounded shadow-xl"
          src="/images/uploads/kawelabay.jpg"
        />
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import settings from '@/content/settings/general.json';

@Component({
  // Called to know which transition to apply
  transition() {
    return 'slide-left';
  },
})
export default class Home extends Vue {
  welcomeText = settings.welcomeText;

  get posts(): Post[] {
    return this.$store.state.posts;
  }

  isPasswordOK = false;

  form = {
    hoaPassword: '',
  };

  encode(data): string {
    return Object.keys(data)
      .map((key) => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`)
      .join('&');
  }

  validPassword(pw): boolean {
    return (pw == "northshore");
  }

  async handleSubmit(): Promise<void> {

    if (this.validPassword(this.form.hoaPassword)) {
      window.sessionStorage.setItem("isLoggedIn","TRUE");
      window.location.replace("https://wkbca.netlify.app/blog");
      return;
    }

    if (!this.validPassword(this.form.hoaPassword)) {
      this.$refs.hoaPasswordInput.focus();
      return;
    }


    try {
      await fetch('/', {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: this.encode({ 'form-name': 'passwordEntry', ...this.form }),
      });

      this.isPasswordOK = true;
      window.sessionStorage.setItem("isLoggedIn","TRUE");
      window.location.replace("https://wkbca.netlify.app/blog");
    } catch (error) {
      console.error(error);
    }
  }
}
</script>
