<template>
  <div class="page-template-default">
    <el-hero :title="post.title" :style="{ textAlign: settings.headerAlignment || 'left' }">
      <template v-slot:hero-content>
        <div v-if="post.subtitle" v-formatted-text="post.subtitle" class="content" />
      </template>
    </el-hero>
    <div class="mast content-inner">
      <div v-formatted-text="renderMarkdown(post.content)" class="content entry-content" />
      <factor-post-edit :post-id="post._id" />
    </div>
  </div>
</template>

<script lang="ts">
import { factorPostEdit } from "@factor/post"
import { renderMarkdown } from "@factor/api/markdown"
import { setting, stored, titleTag, descriptionTag, shareImage } from "@factor/api"

export default {
  components: { factorPostEdit, "el-hero": () => import("./el/hero.vue") },
  data() {
    return {}
  },
  metaInfo() {
    return {
      title: titleTag(this.post._id),
      description: descriptionTag(this.post._id),
      image: shareImage(this.post._id),
    }
  },
  computed: {
    post(this: any) {
      return stored("post") || {}
    },
    settings(this: any) {
      return this.post.settings || {}
    },
  },
  methods: { setting, renderMarkdown },
  templateSettings() {
    return [
      {
        input: "select",
        label: "Header Alignment",
        description: "Alignment of the page header",
        _id: "headerAlignment",
        list: ["left", "center", "right"],
        _default: "left",
      },
    ]
  },
}
</script>

<style lang="less">
.content-inner {
  margin: 5em auto;
  max-width: 600px;
  min-height: 60vh;
  .title {
    font-size: 2.5em;
  }
  .subtitle {
    font-size: 1.4em;
  }
  .content {
    font-size: 1.2em;
    font-weight: 500;
    line-height: 1.5;

    h1,
    h2,
    h3 {
      font-weight: 600;
    }
    h1 {
      font-size: 2.5em;
    }
    h2 {
      font-size: 1.6em;
    }
    h3 {
      font-size: 1.2em;
    }
    p {
      margin: 1.5em 0;
    }

    blockquote {
      padding: 1em;
      margin: 1em 0;
      border-left: 5px solid var(--color-primary);
      :first-child {
        margin-top: 0;
      }
      :last-child {
        margin-bottom: 0;
      }
    }
  }
}
</style>
