<template>
    <div class="w-full flex flex-col items-center justify-center">

        <div v-if="showControls" name="controls" class=" bg-gray-200 border-gray-300 md:w-1/2 lg:w-1/3 rounded mb-2 w-10/12 relative">
            <button 
                @click="editor.chain().focus().toggleBold().run()"
                class="border h-10 w-10 border-gray-600 rounded shadow-md m-2 bg-gray-900 text-white hover:bg-blue-900 transition duration-200"
            >
               <span class="font-bold">G</span>
            </button>

            <button 
                @click="editor.chain().focus().toggleItalic().run()" 
                class="border h-10 w-10 border-gray-600 rounded shadow-md m-2 bg-gray-900 text-white hover:bg-blue-900"
                :class="{ 'is-active': editor.isActive('italic') }">
                <span class="italic">I</span> 
            </button>

            <button 
                @click="editor.chain().focus().toggleUnderline().run()" 
                class="border h-10 w-10 border-gray-600 rounded shadow-md m-2 bg-gray-900 text-white hover:bg-blue-900"
                :class="{ 'is-active': editor.isActive('underline') }">
                <span class="underline">U</span>
            </button>

            <button 
                @click="colorier()" 
                class="border h-10 w-20 border-gray-600 rounded shadow-md m-2 bg-green-600 text-white hover:bg-green-700 absolute right-2"
            >
                <span class="font-bold">Colorier</span>
            </button>
        </div>

        <editor-content 
            :editor="editor" 
            ref="editor"
            class="border border-gray-300 w-10/12 md:w-1/2 lg:w-1/3  rounded p-2"
            spellcheck="false"
        />

    </div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-3'
import StarterKit from '@tiptap/starter-kit'
import TextStyle from '@tiptap/extension-text-style'
import { Color } from '@tiptap/extension-color'
import FontFamily from '@tiptap/extension-font-family'
import Underline from '@tiptap/extension-underline'

export default {
    components: {
        EditorContent,
    },

    data() {
        return {
            editor: null,
            showControls: false,
            content: null
        }
    },

    

    methods : {
        colorier(){
            let html = this.editor.getHTML()
            // console.log(html)
            let content = html.replace(/<\/?[^>]+(>|$)/g, "");
            this.editor.commands.clearContent()

            let words = content.split(' ');
          
            words.forEach(el => {

                let span = document.createElement('span');
                span.style.fontFamily = 'BeVietnam'
                /* si le mot contient dau huyen, le mot est vert */
                if (el.indexOf('à') !== -1 || el.indexOf('ầ') !== -1 || el.indexOf('ằ') !== -1 || el.indexOf('ề') !== -1 || el.indexOf('è') !== -1 || el.indexOf('ì') !== -1 || el.indexOf('ò') !== -1 || el.indexOf('ồ') !== -1 || el.indexOf('ù') !== -1 || el.indexOf('ừ') !== -1 || el.indexOf('ờ') !== -1) {
                    span.style.color = '#4893bd';
                }

                /* si le mot contient dau sac, le mot est orange*/
                if (el.indexOf('á') !== -1 || el.indexOf('ấ') !== -1 || el.indexOf('ắ') !== -1 || el.indexOf('ế') !== -1 || el.indexOf('é') !== -1 || el.indexOf('í') !== -1 || el.indexOf('ó') !== -1 || el.indexOf('ố') !== -1 || el.indexOf('ú') !== -1 || el.indexOf('ứ') !== -1 || el.indexOf('ớ') !== -1) {
                    span.style.color = '#D4A111';
                }

                /* si le mot contient dau nang, le mot est violet*/
                if (el.indexOf('ạ') !== -1 || el.indexOf('ệ') !== -1 || el.indexOf('ẹ') !== -1 || el.indexOf('ị') !== -1 || el.indexOf('ọ') !== -1 || el.indexOf('ộ') !== -1 || el.indexOf('ụ') !== -1 || el.indexOf('ợ') !== -1 || el.indexOf('ự') !== -1 || el.indexOf('ậ') !== -1 || el.indexOf('ặ') !== -1) {
                    span.style.color = '#bd1e51';
                }

                /* si le mot contient dau hoi, le mot est bleu*/
                if (el.indexOf('ả') !== -1 || el.indexOf('ẩ') !== -1 || el.indexOf('ẻ') !== -1 || el.indexOf('ể') !== -1 || el.indexOf('ỉ') !== -1 || el.indexOf('ỏ') !== -1 || el.indexOf('ổ') !== -1 || el.indexOf('ủ') !== -1 || el.indexOf('ử') !== -1 || el.indexOf('ở') !== -1) {
                    span.style.color = 'green';
                }

                /* si le mot contient dau nga, le mot est rouge*/
                if (el.indexOf('ã') !== -1 || el.indexOf('ẵ') !== -1 || el.indexOf('ẫ') !== -1 || el.indexOf('ẽ') !== -1 || el.indexOf('ễ') !== -1 || el.indexOf('ĩ') !== -1 || el.indexOf('õ') !== -1 || el.indexOf('ỗ') !== -1 || el.indexOf('ũ') !== -1 || el.indexOf('ỡ') !== -1 || el.indexOf('ữ') !== -1) {
                    span.style.color = 'violet';
                }

                let w = document.createTextNode(el + " ");

                span.appendChild(w);
                let stringSpan = span.outerHTML
                this.editor.commands.insertContent(stringSpan)
            });

       
  
        }
    },

    mounted() {
        this.editor = new Editor({
        content: '<p><span style="color: #958DF1">Xín Chào mọi người </span></p>',
        extensions: [
            StarterKit,
            TextStyle,
            Color,
            FontFamily,
            Underline
        ],
        })

        this.editor.commands.setFontFamily('BeVietnam')
        console.log(this.editor)
        this.showControls = true
    },

    beforeUnmount() {
        this.editor.destroy()
    },
    }
</script>

<style>
.ProseMirror:focus {
    outline: none;
}

.ProseMirror h1 {

    @apply text-xl font-bold;
}

.ProseMirror h2 {

    @apply text-lg font-semibold;
}
</style>