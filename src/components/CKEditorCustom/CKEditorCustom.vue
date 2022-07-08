<template>
  <div class="ckeditor-classic-container">
    <div class="ckeditor-document-toolbar" ref="ckeditorToolbarDOM"></div>
    <div class="ckeditor-document-wrapper" ref="ckeditorWrapperDOM"></div>
  </div>
  <div>
    {{ editorData }}
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
// 1. 导入 CKEditor online 下载的自定义版本
// 1.1 does not provide an export named 'default'
// import CKEditorCustom from "@/assets/js/ckeditor5custom/ckeditor.js";
// 1.2 does not provide an export named 'DecoupledDocumentEditor'
// import { DecoupledDocumentEditor } from "@/assets/js/ckeditor5custom/ckeditor.js";
// 1.3 Uncaught ReferenceError: require is not defined
// const CKEditorCustom = require("@/assets/js/ckeditor5custom/ckeditor");
// 1.4 当作静态文件导入，通过 Window 对象上的编辑器版本用原生 JS 的方式创建编辑器
import "../../assets/js/ckeditor5custom/ckeditor.js";
// 2. 声明编辑器的数据绑定
const editorData = ref("<p>Hello World!</p>");
// 3. 声明编辑器配置
const editorConfig = {
  initialData: editorData.value
};
// 4. 原生 JS 创建编辑器需要指定编辑器容器节点
// 4.1 先声明编辑器容器 DOM
const ckeditorWrapperDOM = ref(null);
// 4.2 再声明编辑器的初始化 JS 方法
const handleCKEditorCustomInit = async () => {
  // 这里 DecoupledDocumentEditor 是在线构建器，构建时选择的对应版本
  if (DecoupledDocumentEditor) {
    try {
      // 初始化编辑器，传入容器节点和配置
      const editor = await DecoupledDocumentEditor.create(
        ckeditorWrapperDOM.value,
        editorConfig
      );
      if (editor && editor.state === "ready") {
        window.editor = editor;
        // 5.3 解耦编辑器需要手动导入工具栏
        handleCKEditorReady(editor);
        // 监听文档改变事件
        editor.model.document.on("change:data", () => {
          // 获取文档数据绑定到变量中
          editorData.value = editor.getData();
        });
      }
    } catch (error) {
      console.error(error);
    }
  } else {
    console.warn("自定义 CKEditor 对象不存在。请检查是否导入 import ； 编辑器版本；初始化时机 onMounted ；");
  }
}
// 4.3 最后在 onMounted 钩子中调用
onMounted(() => {
  handleCKEditorCustomInit();
})
// 5. 解耦编辑器需要手动导入工具栏
// 5.1 先声明工具栏容器 DOM
const ckeditorToolbarDOM = ref(null);
// 5.2 监听编辑器 ready 事件，从 editor 实例中获取工具栏
const handleCKEditorReady = (editor) => {
  ckeditorToolbarDOM.value.appendChild(editor.ui.view.toolbar.element);
};
</script>
