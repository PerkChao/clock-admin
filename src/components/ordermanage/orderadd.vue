<template>
  <div class="order-add">
    <div class="order-add-info">
      <div class="common-input">
        <h3><span>*</span>订单类型：</h3>
        <Select
          v-model="projectQuery.orderType"
          style="width:200px;margin-left: 10px"
        >
          <Option
            v-for="(item,index) in languageList"
            :value="item.value"
            :key="index"
          >{{ item.label }}</Option>
        </Select>
      </div>
      <div class="common-input">
        <h3><span>*</span>选择客户：</h3>
        <Select
          v-model="projectQuery.orderName"
          style="width:200px;margin-left: 10px"
        >
          <Option
            v-for="(item,index) in languageList"
            :value="item.value"
            :key="index"
          >{{ item.label }}</Option>
        </Select>
      </div>
      <div class="common-input">
        <h3><span>*</span>产品名称：</h3>
        <Select
          v-model="projectQuery.orderProduct"
          style="width:200px;margin-left: 10px"
        >
          <Option
            v-for="(item,index) in languageList"
            :value="item.value"
            :key="index"
          >{{ item.label }}</Option>
        </Select>
      </div>
      <div class="common-input">
        <h3><span>*</span>加工工艺：</h3>
        <Select
          v-model="projectQuery.orderName"
          style="width:200px;margin-left: 10px"
        >
          <Option
            v-for="(item,index) in languageList"
            :value="item.value"
            :key="index"
          >{{ item.label }}</Option>
        </Select>
      </div>
      <div class="price">
        <div class="number-list">
          <h3><span>*</span>单价：</h3>
          <InputNumber
            :max="10000"
            v-model="projectQuery.price"
            :formatter="value => `￥ ${value}`.replace(/B(?=(d{3})+(?!d))/g, ',')"
            :parser="value => value.replace(/$s?|(,*)/g, '')"></InputNumber>
        </div>
        <div class="number-list">
          <h3><span>*</span>数量：</h3>
          <InputNumber
            :max="10000"
            v-model="projectQuery.num"
            :formatter="value => `${value}`.replace(/B(?=(d{3})+(?!d))/g, ',')"
            :parser="value => value.replace(/$s?|(,*)/g, '')"></InputNumber>
        </div>
        <div class="number-list">
          <h3><span>*</span>订单总额：</h3>
          <InputNumber
            v-model="total"
            disabled="disabled"
            ></InputNumber>
        </div>
      </div>
      <div class="add-btn">
        <h3><span>*</span>所需材料：</h3>
        <Button
          class="add"
          type="primary"
          size="small"
          @click="addMaterial"
        >+添加</Button>
      </div>
      <div class="material">
        <div
          class="list"
          v-for="(data, index) in projectQuery.material"
          :key="index"
        >
          <div class="common-input">
            <h3><span>*</span>材料{{index + 1}}：</h3>
            <Select
              v-model="data.name"
              style="width:200px;margin-left: 10px"
            >
              <Option
                v-for="(item,index) in languageList"
                :value="item.value"
                :key="index"
              >{{ item.label }}</Option>
            </Select>
          </div>
          <div class="number-list">
            <h3><span>*</span>数量：</h3>
            <InputNumber
              :max="10000"
              v-model="data.number"
              :formatter="value => `${value}`.replace(/B(?=(d{3})+(?!d))/g, ',')"
              :parser="value => value.replace(/$s?|(,*)/g, '')"></InputNumber>
          </div>
          <Button
            class="btn"
            type="default"
            @click="delMaterial(data)"
          >删除</Button>
        </div>
      </div>
      <div class="date">
        <h3><span>*</span>交货日期：</h3>
        <Row>
          <Col span="12">
            <DatePicker
              type="date"
              placeholder="Select date"
              style="width: 200px"
              v-model="projectQuery.date"
            ></DatePicker>
          </Col>
        </Row>
      </div>
      <div class="remark">
        <h3><span>*</span>跟单备注</h3>
        <Input
          class="textarea"
          v-model="projectQuery.textarea"
          type="textarea"
          :rows="6"
          style="resize: none"
          placeholder="填写内容"
        ></Input>
      </div>
    </div>
    <div class="double-btn">
      <Button
        class="btn-cancal"
        type="default"
        @click="cancal"
      >取消</Button>
      <Button
        class="btn-first"
        type="primary"
        @click="sendTo"
      >下一步</Button>
    </div>
  </div>
</template>

<script>
export default {
  name: "orderadd",
  methods: {
    //增加新的材料列表
    addMaterial() {
      const data = {
        name: '',
        number: 0,
      };
      this.projectQuery.material.push(data);
    },
    //删除增加的材料
    delMaterial(data) {
      this.projectQuery.material = this.projectQuery.material.filter(item => {
        return data != item;
      });
    },
    //取消
    cancal() {

    },
    //确认
    sendTo() {
      this.$router.push({
        path: '/writeplan',
        query: {id: 1},
      })
    },
  },
  data() {
    return {
      //总计
      total: 0,
      //提交数据
      projectQuery: {
        orderType: '',        //订单类型
        orderName: '',        //选择客户
        orderProduct: '',     //产品名称
        orderStyle: '',       //加工工艺
        num: 0,              //数量
        price: 0,           //单价
        material: [
          {
            name: '',
            number: 0,
          },
        ],
        date: '',
        textarea: '',
      },
      //订单类型
      languageList: [

      ],
    };
  },
}
</script>

<style scoped lang="less">
  .order-add{
    background: #fff;
    .order-add-info{
      padding-top: 20px;
    }
    .price{
      display: flex;
    }
    .add-btn{
      padding: 20px;
      display: flex;
      .add{
        background-color: #003c78;
        border-color: #003c78;
      }
    }
    .material{
      .list{
        display: flex;
        .btn{
          margin: 10px 20px;
        }
      }
    }
    .date{
      display: flex;
      align-items: center;
      margin-left: 20px;
      padding: 20px 0;
    }
    .remark{
      padding: 20px;
      display: flex;
      .textarea{
        margin-left: 10px;
        width: 530px;
      }
    }
    .double-btn{
      padding-bottom: 60px;
      .btn-cancal{
        margin: 0 20px 0 40px;
      }
    }
  }

  .common-input{
    margin-left: 20px;
    padding: 10px 0;
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }
  .number-list{
    display: flex;
    margin-left: 20px;
    padding: 10px 0;
    justify-content: flex-start;
    align-items: center;
  }
  h3 {
    font-size: 14px;
    margin-left: 20px;
    margin-right: 10px;
    span{
      color: red;
      margin-right: 2px;
      font-size: 16px;
    }
  }
  .btn-first{
    background-color: #003c78;
    border-color: #003c78;
  }
</style>
        
