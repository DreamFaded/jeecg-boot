<template>
  <div>
    <a-card :borderd="false">


      <a-button @click="handleTableCheck" type="primary">表单验证</a-button>
      <span style="padding-left:8px;"></span>
      <a-tooltip placement="top" title="获取值，忽略表单验证" :autoAdjustOverflow="true">
        <a-button @click="handleTableGet" type="primary">获取值</a-button>
      </a-tooltip>
      <span style="padding-left:8px;"></span>
      <a-tooltip placement="top" title="模拟加载1000条数据" :autoAdjustOverflow="true">
        <a-button @click="handleTableSet" type="primary">设置值</a-button>
      </a-tooltip>


      <j-editable-table
        ref="editableTable"
        :loading="loading"
        :columns="columns"
        :dataSource="dataSource"
        :rowNumber="true"
        :rowSelection="true"
        :actionButton="true"
        style="margin-top: 8px;"
        @selectRowChange="handleSelectRowChange">

        <template v-slot:action="props">
          <a @click="handleDelete(props)">{{ props.text }}</a>
        </template>

      </j-editable-table>


    </a-card>
    <br>
    <a-card title="只读列表">
      <j-editable-table
        :columns="columns1"
        :dataSource="dataSource1"
        :rowNumber="true"
        :rowSelection="true"
        :maxHeight="200"
        :disabled="true"
      />
    </a-card>
  </div>
</template>

