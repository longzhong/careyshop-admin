<template>
  <el-form
    :inline="true"
    :model="form"
    ref="form"
    size="mini"
    style="margin-bottom: -18px;">

    <el-form-item label="账号类型" prop="client_type">
      <el-select
        v-model="form.client_type"
        clearable
        placeholder="请选择"
        style="width: 100px;"
        value="">
        <el-option
          v-for="(item, index) in group"
          :key="index"
          :label="item"
          :value="index"/>
      </el-select>
    </el-form-item>

    <el-form-item label="账号" prop="username">
      <el-input
        v-model="form.username"
        clearable
        placeholder="请输入账号"
        style="width: 125px;"/>
    </el-form-item>

    <el-form-item label="路径" prop="path">
      <el-input
        v-model="form.path"
        clearable
        placeholder="请输入路径"
        style="width: 125px;"/>
    </el-form-item>

    <el-form-item label="时间段" prop="time_period">
      <el-date-picker
        v-model="form.time_period"
        type="datetimerange"
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        style="width: 320px;">
      </el-date-picker>
    </el-form-item>

    <el-form-item>
      <el-button
        type="primary"
        :disabled="loading"
        @click="handleFormSubmit(true)">
        <cs-icon name="search"/>
        查询
      </el-button>
    </el-form-item>

    <el-form-item>
      <el-button
        @click="handleFormReset">
        <cs-icon name="refresh"/>
        重置
      </el-button>
    </el-form-item>

  </el-form>
</template>

<script>
export default {
  props: {
    loading: {
      default: false
    },
    group: {
      default: () => {}
    }
  },
  data() {
    return {
      form: {
        client_type: undefined,
        username: undefined,
        path: undefined,
        begin_time: undefined,
        end_time: undefined,
        time_period: null
      }
    }
  },
  methods: {
    handleFormSubmit(isRestore = false) {
      let form = []
      for (const index in this.form) {
        if (!this.form.hasOwnProperty(index) || !this.form[index]) {
          continue
        }

        // 时间段处理
        if (index === 'time_period') {
          if (this.form[index].length === 2) {
            form['begin_time'] = this.form[index][0].toUTCString()
            form['end_time'] = this.form[index][1].toUTCString()
          }

          continue
        }

        form[index] = this.form[index]
      }

      this.$emit('submit', form, isRestore)
    },
    handleFormReset() {
      this.$refs.form.resetFields()
    }
  }
}
</script>
