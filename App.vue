<template>
  <view class="container">
    <scroll-view class="todo-list">
      <animated:view :style="{
        opacity: dimmingAnimation
      }">
        <text class="text-header">Your TODO list</text>
        <todo-item
          v-for="(todo, index) in todoList"
          :index="index"
          :text="todo.text"
          :onPress="markTodoAsDone"
          :key="index"></todo-item>
        <view v-if="doneList.length > 0">
          <text class="text-header">Done</text>
          <todo-item
            v-for="(todo, index) in doneList"
            :text="todo.text"
            :index="index"
            :onPress="()=>{}"
            :key="index"
            :done="true">
          </todo-item>
          <touchable-opacity
            :onPress="() => { doneList = [] }"
            :style="{ justifySelf: 'center', alignItems: 'center', marginVertical: 10 }">
            <text>Clear all</text>
          </touchable-opacity>
          <view class="hollow-brick"></view>
        </view>
      </animated:view>
    </scroll-view>
    <view
      class="addItemContainer"
      :class="newTodoScreen ? '':'hidden'"
      :style="{shadowOffset: {height: 0, width: 0}}">
      <text class="text-header">Add new todo</text>
      <text-input
        class="text-input"
        placeholder="Buy a piano"
        v-model="newTodoText"
      />
      <touchable-opacity 
        title="add"
        class="button2"
        :onPress="()=> { addTodo() }">
        <text class="text-button">Add</text>
      </touchable-opacity>
    </view>
    <touchable-opacity
      class="addItemButton"
      title="add"
      :style="{
        shadowOffset: {height: 2, width: 0}
      }"
      :onPress="()=>{ newTodoScreen = !newTodoScreen }">
      <view>
        <animated:view
          :style="{
            transform: [{rotate: rotateAnimation}]
          }">
          <image :source="plusIcon" class="image-plus"></image>
        </animated:view>
      </view>
    </touchable-opacity>
  </view>
</template>
<script>
import TodoItem from './components/TodoItem'
import plusIcon from './assets/add.png'
import { Animated, Easing } from "react-native";

export default {
  data: function () {
    return {
      plusIcon,
      todoList: [],
      doneList: [],
      newTodoScreen: false,
      newTodoText: '',
      rotateAnimation: {},
      dimmingAnimation: {},
      buttonRotationDeg: 0,
      todoListOpacity: 1
    }
  },
  components: {
    TodoItem
  },
  created () {
    this.buttonRotationDeg = new Animated.Value(0)
    this.todoListOpacity = new Animated.Value(1)
    this.rotateAnimation = this.buttonRotationDeg.interpolate({
      inputRange: [0, 1],
      outputRange: ['0deg', '45deg']
    })
    this.dimmingAnimation = this.todoListOpacity.interpolate({
      inputRange: [0, 1],
      outputRange: [0.1, 1]
    })
  },
  methods: {
    markTodoAsDone (index) {
      this.doneList.push(this.todoList[index])
      this.todoList.splice(index, 1)
    },
    addTodo () {
      if (this.newTodoText !== '') {
        this.todoList.push({text: this.newTodoText})
        this.newTodoScreen = false
      }
    },
    rotateButton () {
      this.buttonRotationDeg.setValue(this.newTodoScreen ? 0:1)
      Animated.timing(this.buttonRotationDeg, {
        toValue: this.newTodoScreen ? 1:0,
        duration: 120,
        easing: Easing.linear
      }).start()
    },
    dimTodoList () {
      this.todoListOpacity.setValue(this.newTodoScreen ? 1:0)
      Animated.timing(this.todoListOpacity, {
        toValue: this.newTodoScreen ? 0:1,
        duration: 90,
        easing: Easing.linear
      }).start()
    }
  },
  watch: {
    newTodoScreen () {
      this.newTodoText = ''
      this.rotateButton()
      this.dimTodoList()
    }
  }
}
</script>

<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}

.text-color-primary {
  color: white;
}

.addItemButton {
  width: 70px;
  height: 70px;
  background-color: #FF3352;
  shadow-color: #ff3352;
  shadow-opacity: 0.6;
  shadow-radius: 6px;
  justify-content: center;
  align-items: center;
  padding: 10px;
  position: absolute;
  bottom: 30px;
  border-radius: 100px;
  z-index: 3;
}

.button2 {
  align-self: center;
  border-radius: 5px;
  padding-horizontal: 30px;
  padding-vertical: 12px;
  background-color: #f7f7f7;
}

.text-button {
  color: #222;
  font-size: 18px;
  font-weight: bold;
}

.addItemContainer {
  width: 95%;
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  position: absolute;
  top: 170px;
  z-index: 2;
  shadow-color: black;
  shadow-opacity: 0.16;
  shadow-radius: 5px;
}

.text-input {
  margin-vertical: 20px;
  color: #222;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 12px;
  font-size: 21px;
}

.hidden {
  display: none;
}

.todo-list {
  padding-top: 60px;
  flex: 1;
  width: 100%;
  padding-horizontal: 20px;
}

.button {
  font-size: 16px;
  color: black;
  font-weight: 600;
}

.hollow-brick {
  width: 100%;
  height: 180px;
}

.image-plus {
  width: 40px;
  height: 40px;
}

.text-header {
  text-align: left;
  align-self: flex-start;
  margin-bottom: 15px;
  font-size: 26px;
  font-weight: bold;
}

</style>
