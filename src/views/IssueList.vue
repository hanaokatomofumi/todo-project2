<template>
   <div>
    <h1>issueリスト</h1>
    <el-button type="success" @click="getIssues()">issue取得</el-button>
    <el-row :gutter="12">
    
      <TodoItem v-for="( todo, index ) in todos" :key="index" />
      <el-col :span="12"  v-for="( issue, index ) in issues" :key="issue.id">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0;">
          <el-row :gutter="12">
            <el-col :span="21">{{ issue.title }}</el-col>
            <el-col :span="3">
              
              <el-button @click="closeIssue(index)" type="success" icon="el-icon-check" circle></el-button>
            </el-col>
          </el-row>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>


<script>
import axios from 'axios';
import TodoItem from '@/components/TodoItem';
const client = axios.create({
  baseURL: 'https://api.github.com/repos/hanaokatomofumi/Todo-project',
  headers: {
   'Accept': 'application/vnd.github.v3+json',
   'Content-Type':'application/json',
    'Authorization': 'token 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'
 },
})

export default {
  name: 'TodosIssues',
  components: {
    TodoItem
  },
  data() {
    return {
      issues: [],
    }
  },
  methods: {
    getIssues() {
      client.get('/issues')
        .then((res) => {
          this.issues = res.data;
      })
    },
    closeIssue(index){
      const target = this.issues[index];
      client.patch(`/issues/${target.number}`,
          {
            state: 'closed'
          },
        )
        .then(() => {
          this.issues.splice(index, 1);
      })
    },
  },
  // created() {
    // this.getIssues();
  // }
}
</script>