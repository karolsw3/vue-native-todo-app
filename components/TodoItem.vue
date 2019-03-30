<template>
  <view class="todoItem">
    <text class="todo-text">{{text}}</text>
    <touchable-opacity :on-press="() => {onPress(index)}">
      <image
        :source="CheckIcon"
        class="check-icon"
        :class="done ? 'check-icon--done':'check-icon--normal'">
      </image>
    </touchable-opacity>
  </view>
</template>
<script>
import CheckIcon from '../assets/checked.png'

export default {
  props: {
    text: String,
    index: Number,
    done: {
      type: Boolean,
      required: false
    },
    onPress: Function
  },
  data: function () {
    return {
      CheckIcon
    }
  }
}
</script>

<style>
.todoItem {
  width: 100%;
  align-self: stretch;
  flex-direction: row;
  justify-content: space-between;
  padding: 20px;
  margin-bottom: 15px;
  background-color: #f7f7f7;
  border-radius: 12px;
}

.todo-text {
  font-size: 21px;
  font-weight: 400;
}

.check-icon {
  align-self: flex-end;
  height: 25px;
  width: 25px;
}

.check-icon--normal {
  opacity: 0.2;
}

.check-icon--done {
  opacity: 0.9;
}

</style>
