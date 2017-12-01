<template>
  <div class="container">
    <div class="header" @click="overview" :style="showOverview ? headerOverflowStyle : headerNormalStyle">
      <span class="title">拖延症的列表</span>
    </div>
    <div class="list" :style="showOverview ? listOverflowStyle : listNormalStyle">
      <transition-group name="list-complete" tag="div">
        <div class="cell" v-for="(item, index) in items" :key="index" @click="showAction(index)">
          <div class="content">
            <span class="title">{{ item.title }}</span>
          </div>
          <div class="action" :style="index == expandIndex ? actionShownStyle : actionNormalStyle">
            <md-button class="md-icon-button">
              <md-icon>drag_handle</md-icon>
            </md-button>
            <md-button class="md-icon-button">
              <md-icon>edit</md-icon>
            </md-button>
            <md-button class="md-icon-button">
              <md-icon>delete</md-icon>
            </md-button>
            <md-button class="md-icon-button">
              <md-icon>{{ item.completed ? 'clear' : 'done' }}</md-icon>
            </md-button>
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
      md-input-maxlength="100"
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
        margin: '0 50px 30px 50px',
        overflow: 'hidden'
      },
      headerNormalStyle: {
        height: '60px',
        'box-shadow': '0px 5px 8px rgba(180, 198, 215, 0.5)',
        margin: 0,
        'border-radius': 0
      },
      headerOverflowStyle: {
        height: '40px',
        'box-shadow': '0px 0px 8px rgba(180, 198, 215, 0.75)',
        margin: '30px 50px 30px 50px',
        'border-radius': '20px'
      },
      actionNormalStyle: {
        height: '0px',
        opacity: 0
      },
      actionShownStyle: {
        height: '50px',
        opacity: 1
      },
      expandIndex: -1
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
    }
  },
  mounted: function () {
    for (var i = 0; i < 50; i++) {
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
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 0;
  transition: all .5s ease;
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
