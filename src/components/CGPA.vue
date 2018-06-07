<template>
  <div>
      <div class="nav">
        <div class="branch">
        <label>Branch</label>
        <button @click="selectedCourse='it',activeIT=true" :class="[activeIT ? 'active' : '']">IT</button>
        <button @click="selectedCourse='ece',activeIT=false" :class="[activeIT ? '' : 'active']">ECE</button>
        </div>
    <div class="semester">
        <label>Semesters Completed</label>
        <select v-model.number="selectedSemester" class="smaller">
        <option v-for="i in 8" :value="i" :key="i">{{i}} Done</option>
        </select>
        </div>
    </div>
    <div class="course-list">
        <div class="courseitem small" v-for="i in selectedSemester" :key="i">
            <p>Semester {{i}}</p>
            <input type="number" step=0.01 v-model="sgpa[i]" :placeholder="placeholder(i)" max="10" min="0">
        </div>
    </div>
    <hr v-if="totalScore">
    <div class="verdict" v-if="totalScore">
    <h3>{{totalScore}}<span class="outta" v-if="totalScore">/10</span></h3>
    </div>
  </div>
</template>

<script>
import credits from "../../static/credits.json";
export default {
  name: "CGPA",
  data() {
    return {
      credits,
      activeIT: true,
      selectedCourse: "it",
      selectedSemester: 4,
      sgpa: []
    };
  },
  methods: {
    getSemCredit(sem) {
      let course = this.selectedCourse === "it" ? 0 : 1;
      return parseInt(credits["s" + sem][course]);
    },
    calc(num) {
      let numstr = num.toString();
      numstr = numstr.slice(0, numstr.indexOf(".") + 3);
      return Number(numstr);
    },
    placeholder(i) {
      return "GPA of " + "Semester " + i;
    }
  },
  watch: {
    selectedSemester: function() {
      this.sgpa = [];
    }
  },
  computed: {
    totalScore() {
      let totalCredits = 0;
      let obtainedCredits = 0;
      let i;
      for (i = 1; i <= this.selectedSemester; i++) {
        totalCredits += this.getSemCredit(i);
        obtainedCredits += this.getSemCredit(i) * this.sgpa[i];
      }
      let cg = obtainedCredits / totalCredits;
      return this.calc(cg);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.small {
  justify-content: center;
}
.smaller {
  background-position: 83% center !important;
}
</style>