<script>
  import moment from 'moment'
  import JEditableTable from '@/components/jeecg/JEditableTable'
  import { FormTypes } from '@/utils/JEditableTableUtil'
  import { randomUUID, randomNumber } from '@/utils/util'

  export default {
    name: 'JeecgEditableTableExample',
    components: {
      JEditableTable
    },
    data() {
      return {
        loading: false,
        columns: [
          {
            title: '字段名称',
            key: 'dbFieldName',
            // width: '19%',
            width: '300px',
            type: FormTypes.input,
            defaultValue: '',
            placeholder: '请输入${title}',
            validateRules: [
              {
                required: true, // 必填
                message: '请输入${title}' // 显示的文本
              },
              {
                pattern: /^[a-z|A-Z][a-z|A-Z\d_-]{0,}$/, // 正则
                message: '${title}必须以字母开头，可包含数字、下划线、横杠'
              }
            ]
          },
          {
            title: '文件域',
            key: 'upload',
            type: FormTypes.upload,
            // width: '19%',
            width: '300px',
            placeholder: '点击上传',
            token: true,
            responseName: 'message',
            action: window._CONFIG['domianURL'] + '/sys/common/upload'
          },
          {
            title: '字段类型',
            key: 'dbFieldType',
            // width: '18%',
            width: '300px',
            type: FormTypes.select,
            options: [ // 下拉选项
              { title: 'String', value: 'string' },
              { title: 'Integer', value: 'int' },
              { title: 'Double', value: 'double' },
              { title: 'Boolean', value: 'boolean' }
            ],
            allowInput: true,
            defaultValue: '',
            placeholder: '请选择${title}',
            validateRules: [{ required: true, message: '请选择${title}' }]
          },
          {
<<<<<<< HEAD
=======
            title: '性别（字典）',
            key: 'sex_dict',
            width: '300px',
            type: FormTypes.select,
            options: [],
            dictCode: 'sex',
            placeholder: '请选择${title}',
            validateRules: [{ required: true, message: '请选择${title}' }]
          },
          {
>>>>>>> branch 'master' of git@github.com:zhangdaiscott/jeecg-boot.git
            title: '多选测试',
            key: 'multipleSelect',
            // width: '18%',
            width: '300px',
            type: FormTypes.select,
            props: { 'mode': 'multiple' }, // 支持多选
            options: [
              { title: 'String', value: 'string' },
              { title: 'Integer', value: 'int' },
              { title: 'Double', value: 'double' },
              { title: 'Boolean', value: 'boolean' }
            ],
            defaultValue: ['int', 'boolean'], // 多个默认项
            // defaultValue: 'string,double,int', // 也可使用这种方式
            placeholder: '这里可以多选',
            validateRules: [{ required: true, message: '请选择${title}' }]
          },
          {
            title: '字段长度',
            key: 'dbLength',
            // width: '8%',
            width: '100px',
            type: FormTypes.inputNumber,
            defaultValue: 32,
            placeholder: '${title}',
            validateRules: [{ required: true, message: '请输入${title}' }]
          },
          {
            title: '日期',
            key: 'datetime',
            // width: '22%',
            width: '320px',
            type: FormTypes.datetime,
            defaultValue: '2019-4-30 14:52:22',
            placeholder: '请选择${title}',
            validateRules: [{ required: true, message: '请选择${title}' }]
          },
          {
            title: '可以为空',
            key: 'isNull',
            // width: '8%',
            width: '100px',
            type: FormTypes.checkbox,
            customValue: ['Y', 'N'], // true ,false
            defaultChecked: false
          },
          {
            title: '操作',
            key: 'action',
            // width: '8%',
            width: '100px',
            type: FormTypes.slot,
            slotName: 'action',
            defaultValue: '删除'
          }

        ],
        dataSource: [],
        selectedRowIds: [],
        // table2
        columns1: [
          {
            title: '输入框',
            key: 'input',
            type: FormTypes.input,
            placeholder: '清输入'
          },
          {
            title: '下拉框',
            key: 'select',
            type: FormTypes.select,
            options: [
              { title: 'String', value: 'string' },
              { title: 'Integer', value: 'int' },
              { title: 'Double', value: 'double' },
              { title: 'Boolean', value: 'boolean' }
            ],
            placeholder: '请选择'
          },
          {
            title: '多选框',
            key: 'checkbox',
            type: FormTypes.checkbox,
            customValue: [true, false]
          },
          {
            title: '日期',
            key: 'datetime',
            type: FormTypes.datetime
          }
        ],
        dataSource1: []
      }
    },
    created() {

    },
    mounted() {
      this.randomData(23, false)
      this.dataSource1 = [
        { input: 'hello', select: 'int', checkbox: true, datetime: '2019-6-17 14:50:48' },
        { input: 'world', select: 'string', checkbox: false, datetime: '2019-6-16 14:50:48' },
        { input: 'one', select: 'double', checkbox: true, datetime: '2019-6-17 15:50:48' },
        { input: 'two', select: 'boolean', checkbox: false, datetime: '2019-6-14 14:50:48' },
        { input: 'three', select: '', checkbox: false, datetime: '2019-6-13 14:50:48' }
      ]
    },
    methods: {

      /** 表单验证 */
      handleTableCheck() {
        this.$refs.editableTable.getValues((error) => {
          if (error === 0) {
            this.$message.success('验证通过')
          } else {
            this.$message.error('验证未通过')
          }
        })
      },
      /** 获取值，忽略表单验证 */
      handleTableGet() {
        this.$refs.editableTable.getValues((error, values) => {
          console.log('values:', values)
        }, false)
        console.log('deleteIds:', this.$refs.editableTable.getDeleteIds())

        this.$message.info('获取值成功，请看控制台输出')

      },
      /** 模拟加载1000条数据 */
      handleTableSet() {
        this.randomData(1000, true)
      },

      handleSelectRowChange(selectedRowIds) {
        this.selectedRowIds = selectedRowIds
      },

      /* 随机生成数据 */
      randomData(size, loading = false) {
        if (loading) {
          this.loading = true
        }

        let randomDatetime = () => {
          let time = parseInt(randomNumber(1000, 9999999999999))
          return moment(new Date(time)).format('YYYY-MM-DD HH:mm:ss')
        }

        let begin = Date.now()
        let values = []
        for (let i = 0; i < size; i++) {
          values.push({
            id: randomUUID(),
            dbFieldName: `name_${i + 1}`,
            // dbFieldTxt: randomString(10),
            multipleSelect: ['string', ['int', 'double', 'boolean'][randomNumber(0, 2)]],
            dbFieldType: ['string', 'int', 'double', 'boolean'][randomNumber(0, 3)],
            dbLength: randomNumber(0, 233),
            datetime: randomDatetime(),
            isNull: ['Y', 'N'][randomNumber(0, 1)]
          })
        }

        this.dataSource = values
        let end = Date.now()
        let diff = end - begin

        if (loading && diff < size) {
          setTimeout(() => {
            this.loading = false
          }, size - diff)
        }

      },

      handleDelete(props) {
        let { rowId, target } = props
        target.removeRows(rowId)
      }

    }
  }
</script>

<style scoped>

</style>