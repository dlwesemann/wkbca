<script>
import Library from '../index.vue';

export default {
  beforeCreate() {
     if (!window.localStorage.getItem("isLoggedIn")) {
        window.location.replace("https://wkbca.netlify.app/");
     }
    
  },
  extends: Library,
};
</script>
