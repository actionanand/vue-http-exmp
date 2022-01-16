<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && isError">{{ isError }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No stored experience found!, Please add some first.</p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      isError: null
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.isError = null;
      fetch('https://vue-http-exmp-default-rtdb.firebaseio.com/survey.json').then((resp) => {
        if(resp.ok) {
          return resp.json();
        }
      }).then((data) => {
        this.isLoading = false;
        const result = [];
        for(const idx in data) {
          result.push({
            id: idx,
            name: data[idx].name,
            rating: data[idx].rating
          });
        }
        this.results = result;
      }).catch((err) => {
        this.isLoading = false;
        // console.log({err});
        this.isError = 'An Error occurred!, '+err.message;
      });
    }
  },
  mounted() {
    this.loadExperiences();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>