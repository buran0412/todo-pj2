<template>
  <div id="app">
    <div class="container">
      <div class="card">
        <p>Todo List</p>
        <span></span>
        <input v-model="task" class="input-add" />
        <button class="add" v-on:click="addTodo">追加</button>
        <!-- 
        おそらくtodoをコンポーネント化することで処理を複雑にしてしまっているので
        todo-listコンポーネントのtamplateタグ内の記述はApp.vueに直接記述した方が良いと思います
        -->
        <div class="todolist">
          <div
            class="input-update"
            v-for="(item, index) in items"
            v-bind:key="item.id"
          >
            <input type="item.message" v-model="item.message" />
            <button class="edit-button" v-on:click="$emit('edit', id, index)">
              更新
            </button>
            <button class="remove-button" v-on:click="$emit('remove', id, index)">
              削除
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoList from "../src/components/TodoList.vue";
import axios from "axios";
export default {
  name: "app",
  components: {
    TodoList,
  },
  data: function () {
    return {
      task: "",
      todos: [],
      count: 0,
      id: [],
    };
  },
  methods: {
    addTodo: function () {
      if (this.task === "") {
        alert("作業名を入力してください");
        return;
      }
      axios
        .post("http://127.0.0.1:8000/api/todos", {
          // バックエンドのlaravelのTodosControllerのstore関数で受け取るのはtextプロパティなのでtext: this.taskとなります
          text: this.task,
        })
        .then((response) =>
          this.todos.push({
            message: this.task,
            id: ++this.count,
          })
        )
        .catch((error) => console.log(error));
      // コメント
      // this.users.unshift(response.data)
      // 上記記述は何をしている処理ですか？
      // 不要な場合は削除しましょう！

      // コメント
      // 上記リクエストを送る処理が成功した時の処理（.thenの中）より先に下の処理が実行されてしまいます
      // 以下の処理は.thenの中に記述すると良いと思います
      this.task = "";
      // const push = task;
    },
    // async add() {
    //   const resData = await axios.get("http://127.0.0.1:8001/api/todos");
    //   this.todos = resData.data.data;
    // },
    // cancel() {
    //   this.text = "";
    //   this.editIndex = -1;
    // },

    editTodo: function () {
      // コメント
      // patchリクエストではなくputリクエストを送信して既存のtodoの更新を行ってください
      // また、http://127.0.0.1:8001/api/todos/1このように記述することでidが1のtodoのみ変更できるようになってしまうので
      // 他のtodoのidがを入れて更新できるようにURLを変更していただきたいです

      // コメント
      // bodyには変更するテキストの内容を入れてください
      // こちらもpostメソッド同様にlaravelのupdate関数が受け取るのは「text」プロパティになりますので
      // text: todoの変更内容
      axios
        .put("http://127.0.0.1:8000/api/todos" + id)
        .then((response) =>this.users.unshift(response.data))
        .catch((error) => console.log(error));
    },

    removeTodo: function () {
      axios
        .post("http://127.0.0.1:8000/api/todos" + id,{todo:"edit"})
        .then((response) => console.log(response))
        .catch((error) => console.log(error));

      // コメント
      // indexの値が取れていないので削除する対象のidをもとに配列の何番目かを特定してindexの値を入れましょう
      // indexOfを使用すると良いと思います
      this.todos.splice(indexof, id);
    },
    // async edit(id, task) {
    //   const sendData = {
    //     task: task,
    //   };
    //   await axios.put("http://127.0.0.1:8001/api/todos" + id, sendData);
    // },
    // async insert() {
    //   const sendData = {
    //     todos: this.todos,
    //     task: this.task,
    //   };
    //   console.log(sendData);
    //   await axios.post("http://127.0.0.1:8001/api/todos", sendData);
    // },
    // async delete(id) {
    //   await axios.delete("http://127.0.0.1:8001/api/todos" + id);
    // },
  },
  created() {
    axios
      .get("http://127.0.0.1:8000/api/todos")
      .then((response) => (this.users = response.data))
      .catch((error) => console.log(error));
    // コメント
    // todoを描画しているのはdataプロパティにあるtodoなので
    this.todo = response.data;
  },
};
</script>


<style>
.container {
  background-color: #2d197c;
  height: 100vh;
  width: 100vw;
  position: relative;
}
.card {
  background-color: #fff;
  width: 50vw;
  padding: 30px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 10px;
  font-weight: bold;
  font-size: 24px;
}
.input-add {
  width: 80%;
  padding: 5px;
  border-radius: 5px;
  border: 2px solid #ccc;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  font-size: 14px;
  outline: none;
  vertical-align: middle;
}
.add {
  text-align: left;
  border: 2px solid #dc70fa;
  font-size: 12px;
  color: #dc70fa;
  background-color: #fff;
  font-weight: bold;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.4s;
  outline: none;
}
/*!
 * ress.css • v3.0.1
 * MIT License
 * github.com/filipelinhares/ress
 */
/* # =================================================================
   # Global selectors
   # ================================================================= */
html {
  box-sizing: border-box;
  -webkit-text-size-adjust: 100%; /* Prevent adjustments of font size after orientation changes in iOS */
  word-break: normal;
  -moz-tab-size: 4;
  tab-size: 4;
}
*,
::before,
::after {
  background-repeat: no-repeat; /* Set `background-repeat: no-repeat` to all elements and pseudo elements */
  box-sizing: inherit;
}
::before,
::after {
  text-decoration: inherit; /* Inherit text-decoration and vertical align to ::before and ::after pseudo elements */
  vertical-align: inherit;
}
* {
  padding: 0; /* Reset `padding` and `margin` of all elements */
  margin: 0;
}
/* # =================================================================
   # General elements
   # ================================================================= */
