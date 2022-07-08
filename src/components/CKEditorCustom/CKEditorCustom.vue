<template>
  <div class="ckeditor-classic-container">
    <div class="ckeditor-document-toolbar" ref="ckeditorToolbarDOM"></div>
    <CKEditor :editor="CKEditorCustom" :config="editorConfig" v-model="editorData" @ready="handleCKEditorReady" />
  </div>
</template>

<script setup>
import { ref } from "vue";
// 1. 导入 CKEditor 提供的 Vue 3 组件
import CKEditor5Vue from "@ckeditor/ckeditor5-vue";
// 2. 导入 CKEditor online 下载的自定义版本
const CKEditorCustom = require("@/assets/js/ckeditor5custom/ckeditor");
// 3 非全局 app.use(CKEditor) 使用的话，需要将组件单独声明
const CKEditor = CKEditor5Vue.component;
// 4. 声明编辑器的数据绑定
const editorData = "<p>Hello World!</p>";
// 5. 声明编辑器配置
const editorConfig = {};
// 6. 解耦编辑器需要手动导入工具栏
// 6.1 先声明工具栏容器 DOM
const ckeditorToolbarDOM = ref(null);
// 6.2 监听编辑器 ready 事件，从 editor 实例中获取工具栏
const handleCKEditorReady = (editor) => {
  ckeditorToolbarDOM.value.appendChild(editor.ui.view.toolbar.element);
};
</script>
