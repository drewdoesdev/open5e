<template>
  <nav
    class="my-2 flex h-min items-center border-gray-200 py-0 align-middle dark:border-basalt sm:ml-8 sm:border sm:px-4 sm:py-2"
    aria-label="breadcrumbs"
  >
    <ol class="space-x-1 text-xs font-semibold sm:space-x-3 lg:text-sm">
      <!-- Prepend Home Link to list -->
      <li
        class="inline-flex items-center align-middle text-gray-700 after:ml-1 after:text-blood after:content-['/'] last:after:content-[''] visited:text-gray-700 hover:text-red-700 dark:text-gray-200 dark:visited:text-gray-200 dark:hover:text-red-400 sm:after:ml-4"
      >
        <!-- On narrow screens, home breadcrumb serves as site brand -->
        <nuxt-link to="/">
          <span
            class="font-serif text-lg font-bold hover:text-red-700 dark:text-white dark:hover:text-red-200 sm:hidden"
          >
            Open5e
          </span>
          <span class="hidden align-middle uppercase sm:inline-flex">
            <Icon name="heroicons:home" class="mt-0.5 align-middle sm:mr-1" />
            Home
          </span>
        </nuxt-link>
      </li>

      <!-- Render Breadcrumbs -->
      <li
        v-for="crumb in crumbs"
        :key="crumb.url"
        class="inline-flex break-before-auto items-center align-middle uppercase text-gray-700 after:ml-1 after:text-blood after:content-['/'] last:after:content-[''] visited:text-gray-700 hover:text-red-700 dark:text-gray-200 dark:visited:text-gray-200 dark:hover:text-red-400 sm:after:ml-4"
      >
        <nuxt-link :to="crumb.url" class="align-middle">
          <span>{{ crumb.title }}</span>
          <span v-if="crumb.subtitle" class="font-thin text-granite sm:ml-2">
            ({{ crumb.subtitle }})
          </span>
        </nuxt-link>
      </li>
    </ol>
  </nav>
</template>

<script setup>
import { useRoute } from 'nuxt/app';
import { computed } from 'vue';
const crumbs = computed(() => {
  let url = '';
  return useRoute()
    .path.split('/')
    .map((segment) => {
      // ignore initial & trailing slashes
      if (segment === '' || segment === '/') {
        return;
      }

      // rebuild link urls segment by segment
      url += `/${segment}`;

      // seperate segment title & query params
      const [title, queryParams] = segment.split('?');

      // extract & format the search params if on the /search route
      const searchParam =
        title === 'search' &&
        queryParams.split('text=')[1].split('+').join(' ');

      // return a
      return {
        url,
        title: title // format crumb title
          .split('-')
          .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
          .join(' '),
        subtitle: searchParam,
      };
    })
    .filter((breadcrumb) => breadcrumb);
});
</script>
