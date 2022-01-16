<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <ul>
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
      results: []
    };
  },
  methods: {
    loadExperiences() {
      fetch('https://vue-http-exmp-default-rtdb.firebaseio.com/survey.json').then((resp) => {
        if(resp.ok) {
          return resp.json();
        }
      }).then((data) => {
        const result = [];
        for(const idx in data) {
          result.push({
            id: idx,
            name: data[idx].name,
            rating: data[idx].rating
          });
        }
        this.results = result;
      });
    }
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