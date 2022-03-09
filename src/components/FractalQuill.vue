<template>
  <div class="container">
    <button id="insert-table" class="btn" @click="table.insertTable(3, 3)">
      <i class="fas fa-table"></i>
    </button>
    <div class="editor" ref="editor"></div>
  </div>
</template>

<script>
import Quill from "quill";
import quillBetterTable from "quill-better-table";

const AlignStyle = Quill.import("attributors/style/align");
const fontSizeArr = [
  "12px",
  "14px",
  "16px",
  "18px",
  "20px",
  "22px",
  "24px",
  "26px",
  "28px",
  "30px",
  "32px",
];
const Size = Quill.import("attributors/style/size");
Size.whitelist = fontSizeArr;
Quill.register(AlignStyle, true);
Quill.register(Size, true);
// Quill.register(TableFormat, true);
Quill.register(
  {
    "modules/better-table": quillBetterTable,
  },
  true
);
export default {
  name: "FractalQuill",
  props: {
    value: {
      type: String,
      default: "",
    },
    content: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      quill: null,
      table: null,
      thecontent: "",
    };
  },
  mounted() {
    this.initQuill();
  },
  methods: {
    initQuill() {
      this.quill = new Quill(this.$refs.editor, {
        theme: "snow",
        modules: {
          table: false,
          "better-table": {
            operationMenu: {
              items: {},
            },
          },
          keyboard: {
            bindings: quillBetterTable.keyboardBindings,
          },
          toolbar: [
            [{ header: [2, false] }],
            [{ list: "ordered" }, { list: "bullet" }],
            ["bold", "italic", "underline"],
            ["image"],
            [{ color: [] }, { background: [] }],
            [{ align: ["center", "right", "justify", ""] }],
            [{ size: fontSizeArr }],
          ],
        },
      });
      this.table = this.quill.getModule("better-table");
      if (this.value || this.content) {
        this.quill.setText(this.value || this.content);
      }
      this.quill.on("text-change", () => {
        let html = this.$refs.editor.children[0].innerHTML;
        const quill = this.quill;
        const text = this.quill.getText();
        if (html === "<p><br></p>") html = "";
        this.thecontent = html;
        this.$emit("input", this.thecontent);
        this.$emit("change", { html, text, quill });
      });
    },
  },
};
</script>

<style lang="scss">
.ql-snow .ql-picker.ql-size .ql-picker-label[data-value]::before,
.ql-snow .ql-picker.ql-size .ql-picker-item[data-value]::before {
  content: attr(data-value) !important;
}
.ql-editor {
  line-height: inherit;
}
.ql-container {
  font-size: inherit !important;
}
.quill-better-table-wrapper {
  display: flex;
  justify-content: center;
}
.qlbt-col-tool {
  display: flex;
  justify-content: center;
}

.container {
  position: relative;
  height: 400px;
  .btn {
    position: absolute;
    right: 10px;
    top: 10px;
  }
}
</style>
