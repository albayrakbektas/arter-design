<template>
  <LayoutPage>
    <FirstSection />
    <ExperienceCards />
    <MyServices />
    <PricePlans />
    <RecommendationCards :header="header" />
  </LayoutPage>
</template>

<script>
import FirstSection from "@/components/Pages/Home/top/FirstSection";
import ExperienceCards from "@/components/Pages/Home/experience/ExperienceCards";
import MyServices from "@/components/Pages/Home/services/MyServices";
import PricePlans from "@/components/Pages/Home/prices/PricePlans";
import RecommendationCards from "@/components/Pages/Home/recommendation/RecommendationCards";
import LayoutPage from "@/components/Layouts/LayoutPage";
export default {
  name: "HomeView",
  components: {
    LayoutPage,
    RecommendationCards,
    PricePlans,
    MyServices,
    ExperienceCards,
    FirstSection,
  },
  data() {
    return {
      header: {
        title: "Recommendations",
      },
    };
  },
};
</script>

<style scoped lang="scss">
.bg-img {
  left: 0;
  right: 0;
  background-image: url("../assets/img/home/bg.jpeg");
}
.scroll-rows {
  height: 100%;
  overflow-y: scroll;
  overflow-x: hidden;
  -webkit-overflow-scrolling: touch;
  transform: translate3d(0px, 0px, 0px);
  scroll-behavior: smooth;
  scroll-snap-type: y mandatory;
  scroll-snap-align: start;
}
::-webkit-scrollbar {
  display: none;
}
.home {
  transform: translateX(0px);
  transition: 600ms ease-in-out;
}
@media (min-width: 900px) {
  .active-menu-right {
    transform: translateX(-150px);
  }
}
</style>