hr {
  overflow: visible; /* Show the overflow in Edge and IE */
  height: 0; /* Add the correct box sizing in Firefox */
}
details,
main {
  display: block; /* Render the `main` element consistently in IE. */
}
summary {
  display: list-item; /* Add the correct display in all browsers */
}
small {
  font-size: 80%; /* Set font-size to 80% in `small` elements */
}
[hidden] {
  display: none; /* Add the correct display in IE */
}
abbr[title] {
  border-bottom: none; /* Remove the bottom border in Chrome 57 */
  /* Add the correct text decoration in Chrome, Edge, IE, Opera, and Safari */
  text-decoration: underline;
  text-decoration: underline dotted;
}
a {
  background-color: transparent; /* Remove the gray background on active links in IE 10 */
}
a:active,
a:hover {
  outline-width: 0; /* Remove the outline when hovering in all browsers */
}
code,
kbd,
pre,
samp {
  font-family: monospace, monospace; /* Specify the font family of code elements */
}
pre {
  font-size: 1em; /* Correct the odd `em` font sizing in all browsers */
}
b,
strong {
  font-weight: bolder; /* Add the correct font weight in Chrome, Edge, and Safari */
}
/* https://gist.github.com/unruthless/413930 */
sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}
sub {
  bottom: -0.25em;
}
sup {
  top: -0.5em;
}
/* # =================================================================
   # Forms
   # ================================================================= */
input {
  border-radius: 0;
}
/* Replace pointer cursor in disabled elements */
[disabled] {
  cursor: default;
}
[type="number"]::-webkit-inner-spin-button,
[type="number"]::-webkit-outer-spin-button {
  height: auto; /* Correct the cursor style of increment and decrement buttons in Chrome */
}
[type="search"] {
  -webkit-appearance: textfield; /* Correct the odd appearance in Chrome and Safari */
  outline-offset: -2px; /* Correct the outline style in Safari */
}
[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none; /* Remove the inner padding in Chrome and Safari on macOS */
}
textarea {
  overflow: auto; /* Internet Explorer 11+ */
  resize: vertical; /* Specify textarea resizability */
}
button,
input,
optgroup,
select,
textarea {
  font: inherit; /* Specify font inheritance of form elements */
}
optgroup {
  font-weight: bold; /* Restore the font weight unset by the previous rule */
}
button {
  overflow: visible; /* Address `overflow` set to `hidden` in IE 8/9/10/11 */
}
button,
select {
  text-transform: none; /* Firefox 40+, Internet Explorer 11- */
}
/* Apply cursor pointer to button elements */
button,
[type="button"],
[type="reset"],
[type="submit"],
[role="button"] {
  cursor: pointer;
  color: inherit;
}
/* Remove inner padding and border in Firefox 4+ */
button::-moz-focus-inner,
[type="button"]::-moz-focus-inner,
[type="reset"]::-moz-focus-inner,
[type="submit"]::-moz-focus-inner {
  border-style: none;
  padding: 0;
}
/* Replace focus style removed in the border reset above */
button:-moz-focusring,
[type="button"]::-moz-focus-inner,
[type="reset"]::-moz-focus-inner,
[type="submit"]::-moz-focus-inner {
  outline: 1px dotted ButtonText;
}
button,
html [type="button"], /* Prevent a WebKit bug where (2) destroys native `audio` and `video`controls in Android 4 */
[type="reset"],
[type="submit"] {
  -webkit-appearance: button; /* Correct the inability to style clickable types in iOS */
}
/* Remove the default button styling in all browsers */
button,
input,
select,
textarea {
  background-color: transparent;
  border-style: none;
}
/* Style select like a standard input */
select {
  -moz-appearance: none; /* Firefox 36+ */
  -webkit-appearance: none; /* Chrome 41+ */
}
select::-ms-expand {
  display: none; /* Internet Explorer 11+ */
}
select::-ms-value {
  color: currentColor; /* Internet Explorer 11+ */
}
legend {
  border: 0; /* Correct `color` not being inherited in IE 8/9/10/11 */
  color: inherit; /* Correct the color inheritance from `fieldset` elements in IE */
  display: table; /* Correct the text wrapping in Edge and IE */
  max-width: 100%; /* Correct the text wrapping in Edge and IE */
  white-space: normal; /* Correct the text wrapping in Edge and IE */
  max-width: 100%; /* Correct the text wrapping in Edge 18- and IE */
}
::-webkit-file-upload-button {
  /* Correct the inability to style clickable types in iOS and Safari */
  -webkit-appearance: button;
  color: inherit;
  font: inherit; /* Change font properties to `inherit` in Chrome and Safari */
}
/* # =================================================================
   # Specify media element style
   # ================================================================= */
img {
  border-style: none; /* Remove border when inside `a` element in IE 8/9/10 */
}
/* Add the correct vertical alignment in Chrome, Firefox, and Opera */
progress {
  vertical-align: baseline;
}
/* # =================================================================
   # Accessibility
   # ================================================================= */
/* Hide content from screens but not screenreaders */
@media screen {
  [hidden~="screen"] {
    display: inherit;
  }
  [hidden~="screen"]:not(:active):not(:focus):not(:target) {
    position: absolute !important;
    clip: rect(0 0 0 0) !important;
  }
}
/* Specify the progress cursor of updating elements */
[aria-busy="true"] {
  cursor: progress;
}
/* Specify the pointer cursor of trigger elements */
[aria-controls] {
  cursor: pointer;
}
/* Specify the unstyled cursor of disabled, not-editable, or otherwise inoperable elements */
[aria-disabled] {
  cursor: default;
}
</style>
