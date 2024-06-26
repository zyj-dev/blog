<script setup lang="ts">
import { computed } from "vue";
import { Link } from "./types";
import JLogo from "./JLogo.vue";

interface Props {
  data: Link[] | Link | null | undefined;
  logo?: boolean;
  imgBorderRadius?: string;
  container?: boolean;
  deep?: boolean;
  target?: "_blank" | "_self" | "_parent" | "_top";
}

const props = defineProps<Props>();

const data = computed(() => {
  if (!props.data) return null;
  return Array.isArray(props.data) ? props.data : [props.data];
});
</script>

<template>
  <div :class="props.container ? 'links__container' : 'link-bar'">
    <template v-for="(item, index) in data">
      <a
        v-if="!deep || !item.items || !item.items.length"
        class="link"
        :key="index"
        :href="item.link"
        :title="item.text"
        :target="props.target || '_blank'"
      >
        <JLogo
          :src="item.icon"
          :logo="props.logo"
          width="1.2rem"
          height="1.2rem"
          :borderRadius="props.imgBorderRadius"
          alt=""
          class="logo"
        />
        <span class="text">{{ item.text }}</span>
      </a>

      <JLinkBar
        v-else
        :data="item.items"
        :logo="props.logo"
        :container="props.container"
        :deep="props.deep"
        :target="props.target"
        :borderRadius="props.imgBorderRadius"
      ></JLinkBar>
    </template>
  </div>
</template>

<style scoped>
/* container */
.links__container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, max-content));
  grid-auto-flow: dense;
  column-gap: 1rem;
  row-gap: 0.75rem;
  align-items: start;
}

.link-bar {
  display: inline-block;
}

/* link */
.link {
  display: flex;
  flex-wrap: nowrap;
  align-items: center;
  gap: 0.75rem;
  color: inherit;
  text-decoration: none;
  background-color: var(--vp-c-bg-soft);
  border-radius: 0.5rem;
  transition: background-color 0.2s;
  padding: 0.5rem 0.75rem;
}

.link:hover {
  background-color: var(--vp-button-alt-bg);
  color: inherit;
}

.logo {
  flex-shrink: 0;
}

/* text */
.text {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>
