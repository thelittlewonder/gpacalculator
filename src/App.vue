<template>
  <div id="app">
    <img src="../static/background.svg">
    <div class="content">
      <h1>GPA Calculator</h1>
      <h2>Calculate your Semester GPA, Cummulative GPA and check how much you need next semester to cross that legendary GPA Mark</h2>
      <div class="main">
      <div class="selection">
          <button :class="[sgActive ? 'active' : '']" @click="sgActive=true">Semester GPA</button>
          <button :class="[sgActive ? '' : 'active']" @click="sgActive=false">Cumulative GPA</button>
      </div>
      <div class="sgbox" v-if="sgActive">
        <SGPA/>
      </div>
      <div class="cgbox" v-if="!sgActive">
        <CGPA/>
      </div>
    </div>
    </div>
    <footer><a href="https://www.github.com/littlewonder">Hacked by a lowly 8 CG human 🙈</a></footer>
  </div>
</template>

<script>
import SGPA from "./components/SGPA";
import CGPA from "./components/CGPA";
import "reset-css";

export default {
  name: "App",
  components: {
    SGPA,
    CGPA
  },
  data() {
    return {
      sgActive: true
    };
  }
};
</script>

<style lang="scss">
#app {
  img {
    position: fixed;
    top: 0;
    left: 0;
    min-width: 100%;
    max-width: 100%;
    z-index: -999;
  }
  html {
    font-size: 100%;
  }
  @media screen and (max-width: 767px) {
    .content {
      padding: 5%;
    }
    min-width: 500px;
  }

  @media screen and (min-width: 768px) {
    .content {
      padding: 8% 25% 0 25%;
    }
    .content {
      min-width: 600px;
    }
  }
  .content {
    display: flex;
    flex-direction: column;
    margin-bottom: 100px;
    h1 {
      font-family: "Nunito Sans";
      font-size: 2rem;
      color: #ffffff;
      font-weight: 700;
      text-align: center;
    }
    .main {
      background: #ffffff;
      box-shadow: 0 5px 20px 0 rgba(179, 179, 179, 0.5);
      border-radius: 0.5rem;
    }
    h2 {
      font-family: "Nunito Sans";
      color: #ffffff;
      text-align: center;
      text-align: center;
      font-size: 1.125rem;
      margin: 0.75rem 0 3rem 0;
      line-height: 1.5rem;
    }
    .selection {
      position: relative;
      top: 82px;
      width: 100%;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      button {
        cursor: pointer;
        width: 50%;
        padding: 0.75rem;
        background: #fff;
        border: none;
        font-family: "Nunito Sans";
        font-size: 0.875rem;
        color: #a6a6a6;
        text-transform: uppercase;
        border-bottom: 1px solid transparent;
        letter-spacing: 0.5px;
        transition-timing-function: ease-in-out;
        transition-duration: 0.25s;
        transition-property: all;
        &:focus {
          outline: none;
        }
      }
      .active {
        border-bottom: 1px solid #2d85fd;
        font-family: "Nunito Sans";
        font-weight: 700;
        font-size: 0.875rem;
        color: #2d85fd;
        letter-spacing: 0.5px;
      }
    }
  }
  footer {
    padding: 1rem 0;
    text-align: center;
    background: #f9f9f9;
    position: fixed;
    width: 100%;
    z-index: -999;
    bottom: 0;
    a {
      text-decoration: none;
      font-family: "Nunito Sans";
      font-size: 12px;
      color: #a19d9d !important;
    }
  }
}
</style>
