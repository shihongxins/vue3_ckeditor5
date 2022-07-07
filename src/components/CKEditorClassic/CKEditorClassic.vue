<template>
  <div class="ckeditor-classic-container">
    <CKEditor :editor="CKEditorClassic" :config="editorConfig" v-model="editorData" />
  </div>
</template>

<script setup>
  // 1. 导入 CKEditor 提供的 Vue 3 组件
  import CKEditor5Vue from "@ckeditor/ckeditor5-vue";
  // 2. 导入 CKEditor 提供的 经典版本
  import CKEditorClassic from "@ckeditor/ckeditor5-build-classic";
  // 6.1 导入语言
  import "@ckeditor/ckeditor5-build-classic/build/translations/zh-cn";
  // 7.1 导入粘贴图片插件（失败：预定义版本无法手动导入插件）
  // import Base64UploadAdapter from "@ckeditor/ckeditor5-upload/src/adapters/base64uploadadapter";
  // 3 非全局 app.use(CKEditor) 使用的话，需要将组件单独声明
  const CKEditor = CKEditor5Vue.component;
  // 4. 声明编辑器的数据绑定
  const editorData = "<p>Hello World!</p>";
  // 5. 声明编辑器配置
  const editorConfig = {
    // 6.2 配置语言
    language: "zh-cn",
    // 7.2 配置粘贴图片插件（失败）
    // plugins: [ Base64UploadAdapter ]
  };
</script>
