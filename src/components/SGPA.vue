<template>
  <div>
    <div class="nav">
        <div class="branch">
        <label>Branch</label>
        <button @click="selectedCourse='it', activeIT=true" :class="[activeIT ? 'active' : '']">IT</button>
        <button @click="selectedCourse='ece', activeIT=false" :class="[activeIT ? '' : 'active']">ECE</button>
        </div>
    <div class="semester">
        <label>Semester</label>
        <select v-model.number="selectedSemester">
        <option v-for="i in 8" :value="i-1" :key="i">Semester {{i}}</option>
        </select>
        </div>
    </div>
    <table class="course-list">
    <tr v-for="(course,index) in course[selectedSemester]" :key="course.id" class="course">
      <td class="name">{{course.courseName}}</td>
        <td :colspan="course.theoryCredits&&course.labCredits?'':2" v-if="course.theoryCredits">
          <input type="text" placeholder="Theory Grade" v-model="theoryGrades[index]" >
        </td>
        <td :colspan="course.theoryCredits&&course.labCredits?'':2" v-if="course.labCredits">
          <input type="text" placeholder="Lab Grade" v-model="labGrades[index]"  >
        </td>
        <td colspan=2 v-if="course.projectCredits">
          <input type="text" placeholder="Project Grade" v-model="projectGrade" :change="projectScore=course.projectCredits">
        </td>
    </tr>
    </table>
    <hr v-if="totalScore">
    <div class="verdict" v-if="totalScore">
    <h4>{{showMessage}}</h4>
    <h3>{{animatedResult}}<span class="outta" v-if="totalScore">/10</span></h3>
    </div>
  </div>
</template>

