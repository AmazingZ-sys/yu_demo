<template>
  <div class="main-container">
    <div class="left">
      <el-input v-model="filterText" placeholder="Filter keyword" />

      <el-tree
        ref="treeRef"
        class="filter-tree"
        :data="data"
        :props="defaultProps"
        default-expand-all
        :filter-node-method="filterNode"
      />
    </div>
    <div class="left right">
      <div class="message">
        {{ message }}
      </div>
      <div class="input">
        <el-input v-model="inputText" placeholder="Please input" clearable @keydown.enter="submit" />
        <el-button type="primary" @click="submit">Submit</el-button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';
import { ElTree } from 'element-plus'
import axios from 'axios'
interface Tree {
  id: number
  label: string
  children?: Tree[]
}

// import HelloWorld from './components/HelloWorld.vue';

export default defineComponent({
  name: 'App',
  components: {
    // HelloWorld
  },
  setup() {
    const filterText = ref('')    // 左边筛选框
    const treeRef = ref<InstanceType<typeof ElTree>>()
    const inputText = ref('')    // 右边输入框
    const message = ref('message')    // message

    const defaultProps = {
      children: 'children',
      label: 'label',
    }

    watch(filterText, (val) => {
      treeRef.value!.filter(val)
    })

    const filterNode = (value: string, data: Tree) => {
      if (!value) return true
      return data.label.includes(value)
    }

    const data: Tree[] = [
      {
        id: 1,
        label: 'Level one 1',
        children: [
          {
            id: 4,
            label: 'Level two 1-1',
            children: [
              {
                id: 9,
                label: 'Level three 1-1-1',
              },
              {
                id: 10,
                label: 'Level three 1-1-2',
              },
            ],
          },
        ],
      },
      {
        id: 2,
        label: 'Level one 2',
        children: [
          {
            id: 5,
            label: 'Level two 2-1',
          },
          {
            id: 6,
            label: 'Level two 2-2',
          },
        ],
      },
      {
        id: 3,
        label: 'Level one 3',
        children: [
          {
            id: 7,
            label: 'Level two 3-1',
          },
          {
            id: 8,
            label: 'Level two 3-2',
          },
        ],
      },
    ]

    // 发起后端请求
    const submit = () => {
      // axios.get('url').then(res => {
      //
      // })
      console.log('submit')
    }


    return {
      filterText,
      treeRef,
      filterNode,
      data,
      defaultProps,
      inputText,
      submit,
      message
    }

  }
});
</script>

<style lang="scss">
html, body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  //margin-top: 60px;
}
.main-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  overflow: hidden;
  //background-color: #000;
  .left {
    width: 35%;
    height: 100%;
    overflow: auto;
    box-sizing: border-box;
    padding: 20px;
    .filter-tree {
      margin-top: 20px;
    }
  }
  .right {
    width: 65%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border-left: 1px solid #ececec;
    .input {
      margin-top: 20px;
      display: flex;
      justify-content: center;
      align-items: center
    }
  }
}
</style>
