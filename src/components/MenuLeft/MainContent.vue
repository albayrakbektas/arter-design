<template>
  <div class="menu-left-main-content">
    <TableInfo :table-list="tableList" />
    <div class="divider" />
    <div class="language-circle-container">
      <LanguageCircle
        v-for="(item, index) of languageCircleList"
        :circle="item"
        :key="index"
      />
    </div>
    <div class="divider" />
    <SkillProgressContainer />
    <div class="divider" />
    <KnowledgeList :knowledge-list="knowledgeList" />
    <div class="divider" />
    <div class="pdf-container">
      <router-link class="download-pdf" to="#.">
        DOWNLOAD CV
        <i class="fa-solid fa-download"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
import TableInfo from "@/components/MenuLeft/TableInfo";
import LanguageCircle from "@/components/MenuLeft/LanguageCircle";
import SkillProgressContainer from "@/components/MenuLeft/SkillProgressContainer";
import KnowledgeList from "@/components/MenuLeft/KnowledgeList";
export default {
  name: "MainContent",
  components: {
    KnowledgeList,
    SkillProgressContainer,
    LanguageCircle,
    TableInfo,
  },
  data() {
    return {
      index: 0,
      tableList: [
        {
          key: "Residence:",
          value: "Turkey",
        },
        {
          key: "City:",
          value: "Izmir",
        },
        {
          key: "Age:",
          value: "26",
        },
      ],
      languageCircleList: [
        {
          language: "french",
          percent: "100",
        },
        {
          language: "english",
          percent: "80",
        },
        {
          language: "spanish",
          percent: "70",
        },
      ],
      knowledgeList: [
        { icon: "fa-solid fa-check", content: "Bootstrap, Materialize" },
        { icon: "fa-solid fa-check", content: "Stylus, Sass, Less" },
        { icon: "fa-solid fa-check", content: "Gulp, Webpack, Grunt" },
        { icon: "fa-solid fa-check", content: "GIT knowledge" },
      ],
    };
  },
  mounted() {
    document.querySelector(".menu-left-main-content").style.transform =
      "translate3d(0px, 0px, 0px)";
    this.percentAnimate(0, Number(this.languageCircleList[0].percent), 2000, 1);
    this.percentAnimate(0, Number(this.languageCircleList[1].percent), 2000, 2);
    this.percentAnimate(0, Number(this.languageCircleList[2].percent), 2000, 3);
  },
  methods: {
    percentAnimate(start, end, duration, n) {
      if (start === end) {
        return;
      }
      let range = end - start;
      let current = start;
      let increment = end > start ? 1 : -1;
      let stepTime = Math.abs(Math.floor(duration / range));
      let timer = setInterval(() => {
        current += increment;
        const spinner = document.querySelector(
          `div.language-circle-container > div:nth-child(${n}) > div > div.progress-spinner`
        );
        spinner.style.background =
          "conic-gradient(rgb(255, 193, 7) " +
          0 +
          "deg,rgb(255, 193, 7)" +
          current * 3.6 +
          "deg,rgb(0, 0, 0) " +
          current * 3.6 +
          "deg,rgb(0, 0, 0) 360deg)";
        if (current === end) {
          clearInterval(timer);
        }
      }, stepTime);
    },
  },
};
</script>

<style lang="scss" scoped>
.menu-left-main-content {
  padding-top: 260px;
  height: calc(100vh + 50px);
  width: 75%;
  margin: 0 auto;
}
.divider {
  background-color: #646466;
  height: 1px;
  width: 100%;
  opacity: 0.5;
}
.language-circle-container {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(3, auto);
  align-items: center;
  justify-content: space-between;
  padding: 30px 0 15px;
}
.download-pdf {
  height: calc(100% - 50px);
  display: grid;
  align-items: center;
  grid-template-columns: repeat(2, auto);
  gap: 5px;
  justify-content: start;
  padding: 15px 0;
  font-size: 10px;
  color: #8c8c8e;
  line-height: 1.5;
  transition: 0.4s ease-in-out;
  font-weight: 600;
}
.pdf-container {
  &:hover {
    .download-pdf {
      color: #fafafc;
    }
  }
}
@media (min-width: 900px) {
  .menu-left-main-content {
    //height: calc(100vh - 30px);
    height: 100%;
  }
}
</style>
