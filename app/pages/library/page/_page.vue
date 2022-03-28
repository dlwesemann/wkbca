<script>
import Library from '../index.vue';

if (!window.localStorage.getItem("isLoggedIn")) {
  window.location.replace("https://wkbca.netlify.app/");
}

export default {
  extends: Library,
};
</script>
