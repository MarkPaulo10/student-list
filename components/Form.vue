<template>
    <a-form-model :model="form" ref="form" :rules="rules">
        <a-form-model-item label="Name" prop="name">
            <a-input v-model="form.name" />
        </a-form-model-item>
        <a-form-model-item label="Age" prop="age">
            <a-input v-model="form.age" />
        </a-form-model-item>
        <a-form-model-item label="Course" prop="course">
            <a-select v-model="form.course" placeholder="Please Select" allowClear>
                <a-select-option key="BSIT">BSIT</a-select-option>
                <a-select-option key="BSHM">BSHM</a-select-option>
                <a-select-option key="BAFM">BAFM</a-select-option>
                <a-select-option key="BSA">BSA</a-select-option>
            </a-select>
        </a-form-model-item>

        <a-row type="flex" justify="end" :gutter="10">
            <a-col>
                <a-button type="danger" v-if="showReset && !isEdit" @click="clearFields">Reset</a-button>
                <a-button type="danger" v-if="isEdit" @click="cancelEdit">Cancel</a-button>
            </a-col>
            <a-col>
                <a-button type="primary" v-if="!isEdit" @click="addStudent">Add</a-button>
                <a-button type="primary" v-else @click="updateStudent">Save</a-button>
            </a-col>
        </a-row>
    </a-form-model>
</template>
<script>
export default {
    props: ["form", "students", "isEdit"],
    data() {
        return {
            rules: {
                name: [{
                    required: true,
                    message: "Name is required",
                    trigger: "change"
                }],
                age: [{
                    required: true,
                    message: "Age is required",
                    trigger: "change"
                }],
                course: [{
                    required: true,
                    message: "Course is required",
                    trigger: "change"
                }]
            }
        }
    },
    computed: {
        showReset() {
            const name = this.form.name && this.form.name.length;
            const age = this.form.age && this.form.age.length;
            const course = this.form.course && this.form.course.length;
            if(name || age || course) {
                return true;
            } else {
                return false;
            }
        }
    },
    methods: {
        addStudent() {
            const studentLength = this.students.length;
            if(studentLength > 0) {
                this.form.id = this.students[studentLength - 1].id + 1;
            } else {
                this.form.id = 1;
            }
            this.$emit('addStudent', this.form);
        },
        cancelEdit() {
            this.$emit('cancelEdit');
        },
        updateStudent() {
            this.$emit('updateStudent', this.form);
        }
    }
}
</script>
<style>

</style>