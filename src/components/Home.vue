<template>
  <div class="container flex flex-col m-auto lg:px-52 ">
    <div class="p-2 mt-8"
      ><h1 class="font-serif text-4xl py-4 md:text-5xl lg:text-8xl"
        >The creative crew</h1
      >
      <section>
        <h2 class="text-lg uppercase font-bold py-2 md:text-xl lg:text-3xl"
          >who we are</h2
        >
        <p class="text-sm md:text-lg md:tracking-tight lg:text-2xl"
          >Lorem, ipsum dolor sit amet consectetur adipisicing elit. Eaque amet
          nesciunt labore voluptate mollitia natus unde, voluptas nulla dicta
          accusantium!</p
        >
      </section>
    </div>
    <div class="p-2 mt-8 flex flex-wrap">
      <div
        v-for="(item, index) in data"
        :key="item"
        class="w-1/2 md:w-1/3"
        :class="[
          index % 2 && windowsSize < 768 != 0
            ? 'mt-24'
            : (index == 1 || index == 4) && windowsSize >= 768
            ? 'mt-44'
            : '',
        ]"
      >
        <div class="relative" style="padding-bottom: 150%;">
          <div class="uppercase md:text-xl putElement">{{
            iterator.next().value
          }}</div>
          <img
            :src="item.picture.large"
            alt="avatarPicture"
            class="absolute w-6/7 h-full top-0 left-0 object-center"
          />
        </div>
        <span class="font-bold text-base mt-3 whitespace-nowrap lg:text-xl">{{
          item.name.first + " " + item.name.last
        }}</span>
      </div>
    </div>
  </div>
</template>
<script>
import { computed, onMounted, ref } from "vue";
export default {
  setup() {
    var data = ref([]);
    var role = [
      "Product owner",
      "Art Director",
      "Tech lead",
      "Ux Designer",
      "Developer",
      "Developer",
    ];
    var windowsSize = computed(function getWindowsSize() {
      return window.innerWidth;
    });
    var iterator = ref(undefined);
    function* getRandomTitle() {
      for (const item of role) {
        yield item;
      }
    }
    onMounted(async function getData() {
      var response = await fetch(
        "https://randomuser.me/api/?results=6&inc=name,gender,nat,picture&noinfo&nat=US"
      );
      if (response.ok) {
        var json = await response.json();
        data.value = json.results;
        iterator.value = getRandomTitle();
      } else {
        alert("Something went wrong...");
      }
    });

    return { data, iterator, windowsSize };
  },
};
</script>
<style scoped>
.putElement {
  /* transform-origin: bottom right;
  transform: rotate(-90deg) translate(20px, -5px); */
  writing-mode: vertical-rl;
  text-orientation: mixed;
  position: absolute;
  right: 1%;
  top: 0%;
}
@media screen and (min-width: 768px) {
  .putElement {
    right: 3%;
  }
}
</style>
