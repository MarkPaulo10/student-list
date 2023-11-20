<template>
  <div class="layout">
    <a-row type="flex" :gutter="10">
      <a-col :span="8">
        <a-card>
          <Form :form="form" :students="students" @addStudent="addStudent" :isEdit="isEdit" @cancelEdit="cancelEdit" @updateStudent="updateStudent" />
        </a-card>
      </a-col>
      <a-col :span="16">
        <a-card style="height: 100%;">
          <a-table :columns="columns" :dataSource="students">
            <template slot="action" slot-scope="rec">
              <a-row type="flex" :gutter="10">
                <a-col>
                  <a-button type="primary" icon="edit" @click="editStudent(rec)"></a-button>
                </a-col>
                <a-col>
                  <a-button type="danger" icon="delete" @click="deleteStudent(rec)"></a-button>
                </a-col>
              </a-row>
            </template>
          </a-table>
        </a-card>
      </a-col>
    </a-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {},
      students: [],
      columns: [
        {
          title: "Name",
          dataIndex: "name",
          key: "name"
        },
        {
          title: "Age",
          dataIndex: "age",
          key: "age"
        },
        {
          title: "Course",
          dataIndex: "course",
          key: "course"
        },
        {
          title: "Action",
          key: "action",
          scopedSlots: { customRender: "action" }
        }
      ],
      isEdit: false
    }
  },
  methods: {
    clearFields() {
      this.isEdit = false;
      this.form = {};
      this.$forceUpdate();
    },
    addStudent(form) {
      this.students.push(form);
      this.clearFields();
    },
    deleteStudent(rec) {
      const index = this.students.findIndex(student => {
        return student.id === rec.id;
      });
      this.students.splice(index, 1);
    },
    editStudent(rec) {
      this.isEdit = true;
      this.form = JSON.parse(JSON.stringify(rec));
    },
    cancelEdit() {
      this.clearFields();
    },
    updateStudent(form) {
      const index = this.students.findIndex(student => {
        return student.id === form.id;
      });
      this.$set(this.students, index, form);
      this.clearFields();
    }
  }
}
</script>
<style>
.layout {
  padding: 50px;
  background: #ededed;
  height: 100vh;
}
</style>