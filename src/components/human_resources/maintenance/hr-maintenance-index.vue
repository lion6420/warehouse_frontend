<template>
  <div :class="$style.contentContainer">
    <hr-maintenance></hr-maintenance>
    <!--新增人員-->
    <div :class="$style.newData">
      <div :class="$style.contentSubTitle" style="margin-top:20px;">
        <div style="width: 95%;display:flex">
          <div class="fas fa-square" style="color:red;font-size:16px;margin:15px 10px;"></div>
          <div>人員新增</div>
        </div>
        <div :class="$style.submitBtn" @click="submit_check">送出</div>
      </div>
      <div :class="$style.newDataArea">
        <hr-new-data v-for="(row,r_index) in new_data.length" :key="r_index" @removeRow="removeRow" :index="r_index" :new_data="new_data[r_index]"></hr-new-data>
        <div class="col align-self-end">
          <div :class="$style.addRow" @click="addRow()"><span style="position:relative;top:5px" class="fas fa-plus"></span></div>
        </div>
      </div>
    </div>

    <!--檔案上傳-->
    <div :class="$style.fileUpload">
      <div :class="$style.contentSubTitle" style="margin-top:10px;">
        <span class="fas fa-square" style="color:red;font-size:16px;margin:15px 10px"></span>檔案上傳
      </div>
      <div :class="$style.hrUploadArea">
        <hr-upload-file></hr-upload-file>
      </div>
    </div>
  </div>
</template>

<script>
import hrMaintenance from './hr-maintenance'
import hrNewData from './hr-new-data'
import hrUploadFile from './hr-upload-file'

export default {
  components: {
    hrMaintenance,
    hrNewData,
    hrUploadFile,
  },
  data() {
    return {
      //week selection
      week: '35',

      //site BU
      site: '',
      BU: '',

      new_data: [{
        EMP_NO: '',
        EMP_NAME: '',
        SOURCE: '',
        DEPARTMENT: '',
        WHS_TYPE: '',
        AREA: '',
        CLASS: '',
        JOB_NAME: '',
      }],
    }
  },
  created() {
  },
  methods: {
    removeRow(index) {
      var left = this.new_data.slice(0, index)
      var right = this.new_data.slice(index+1)
      this.new_data = left.concat(right)
    },
    addRow() {
      this.new_data.push({
        EMP_NO: '',
        EMP_NAME: '',
        SOURCE: '',
        DEPARTMENT: '',
        WHS_TYPE: '',
        AREA: '',
        CLASS: '',
        JOB_NAME: '',
      })
    },
    reset() {
      this.new_data = [{
        EMP_NO: '',
        EMP_NAME: '',
        SOURCE: '',
        DEPARTMENT: '',
        WHS_TYPE: '',
        AREA: '',
        CLASS: '',
        JOB_NAME: '',
      }]
    },
    submit_check() {
      // 檢查所有input框都填寫
      for (let i=0; i<this.new_data.length; i++) {
        if (
          this.new_data[i]['EMP_NO'] === '' ||
          this.new_data[i]['EMP_NAME'] === '' ||
          this.new_data[i]['SOURCE'] === '' ||
          this.new_data[i]['DEPARTMENT'] === '' ||
          this.new_data[i]['WHS_TYPE'] === '' ||
          this.new_data[i]['AREA'] === '' ||
          this.new_data[i]['CLASS'] === '' ||
          this.new_data[i]['JOB_NAME'] === ''
        ) {
          alert('請將新增資料填寫完整')
          return
        }
      }
      // Confirm message
      var self = this
      this.$confirm({
        title: '新增確認',
        content: '確認後新增結果將會寫入資料庫',
        onOk() {
          self.submit_new_data()
          self.reset()
        },
        onCancel() {
        },
      })
    },
    submit_new_data() {
      this.$message.success('新增成功')
    },
  }
}
</script>
<style lang="scss" module>
@import '@/styles/general.scss';
.contentContainer {
  @include block(100%);
  display:flex;
  flex-direction: column;
  margin-bottom:30px;
  text-align: center;
  .submitBtn {
    @include block(80px, 34px, $radius: 5px);
    @include border();
    font-size: 20px;
    background-color: rgb(62, 158, 62);
    color: #fff;
    text-align: center;
    margin-right: 1vw;
    cursor: pointer;
  }
  .submitBtn:hover {
    background-color: rgb(72, 179, 72);
  }
  .newData {
    @include block(98%, $radius: 5px);
    margin:20px auto 0px auto;
    background-color: var(--bg-color-snd);
    .newDataArea {
      @include block(95%);
      margin:10px auto;
    }
    .addRow {
      @include block(40px, 40px, $radius:50%);
      @include border();
      float:right;
      background-color: rgb(65, 76, 194);
      color: #fff;
      font-size: 20px;
      text-align: center;
      cursor: pointer;
      margin-bottom: 10px;
      margin-top:10px;
    }
    .addRow:hover {
      background-color: rgb(99, 108, 206);
    }
  }
  
  .filterContainer {
    @include block(100%);
    display: flex;
    .filter_EMPID {
      @include block(150px);
    }
    .filter_EMPNAME {
      @include block(150px);
      margin-left: 10px;
    }
  }
  .contentSubTitle {
    @include block(100%);
    display: flex;
    font-size:30px;
    font-family: Microsoft JhengHei;
    font-weight: bold;
    text-align: left;
    color: var(--text-color);
  }
  .fileUpload {
    @include block(98%, $radius: 5px);
    margin:20px auto 0px auto;
    background-color: var(--bg-color-snd);
    .hrUploadArea {
      @include block(95%);
      margin:0px auto 10px auto;
    }
  }
}

</style>
