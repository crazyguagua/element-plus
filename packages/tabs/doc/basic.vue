<template>
  <el-button @click="show = !show">
    显示/隐藏 “定时任务补偿”
  </el-button>
  <el-button
    @click="activeName = activeName === 'first' ? 'second' : 'first'"
  >
    Change
  </el-button>

  <div class="flag">
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane>
        <template #label>用户管理slot-title</template>
        用户管理slot-title
      </el-tab-pane>
      <template v-if="true">
        <el-tab-pane
          label="用户管理"
          name="first"
        >
          用户管理
        </el-tab-pane>
      </template>
      <el-tab-pane
        v-if="show"
        label="定时任务补偿"
        name="fourth"
      >
        定时任务补偿
      </el-tab-pane>
      <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
      <el-tab-pane label="角色管理" name="third">角色管理</el-tab-pane>
      <el-tab-pane v-for="i in 3" :key="i" :label="`a-${i}`">
        {{
          `name-${i}`
        }}
      </el-tab-pane>
    </el-tabs>
  </div>

  <div class="flag">
    <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
      <el-tab-pane
        closable
        label="用户管理"
        name="first"
      >
        用户管理
      </el-tab-pane>
      <el-tab-pane
        disabled
        label="定时任务补偿"
        name="fourth"
      >
        定时任务补偿
      </el-tab-pane>
      <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
      <el-tab-pane label="角色管理" name="third">角色管理</el-tab-pane>
    </el-tabs>
  </div>

  <div class="flag">
    <el-tabs
      v-model="activeName"
      type="border-card"
      @tab-click="handleClick"
    >
      <el-tab-pane
        lazy
        label="用户管理"
        name="first"
      >
        用户管理
      </el-tab-pane>
      <el-tab-pane
        lazy
        label="定时任务补偿"
        name="fourth"
      >
        定时任务补偿
      </el-tab-pane>
      <el-tab-pane
        lazy
        label="配置管理"
        name="second"
      >
        配置管理
      </el-tab-pane>
      <el-tab-pane
        lazy
        label="角色管理"
        name="third"
      >
        角色管理
      </el-tab-pane>
    </el-tabs>
  </div>

  <div class="flag">
    <el-button @click="tabPosition = 'top'">top</el-button>
    <el-button @click="tabPosition = 'right'">right</el-button>
    <el-button @click="tabPosition = 'bottom'">bottom</el-button>
    <el-button @click="tabPosition = 'left'">left</el-button>

    <el-tabs :tab-position="tabPosition" style="height: 200px;">
      <el-tab-pane label="用户管理">用户管理</el-tab-pane>
      <el-tab-pane label="配置管理">配置管理</el-tab-pane>
      <el-tab-pane label="角色管理">角色管理</el-tab-pane>
      <el-tab-pane label="定时任务补偿">定时任务补偿</el-tab-pane>
    </el-tabs>
  </div>

  <div class="flag">
    <el-tabs type="border-card">
      <el-tab-pane>
        <template #label>
          <i class="el-icon-date"></i> 我的行程
        </template>
        我的行程
      </el-tab-pane>
      <el-tab-pane label="消息中心">消息中心</el-tab-pane>
      <el-tab-pane label="角色管理">角色管理</el-tab-pane>
      <el-tab-pane label="定时任务补偿">定时任务补偿</el-tab-pane>
    </el-tabs>
  </div>

  <div class="flag">
    <el-tabs
      v-model="editableTabsValue"
      type="card"
      editable
      style="width: 450px;"
      @edit="handleTabsEdit"
    >
      <el-tab-pane
        v-for="item in editableTabs"
        :key="item.name"
        :label="item.title"
        :name="item.name"
      >
        {{ item.content }}
      </el-tab-pane>
    </el-tabs>
  </div>

  <div class="flag">
    <div style="margin-bottom: 20px;">
      <el-button size="small" @click="addTab(editableTabsValue)">
        add tab
      </el-button>
    </div>
    <el-tabs
      v-model="editableTabsValue"
      type="card"
      stretch
      addable
      closable
      @tab-remove="removeTab"
      @tab-add="addTab(editableTabsValue)"
    >
      <el-tab-pane
        v-for="item in editableTabs"
        :key="item.name"
        :label="item.title"
        :name="item.name"
      >
        {{ item.content }}
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script lang='ts'>
export default {
  data() {
    return {
      show: false,
      activeName: 'second',
      tabPosition: 'left',

      editableTabsValue: '2',
      editableTabs: [
        {
          title: 'Tab 1',
          name: '1',
          content: 'Tab 1 content',
        },
        {
          title: 'Tab 2',
          name: '2',
          content: 'Tab 2 content',
        },
      ],
      tabIndex: 2,
    }
  },
  methods: {
    addTab() {
      let newTabName = ++this.tabIndex + ''
      this.editableTabs.push({
        title: 'New Tab',
        name: newTabName,
        content: 'New Tab content',
      })
      this.editableTabsValue = newTabName
    },
    removeTab(targetName) {
      let tabs = this.editableTabs
      let activeName = this.editableTabsValue
      if (activeName === targetName) {
        tabs.forEach((tab, index) => {
          if (tab.name === targetName) {
            let nextTab = tabs[index + 1] || tabs[index - 1]
            if (nextTab) {
              activeName = nextTab.name
            }
          }
        })
      }
      this.editableTabsValue = activeName
      this.editableTabs = tabs.filter((tab) => tab.name !== targetName)
    },
    handleTabsEdit(targetName, action) {
      if (action === 'add') {
        let newTabName = ++this.tabIndex + ''
        this.editableTabs.push({
          title: 'New Tab',
          name: newTabName,
          content: 'New Tab content',
        })
        this.editableTabsValue = newTabName
      }
      if (action === 'remove') {
        let tabs = this.editableTabs
        let activeName = this.editableTabsValue
        if (activeName === targetName) {
          tabs.forEach((tab, index) => {
            if (tab.name === targetName) {
              let nextTab = tabs[index + 1] || tabs[index - 1]
              if (nextTab) {
                activeName = nextTab.name
              }
            }
          })
        }
        this.editableTabsValue = activeName
        this.editableTabs = tabs.filter(
          (tab) => tab.name !== targetName,
        )
      }
    },
    handleClick(tab, event) {
      console.log(tab, event)
    },
  },
}
</script>
<style scoped>
.flag {
    border: 2px solid #eee;
    margin: 15px 0;
    padding: 10px;
    min-height: 250px;
}
</style>
