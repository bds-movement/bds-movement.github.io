<script setup>
import { computed, ref } from 'vue';
import products from './products';
import VLazyImage from "v-lazy-image";

const search_keyword = ref('')
const product_shown = computed(() => {
  let prods = []
  if (search_keyword.value?.trim()) {
    const search_query = search_keyword.value.trim().toLowerCase()
    for (let product of products) {
      if (product.name.toLowerCase().includes(search_query) || product.company?.toLowerCase()?.includes(search_query) || product.kind?.toLowerCase()?.includes(search_query)) {
        prods.push(product)
        continue
      }

      if (product.keywords?.length) {
        for (let keyword of product.keywords) {
          if (keyword.toLowerCase()?.includes(search_query)) {
            prods.push(product)
            break
          }
        }
      }
    }
  } else {
    prods = products
  }
  return prods
})
</script>

<template>
  <main class="text-center my-10 md:mt-32 px-14 md:px-20">
    <h1 class="text-6xl font-mono my-10">BDS Movement</h1>
    <input type="text" v-model="search_keyword" class="w-full shadow-sm border-[1px] text-center py-3 my-10 rounded-full" placeholder="Cari Nama Produk">
    <div class="rounded-3xl border-[1px] py-4 px-10 shadow">
      <table class="w-full align-middle">
        <tr v-for="product in product_shown">
          <td>
            <!-- <div class="rounded-xl w-24 h-24 bg-center bg-contain bg-no-repeat border-[1px] mr-5" :style="`background-image: url(${product.image})`"></div>     -->
            <v-lazy-image src-placeholder="https://cdn-images-1.medium.com/max/80/1*xjGrvQSXvj72W4zD6IWzfg.jpeg" :src="product.image" class="rounded-xl w-24 h-24 bg-center bg-contain bg-no-repeat border-[1px] mr-5" :alt="product.name"/>
          </td>
          <td class="text-left text-xl font-semibold text-gray-700">
            {{ product.name }}
          </td>
          <td class="text-left text-lg text-gray-600">
            {{ product.kind }}
          </td>
          <td class="text-right">
           <div class="flex items-center justify-end space-x-3">
              <v-lazy-image v-if="product.company_img" src-placeholder="https://cdn-images-1.medium.com/max/80/1*xjGrvQSXvj72W4zD6IWzfg.jpeg" :src="product.company_img" class="max-h-10 max-w-sm" :alt="product.company"/>
              <!-- <span class="text-lg text-slate-700 capitalize">{{ product.company }}</span> -->
            </div>
          </td>
        </tr>
      </table>
    </div>
  </main>
</template>

<style scoped lang="scss">
td{
  @apply py-3;
}
.v-lazy-image {
  filter: blur(10px);
  transition: filter 0.7s;
}
.v-lazy-image-loaded {
  filter: blur(0);
}
</style>
