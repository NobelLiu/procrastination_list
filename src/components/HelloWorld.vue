<template>
  <div class="container">
    <div class="header">
      <span class="title">拖延症的列表</span>
    </div>
    <div class="list">
      <transition-group name="list-complete" tag="p">
        <div class="cell" v-for="(item, index) in items" :key="index">
          <div class="content">
            <span class="title">{{ item.title }}</span>
          </div>
          <div class="action">
            <span v-if="item.completed">Click again to mark finished</span>
          </div>
        </div>
      </transition-group>
    </div>
    <!--<div v-if="active" class="alert">-->
      <!--<span>alert</span>-->
    <!--</div>-->
    <md-button class="md-icon-button md-raised add" @click="value = '', active = true">
      <md-icon>add</md-icon>
    </md-button>


    <md-dialog-prompt
      :md-active.sync="active"
      v-model="value"
      md-title="添加"
      md-input-maxlength="30"
      md-input-placeholder="填写内容"
      md-confirm-text="添加"
      md-cancel-text="取消"
      @md-cancel="onDialogCancel"
      @md-confirm="onDialogConfirm"
      style="background: white"/>

    <md-snackbar :md-duration="1000" :md-active.sync="showSnackbar" style="background: white" md-persistent>
      <span>成功添加：{{ value }}</span>
      <md-button class="md-primary" @click="onSnackBarCancel">取消</md-button>
    </md-snackbar>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      items: [],
      menuVisible: false,
      active: false,
      value: '',
      showSnackbar: false
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
    }
  },
  methods: {
    onDialogConfirm: function () {
      if (this.value.length > 0) {
        this.items.push({ title: this.value, completed: false })
        this.showSnackbar = true
      }
    },
    onDialogCancel: function () {
      this.value = ''
    },
    onSnackBarCancel: function () {
      this.showSnackbar = false
    }
  },
  mounted: function () {
    for (var i = 0; i < 0; i++) {
      this.items.push({title: Math.random() * 1000, completed: Math.random() > 0.5})
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
  height: 60px;
  display: flex;
  flex-direction: column;
  box-shadow: 0px 2px 4px rgba(180, 198, 215, 0.5);
  justify-content: center;
  align-items: center;
  z-index: 0;
}

.title {
  font-size: 18px;
}

.list {
  flex: 1;
  overflow: auto;
  box-shadow: 0px 0px 8px rgba(180, 198, 215, 0.5);
  border-radius: 5px;
  margin: 50px;
  transition: all .5s ease;
}

.add {
  margin: 15px;
  background: white;
}

.cell {
  display: flex;
  flex-direction: row;
  background: white;
  margin: 8px;
  border-radius: 5px;
}

.cell .content {
  flex: 1;
  padding: 15px;
}

.cell .action {
  width: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
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
