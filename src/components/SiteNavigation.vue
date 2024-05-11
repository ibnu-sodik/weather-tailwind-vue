<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav
      class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6"
    >
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3 flex-1">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">GoBlog - Cuaca Lokal</p>
        </div>
      </RouterLink>

      <div class="flex gap-3 flex-1 justify-end">
        <i
          @click="toggleModal"
          class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
        ></i>
        <i
          class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"
        ></i>
      </div>

      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <h1 class="text-black">Hello from modal</h1>
        <p class="text-black">
          Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sint
          perspiciatis beatae magnam laboriosam qui ipsa illo quae magni
          delectus voluptatibus sed deserunt dolores molestiae facere modi
          quaerat consequatur minima similique dignissimos consequuntur
          exercitationem, sit doloribus! Maiores vel, quo quas repudiandae
          assumenda saepe consequatur quam at corrupti optio tempore soluta
          sequi doloremque facilis ex porro excepturi commodi exercitationem
          quia in officiis. Eaque fugiat alias fuga totam et dolore tempore
          fugit ut ducimus, molestiae neque accusamus dolorem voluptatibus atque
          deleniti, vel obcaecati maxime voluptatum id dignissimos error sequi!
          Dolor quam perferendis culpa? A, iusto officiis. Exercitationem et
          laudantium blanditiis sunt commodi tenetur!
        </p>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue";
import BaseModal from "./BaseModal.vue";
import { RouterLink } from "vue-router";

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
