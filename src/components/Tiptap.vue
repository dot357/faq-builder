<template>
  <section>
      <div class="menu">
    <button
      @click="editor.chain().focus().toggleBold().run()"
      
    >
      bold
    </button>
    <button
      @click="editor.chain().focus().toggleItalic().run()"
      
    >
      italic
    </button>
    <button
      @click="editor.chain().focus().toggleStrike().run()"
      
    >
      strike
    </button>
  </div>

  <editor-content
    :editor="editor"
    @input="change"
    @mouseleave="change"
    @keydown.ctrl="change"
    @keydown.shift="change"
    @keydown.meta="change"
  />
  </section>
</template>

<script>
import { Editor, EditorContent } from "@tiptap/vue-3";
import StarterKit from "@tiptap/starter-kit";

export default {
  components: {
    EditorContent,
  },

  data() {
    return {
      editor: null,
    };
  },
  methods: {
    change() {
      this.$emit("tiptap-change", this.editor.getHTML());
    },
  },

  mounted() {
    this.editor = new Editor({
      content: "<p>This is an html editor. 🎉</p>",
      extensions: [
        StarterKit,
        
      ],
    });
  },

  beforeUnmount() {
    this.editor.destroy();
  },
};
</script>


<style scoped>

.menu{
    display: flex;
    flex-direction: row;
    align-items: center;
    flex-wrap: wrap;
    gap: 5px;
    padding: 0.5rem 0;
    
}
button{
    border: none;
    outline: none;
    background: white;
    padding: 6px 12px;
    border: 1px solid black;
    border-radius: 4px;
    text-transform: capitalize;
    cursor: pointer;
}
button:hover{
    transition: all 0.3s ease-in;
    background: black;
    color: white;
}
</style>