<script>
import it from "../../static/btech-it.json";
import ece from "../../static/btech-ece.json";
export default {
  name: "SGPA",
  data() {
    return {
      it,
      ece,
      activeIT: true,
      selectedCourse: "it",
      selectedSemester: 0,
      theoryGrades: [],
      labGrades: [],
      projectScore: 0,
      projectGrade: "",
      tweenedNumber: 0
    };
  },
  methods: {
    getScore(grade) {
      switch (grade) {
        case "A+":
        case "a+":
          return 10;
          break;
        case "A":
        case "a":
          return 9;
          break;
        case "B+":
        case "b+":
          return 8;
          break;
        case "B":
        case "b":
          return 7;
          break;
        case "C":
        case "c":
          return 6;
          break;
        case "D":
        case "d":
          return 5;
          break;
        default:
          return 0;
      }
    },
    calc(num) {
      let numstr = num.toString();
      numstr = numstr.slice(0, numstr.indexOf(".") + 3);
      return Number(numstr);
    }
  },
  computed: {
    animatedResult() {
      return this.tweenedNumber.toFixed(2);
    },
    course() {
      return this[this.selectedCourse];
    },
    theoryScore() {
      let score = 0;
      this.theoryGrades.forEach(el => {
        score += this.getScore(el);
      });
      return score * 3;
    },
    labScore() {
      let score = 0;
      this.labGrades.forEach(el => {
        score += this.getScore(el);
      });
      return score * 2;
    },
    totalScore() {
      let labCredits = 0;
      this.labGrades.forEach(el => {
        if (el !== null) {
          labCredits += 1;
        }
      });

      let theoryCredits = 0;
      this.theoryGrades.forEach(el => {
        if (el !== null) {
          theoryCredits += 1;
        }
      });

      let totalCredits = theoryCredits * 3 + labCredits * 2 + this.projectScore;
      let cg =
        (this.labScore +
          this.theoryScore +
          this.getScore(this.projectGrade) * this.projectScore) /
        totalCredits;
      return isNaN(cg) || cg === 0 ? null : this.calc(cg);
    },
    showMessage() {
      if (this.totalScore <= 10 && this.totalScore > 9) {
        return "Teach me, Master 🙏🏻";
      } else if (this.totalScore <= 9 && this.totalScore > 8) {
        return " Machaya! Decent Score 🍻";
      } else if (this.totalScore <= 8 && this.totalScore > 7) {
        return " Push yourself, just a little 🙌";
      } else if (this.totalScore <= 7 && this.totalScore > 6) {
        return "Need to work harder 🔨";
      } else if (this.totalScore <= 6 && this.totalScore > 5) {
        return "Beta tumse na ho payega 😂";
      } else {
        return "You entered a wrong value 😪";
      }
    }
  },
  watch: {
    selectedSemester: function() {
      this.theoryGrades = [];
      this.labGrades = [];
      this.projectScore = 0;
      this.projectGrade = "";
    },
    selectedCourse: function() {
      this.theoryGrades = [];
      this.labGrades = [];
      this.projectScore = 0;
      this.projectGrade = "";
    },
    totalScore: function(newValue) {
      TweenLite.to(this.$data, 0.5, { tweenedNumber: newValue });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
html {
  font-size: 100%;
}
.branch,
.semester {
  display: flex;
  flex-direction: row;
}
.nav,
.courseitem,
.verdict {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.course-list {
  margin: 1rem 0;
  width: 100%;
  tr {
    padding: 0 !important;

    .name {
      padding: 0.5rem 1.25rem;
    }
  }
  td {
    input {
      width: 100%;
      box-sizing: border-box;
      margin: 0 !important;
    }
  }
  td:not(.name) {
    padding: 0.5rem 0.625rem;
  }
}
hr {
  height: 0px;
  border: 1px solid #f7f7f7;
}
.verdict {
  padding: 2rem;
  align-items: center;
  justify-content: space-around;
  h4 {
    font-family: "Nunito Sans";
    font-size: 1.5rem;
    color: #a6a6a6;
  }
  h3 {
    font-family: "Nunito Sans";
    font-weight: 700;
    font-size: 4rem;
    color: #2e86fe;
    .outta {
      font-family: "Nunito Sans";
      font-size: 1rem;
      font-weight: normal;
      margin-left: 0.5rem;
      color: #7c95aa;
    }
  }
}
.course,
.courseitem {
  background-color: #fff;
  padding: 0.5rem 1.5rem;
  align-items: center;
  .name,
  p {
    font-family: "Nunito Sans";
    font-size: 1rem;
    line-height: 1.5rem;
    color: #606060;
  }
  input {
    margin: 0 0.5rem;
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
    font-family: "Nunito Sans";
    font-weight: 700;
    font-size: 0.75rem;
    color: #606060;
    border: 1px solid #ececec;
    border-radius: 3px;
    text-align: center;
    &:focus {
      border: 1px solid #2d85fd;
      outline: none;
    }
  }
  input[type="number"] {
    min-width: 25%;
    margin-left: 1.5rem;
  }
  input:-ms-input-placeholder {
    font-family: "Nunito Sans";
    font-size: 12px;
    font-weight: normal;
    text-align: center;
    color: #c1c1c1;
  }
  input::-webkit-input-placeholder {
    font-family: "Nunito Sans";
    font-size: 12px;
    color: #c1c1c1;
    font-weight: normal;
    text-align: center;
  }
  input::-moz-placeholder {
    font-family: "Nunito Sans";
    font-size: 12px;
    color: #c1c1c1;
    font-weight: normal;
    text-align: center;
  }
}
.nav {
  position: relative;
  top: -44px;
  border-radius: 4px 4px 0 0;
  padding: 1.5rem;
  label {
    margin-right: 1rem;
    font-family: "Nunito Sans";
    font-weight: 600;
    font-size: 1rem;
    color: #606060;
  }
  background-color: #f9f9f9;
  .branch {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    button {
      font-family: "Nunito Sans";
      font-size: 0.75rem;
      color: #7c95aa;
      padding: 0.5rem 1.25rem;
      line-height: 12px;
      background: #ffffff;
      border: 1px solid #ececec;
      border-radius: 100px;
      margin-right: 0.75rem;
      cursor: pointer;
      transition-timing-function: ease-in-out;
      transition-duration: 0.3s;
      transition-property: all;
      &:focus {
        outline: none;
      }
    }
    .active {
      background-color: #2d85fd;
      color: #fff;
      font-family: "Nunito Sans";
      font-weight: 700;
      letter-spacing: 1px;
    }
  }
  .semester {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    select {
      font-size: 0.75rem;
      font-family: "Nunito Sans";
      font-weight: 700;
      color: #2d85fd;
      background-color: #fff;
      padding: 0.5rem 1.5rem;
      border: 1px solid #ececec;
      border-radius: 100px;
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      background-image: url("../../static/dropdown.svg");
      background-position: 88% center;
      background-repeat: no-repeat;
      &:focus {
        outline: none;
      }
    }
  }
}
</style>
