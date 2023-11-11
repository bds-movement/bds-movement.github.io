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
  <main class="text-center my-10 md:mt-16 px-3 md:px-20">
    <img src="https://bdsmovement.net/sites/all/themes/bds/logo.png" alt="BDS Movement" class="w-64 mx-auto">
    <p class="mt-10 text-left text-xs mx-0 sm:mx-10 md:mx-24 md:text-lg">Gerakan Boikot, Divestasi, Sanksi (BDS) berupaya mengakhiri dukungan internasional terhadap penindasan Israel terhadap warga Palestina dan menekan Israel agar mematuhi hukum internasional.</p>
    <h4 class="text-red-600 font-mono font-semibold mt-5">Update Terakhir : 2023-11-11 20:45 WIB</h4>
    <input type="text" v-model="search_keyword" class="w-full shadow-sm border-[1px] text-center px-5 py-3 my-4 md:my-10 rounded-full" placeholder="Cari Nama Produk/Perusahaan/Kata Kunci Dalam Daftar Boikot">
    <div class="rounded-3xl border-[1px] py-4 px-10 shadow overflow-hidden">
      <div class="overflow-x-scroll">
        <h3 class="sm:hidden">Scroll ke kanan untuk melihat keterangan lebih lanjut</h3>
        <table class="w-full min-w-[600px] align-middle">
          <tr v-for="product in product_shown">
            <td class="w-24">
              <!-- <div class="rounded-xl w-24 h-24 bg-center bg-contain bg-no-repeat border-[1px] mr-5" :style="`background-image: url(${product.image})`"></div>     -->
              <v-lazy-image src-placeholder="https://cdn-images-1.medium.com/max/80/1*xjGrvQSXvj72W4zD6IWzfg.jpeg" :src="product.image" class="rounded-xl max-w-[4rem] bg-center bg-contain bg-no-repeat border-[1px] mr-5" :alt="product.name"/>
            </td>
            <td class="text-left text-sm pl-5">
              <div class="font-semibold text-gray-500">Produk</div>
              <span class="text-800 font-mono text-lg">{{ product.name }}</span>
            </td>
            <td class="text-left text-sm pl-5">
              <div class="font-semibold text-gray-500">Jenis</div>
              <span class="text-800 text-base font-mono">{{ product.kind }}</span>
            </td>
            <td class="text-left text-sm pl-5">
              <div class="font-semibold text-gray-500">Alternatif</div>
              <span class="text-800 font-mono whitespace-nowrap text-gray-600">-- coming soon --</span>
            </td>
            <!-- <td class="text-right">
              <div>Perusahaan</div>
              <v-lazy-image v-if="product.company_img" src-placeholder="https://cdn-images-1.medium.com/max/80/1*xjGrvQSXvj72W4zD6IWzfg.jpeg" :src="product.company_img" class="max-h-10 max-w-sm" :alt="product.company"/>
            </td> -->
          </tr>
        </table>
      </div>
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
