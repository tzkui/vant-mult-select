<template>
  <van-popup class="search-data-popup" v-model="show" position="bottom" :lazy-render="false">
    <div class="header-line">
      <div class="cancel" @click="onSearchCancel">取消</div>
      <div class="title">{{ title }}</div>
      <div class="sure" @click="onSearchConfirm">确定</div>
    </div>
    <div class="tip">已选{{ temp_datas.length }}条</div>
    <div class="lists">
      <div class="line" v-for="(item, index) in options" :key="index" @click="checkedLine(item)">
        <div class="name">{{ item[showWord] }}</div>
        <!-- <img src="@/assets/img/checked.png" v-if="getArrayColumn(temp_datas, valWord).indexOf(item[valWord]) !== -1" /> -->
      </div>
    </div>
  </van-popup>
</template>
<script>
import { Popup } from 'vant';
import "vant/lib/popup/index.css"
export default {
  name: "searchDataPopup",
  components: {
    "van-popup": Popup
  },
  props: {
    showPicker: Boolean, //是否显示
    datas: Array, //未根据搜索内容筛选的所有列表数据
    defaultDatas: {
      type: Array,
      default() {
        return [];
      }
    }, //默认选中数据
    title: String, //标题
    showWord: {
      type: String,
      default: "name"
    },
    valWord: {
      type: String,
      default: "id"
    },
    hasNone: {
      type: Boolean,
      default: true,
    },
    noneId: {
      type: String,
      default: "0"
    }
  },
  data() {
    return {
      options: [], //根据搜索内容筛选出来的列表数据
      temp_datas: [] //选中的列表数据，通过确定按钮传给父组件
    }
  },
  computed: {
    show: {
      get() {
        return this.showPicker;
      },
      set() {
        this.$emit("cancel");
      },
    },
  },
  watch: {
    defaultDatas(val) {
      console.log(val)
      this.initTempData()
    }
  },
  mounted() {
    this.options = this.datas;
  },
  methods: {
    initTempData() {
      if (this.defaultDatas.length > 0) {
        this.defaultDatas.forEach(item => {
          for (const data of this.datas) {
            if (data.dataCode == item.dataCode) {
              this.temp_datas.push(data);
              break;
            }
          }
        })
      }
    },
    checkedLine(item) {
      if(this.hasNone){
        if(item[this.valWord]==this.noneId){
          this.temp_datas = []
        }else{
          this.temp_datas = this.temp_datas.filter(item => {
            return item[this.valWord] != this.noneId
          })
        }
      }
      //选中行
      if (this.getArrayColumn(this.temp_datas, this.valWord).indexOf(item[this.valWord]) !== -1) {
        //已经选中此行数据，再次点击则取消选中
        this.temp_datas = this.temp_datas.filter(row => {
          return row[this.valWord] !== item[this.valWord];
        });
      } else {
        //未选中此行数据，添加
        this.temp_datas.push(item);
      }
    },
    onSearchCancel() {
      //取消
      this.$emit("cancel");
    },
    onSearchConfirm() {
      //确认
      this.$emit("confirm", this.temp_datas);
    },
    getArrayColumn(arr, column) {
      //获取二维数组指定元素的一维数组
      var temp = [];
      for (let i = 0; i < arr.length; i++) {
        temp.push(arr[i][column]);
      }
      return temp;
    }
  }
}
</script>
<style scoped>
.search-data-popup .tip {
  color: #666;
  padding-bottom: 5px;
  font-size: 16px;
  text-align: center;
}

.search-data-popup .header-line {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 44px;
}

.search-data-popup .header-line .cancel {
  padding: 0 16px;
  font-size: 14px;
  color: #969799;
}

.search-data-popup .header-line .title {
  font-weight: 500;
  font-size: 16px;
  color: #343434;
}

.search-data-popup .header-line .sure {
  padding: 0 16px;
  font-size: 14px;
  color: #576b95;
}

.search-data-popup .lists {
  display: flex;
  flex-direction: column;
  padding: 10px 12px 20px 12px;
  height: 200px;
  overflow: auto;
}

.search-data-popup .lists .line {
  line-height: 40px;
  font-size: 16px;
  color: #000;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.search-data-popup .lists .line img {
  width: 20px;
  height: 20px;
}
</style>
