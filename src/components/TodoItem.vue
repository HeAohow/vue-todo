<template>
    <div :class="{editing: isEdited}">
      <div class="view">
        <input class="toggle" type="checkbox"
          @change='updateChecked($event.target.checked)'/>
        <label @dblclick="editTodo(todo)">{{ title }}</label>
        <button class="destroy" @click="removeTodo(todo)"/>
      </div>
      <input class="edit" type="text"
        v-model="editingTitle"
        v-autofocus
        v-if="isEdited"
        @blur="doneEdit()"
        @keyup.enter="doneEdit()"
        @keyup.esc="cancelEdit()"/>
    </div>
</template>

<script>
import confirmDialog from '../utils/confirm'

export default {
  data () {
    return {
      isEdited: false,
      editingTitle: ''
    }
  },

  props: {
    title: {
      type: String,
      default: ''
    },
    completed: {
      type: Boolean,
      default: false
    }
  },

  methods: {
    editTodo () {
      this.editingTitle = this.title
      this.isEdited = true
    },
    doneEdit () {
      if (this.isEdited) {
        this.$emit('update:title', this.editingTitle)
        this.isEdited = false
      }
    },
    cancelEdit () {
      this.isEdited = false
    },
    updateChecked (completed) {
      this.$emit('update:completed', completed)
    },
    removeTodo () {
      confirmDialog({
        text: '确认要删除嘛？'
      }).then(res => {
        // this.$emit('delete')
        confirmDialog({
          text: '真的确认要删除嘛？'
        }).then(res => {
          this.$emit('delete')
        }).catch(() => {
          console.log('用户点击了取消')
        })
      }).catch(() => {
        console.log('用户点击了取消')
      })
    }
  },

  directives: {
    autofocus: {
      inserted: function (el) {
        el.focus()
      }
    }
  }
}
</script>

<style>
  .todo-list li .editing .view {
    display: none;
  }
  .todo-list li .editing .edit {
    display: block;
    width: 506px;
    padding: 12px 16px;
    margin: 0 0 0 43px;
  }
</style>
