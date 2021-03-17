<template>
  <div class="todolist">
    <div
      class="input-update"
      v-for="(item,index) in items"
      v-bind:key="item.id"
    >
      <input type="item.message" v-model="item.message" />
      <!-- 
      $eventは子からデータを受け取った時に記述するので$emitの第二引数部分には渡したい値を記述します
      todoのidを渡すと良いと思います！
      -->
      <button class="edit-button" @click="$emit('edit', id,index)"></button>
        更新
      <button class="remove-button" @click="$emit('remove', id,index)">
        削除
      </button>
    </div>
  </div>
</template>
<script>
export default {
  name: "todo-list",
  props: {
    items: {
      type: Array,
      default: function() {
        return [];
      },
    },
  },
};
</script>

<style>
.input {
  width: 80%;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  font-size: 14px;
  outline: none;
  vertical-align: middle;
}
.todolist {
  margin: 0;
  padding: 0;
  border: 0;
  outline: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
}
.edit-button {
  text-align: left;
  border: 2px solid #fa9770;
  font-size: 12px;
  color: #fa9770;
  background-color: #fff;
  font-weight: bold;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.4s;
  outline: none;
}
.remove-button {
  text-align: left;
  border: 2px solid #71fadc;
  font-size: 12px;
  color: #71fadc;
  background-color: #fff;
  font-weight: bold;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.4s;
  outline: none;
}
</style>
