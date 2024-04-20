<template>
  <nav class="breadcrumb">
    <ol class="flex space-x-2 mt-5 mb-5">
      <li v-for="(item, index) in items" :key="index">
        <template v-if="item.link">
          <Link :href="item.link" class="text-blue-500 hover:underline">{{ item.text }}</Link>
        </template>
        <template v-else>
          <span class="text-gray-500">{{ item.text }}</span>
        </template>
        <span v-if="index < items.length - 1" class="text-gray-500"> &nbsp;/</span>
      </li>
    </ol>
  </nav>
</template>

<script>
import { Link } from "@inertiajs/vue3";

export default {
  components: {
    Link,
  },
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
};
</script>
