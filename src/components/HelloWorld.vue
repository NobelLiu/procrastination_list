<template>
  <div class="container">
    <div class="header" :style="showOverview ? headerOverflowStyle : headerNormalStyle">
      <span class="title">{{ title }}</span>
      <md-button active class="md-icon-button" :style="{opacity: showOverview ? 1 : 0}" @click="editTitle">
        <md-icon>edit</md-icon>
      </md-button>
    </div>
    <div class="list" :style="showOverview ? listOverflowStyle : listNormalStyle">
      <transition-group name="list-complete" tag="div">
        <div class="cell" v-for="(item, index) in sortedItems" :style="item.completed ? completedItem : normalItem" :key="index" @click="showAction(index)">
          <div class="content">
            <span class="title" :style="{textDecoration: item.completed ? 'line-through' : 'none'}">{{ item.title }}</span>
          </div>
          <div class="action" :style="index == expandIndex ? actionShownStyle : actionNormalStyle">
            <md-button class="md-icon-button" @click="moveItem(index)">
              <md-icon>drag_handle</md-icon>
            </md-button>
            <md-button class="md-icon-button" @click="editItem(index)">
              <md-icon>edit</md-icon>
            </md-button>
            <md-button class="md-icon-button" @click="deleteItem(index)">
              <md-icon>delete</md-icon>
            </md-button>
            <md-button class="md-icon-button" @click="completeItem(index)">
              <md-icon>{{ item.completed ? 'clear' : 'done' }}</md-icon>
            </md-button>
          </div>
        </div>
      </transition-group>

      <div class="empty-state" v-if="!items.length > 0">
        <md-icon  style="margin: 0;">inbox</md-icon>
        <span class="title">空空如也</span>
        <div class="content-box">
          <span class="content">点击下方</span>
          <md-button class="md-icon-button md-raised button" @click="addItem">
            <md-icon>add</md-icon>
          </md-button>
          <span class="content">添加第一条</span>
        </div>
      </div>
    </div>
    <div class="action">
      <md-button class="md-icon-button md-raised button" @click="addItem">
        <md-icon>add</md-icon>
      </md-button>
      <md-button class="md-icon-button md-raised button"  @click="overview">
        <md-icon>view_column</md-icon>
      </md-button>
    </div>


    <md-dialog-prompt
      :md-active.sync="dialogActive"
      v-model="dialogValue"
      :md-title="dialogTitle"
      :md-input-maxlength="dialogMaxLength"
      :md-input-placeholder="dialogPlaceHolder"
      :md-confirm-text="dialogConfirmText"
      md-cancel-text="取消"
      @md-cancel="onDialogCancel"
      @md-confirm="onDialogConfirm"
      style="background: white"/>

    <md-snackbar :md-duration="1000" :md-active.sync="showSnackbar" style="background: white" md-persistent>
      <span>成功添加：{{ dialogValue }}</span>
      <md-button class="md-primary" @click="onSnackBarCancel">取消</md-button>
    </md-snackbar>


  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      title: '拖延症列表',
      items: [],
      menuVisible: false,
      showSnackbar: false,
      showOverview: false,
      listNormalStyle: {
        'box-shadow': 'none',
        'border-radius': '0px',
        margin: '0',
        overflow: 'auto'
      },
      listOverflowStyle: {
        'box-shadow': '0px 0px 8px rgba(180, 198, 215, 0.75)',
        'border-radius': '5px',
        margin: '0 50px',
        overflow: 'hidden'
      },
      headerNormalStyle: {
        height: '60px',
        'box-shadow': '0px 5px 8px rgba(180, 198, 215, 0.5)',
        margin: 0,
        'border-radius': 0
      },
      headerOverflowStyle: {
        height: '52px',
        'box-shadow': '0px 0px 8px rgba(180, 198, 215, 0.75)',
        margin: '20px 50px 20px 50px',
        'border-radius': '26px'
      },
      actionNormalStyle: {
        height: '0px',
        opacity: 0
      },
      actionShownStyle: {
        height: '50px',
        opacity: 1
      },
      expandIndex: -1,
      completedItem: {
        opacity: 0.5
      },
      normalItem: {
        opacity: 1
      },
      dialogActive: false,
      dialogValue: '',
      dialogTitle: '',
      dialogContent: '',
      dialogPlaceHolder: '',
      dialogConfirmText: '',
      dialogMaxLength: 0
    }
  },
  computed: {
    percentage: function () {
      if (!this.items.length > 0) {
        return 0
      }
      var p = 0
      for (var i = 0; i < this.items.length; i++) {
        if (this.items[i].completed) {
          p = p + 1
        }
      }
      return Math.ceil(p / this.items.length * 100)
    },
    sortedItems: function () {
      var u = []
      var f = []
      this.items.forEach(function (item) {
        if (item.completed) {
          f.push(item)
        } else {
          u.push(item)
        }
      })
      return this.items
      // return u.concat(f)
    }
  },
  methods: {
    onDialogConfirm: function () {
      if (this.dialogValue.length > 0) {
        this.items.push({ title: this.dialogValue, completed: false })
        this.saveToLocalStorage()
        this.showSnackbar = true
      }
    },
    onDialogCancel: function () {
      this.dialogValue = ''
    },
    onSnackBarCancel: function () {
      this.showSnackbar = false
    },
    overview: function () {
      this.showOverview = !this.showOverview
    },
    showAction: function (index) {
      if (index === this.expandIndex) {
        this.expandIndex = -1
      } else {
        this.expandIndex = index
      }
    },
    addItem: function () {
      this.showInputDialog('添加', '力求简洁', '', '点此键入', 30, '添加')
    },
    deleteItem: function (index) {
      this.items.shift(index)
      this.saveToLocalStorage()
    },
    saveToLocalStorage: function () {
      localStorage.setItem('items', JSON.stringify(this.items))
    },
    showInputDialog: function (title, content, value, placeHolder, maxLength, confirmText) {
      this.dialogValue = value
      this.dialogTitle = title
      this.dialogContent = content
      this.dialogPlaceHolder = placeHolder
      this.dialogMaxLength = maxLength
      this.dialogConfirmText = confirmText
      this.dialogActive = true
    },
    editTitle: function () {
      this.showInputDialog('修改标题', '列表的名称', this.title, '点此键入', 10, '确定')
    }
  },
  mounted: function () {
    for (var i = 0; i < 10; i++) {
      this.items = JSON.parse(localStorage.getItem('items'))
      // this.items.push({title: Math.random() * 1000, completed: Math.random() > 0.5})
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.container {
  display: flex;
  flex: 1;
  flex-direction: column;
}

.header {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  z-index: 0;
  transition: all .35s ease;
}

.header .title {
  padding: 0 0px 0 52px;
  background: none;
  border: none;
  text-align: center;
  width: 100%;
  user-select: none;
}

.title {
  font-size: 18px;
}

.list {
  flex: 1;
  transition: all .35s ease;
}

.add {
  margin: 15px;
  background: white;
  transition: all .35s ease;
}

.cell {
  display: flex;
  flex-direction: column;
  background: white;
  margin: 8px;
  border-radius: 5px;
}

.cell .content {
  padding: 15px;
}

.cell .action {
  display: flex;
  justify-content: space-around;
  align-items: center;
  overflow: hidden;
  transition: all .35s ease;
}

.md-icon-button {
  color: #b4c6d7;
}

.container> .action {
  padding: 20px;
  display: flex;
  justify-content: space-around;
}

.action> .button {
  background: white;
}

.empty-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.empty-state> .title {
  margin: 20px 0;
}

.content-box {
  display: flex;
  align-items: center;
}

.content-box> .button {
  background: white;
}

h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.list-complete-item {
  transition: all 1s;
  display: inline-block;
  margin-right: 10px;
}
.list-complete-enter, .list-complete-leave-to
  /* .list-complete-leave-active for below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
.list-complete-leave-active {
  position: absolute;
}
</style>
