<template>
  <div class="popup" :class="{ 'popup-invisible': popupState }">
    <div class="popup-background">
      <button class="btn-X" @click="btnCloseClick"></button>
      <div class="popup-content">
        <div class="delete">Xoá bản ghi</div>
        <div class="popup-content-item">
          <div class="icon"></div>
          <div style="font-size: 18px; font-weight: bold;">Bạn có chắc chắn muốn xóa Nhân viên [{{ employeeCode }}] này?</div>
        </div>
        <div class="popup-content-button">
          <button class="btn-delete" @click="btnAcceptClick">Xác nhận</button>
          <button class="btn-cancel" @click="btnCloseClick">Hủy</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  props: {
    popupState: { type: Boolean, selector: false },
    employeeCode: { type: String, selector: null },
    employeeId: null,
  },
  
  methods: {
    btnCloseClick(){
        this.$emit('hidePopup');
    },
    btnAcceptClick(){
      axios.delete("http://api.manhnv.net/v1/Employees/"+this.employeeId).then(response =>{
        console.log(response.data);
        this.$emit('hidePopup');
        alert('Xóa thành công');
      }).catch(response =>{
        console.log(response.data);
        alert('Xóa thất bại');
      })
    }
  },
};
</script>
<style scoped>
.popup {
  
}

.popup .popup-background {
  width: 430px;
  height: 210px;
  background-color: white;
  border: 1px solid #bbb;
  position: absolute;
  left: 43%;
  top: 10%;
  transform: translate(-50%, -50%);
  border-radius: 4px;
  -webkit-box-shadow: 6px 10px 14px 0px rgba(0, 0, 0, 0.7);
  -moz-box-shadow: 6px 10px 14px 0px rgba(0, 0, 0, 0.7);
  box-shadow: 6px 10px 14px 0px rgba(0, 0, 0, 0.7);
}

.popup .popup-background .popup-content {
  background-color: white;
  position: absolute;
  top: 25px;
  left: 2px;
  right: 2px;
  bottom: 2px;
  border-radius: 4px;
  border: 1px solid #fff;
}

.popup .popup-background .popup-content .popup-content-item {
  display: flex;
  justify-content: center;
  margin-top: 24px;
}

.popup .popup-background .popup-content .popup-content-button {
  display: flex;
  height: 60px;
  justify-content: space-around;
  position: absolute;
  bottom: 0px;
  right: 0;
  left: 0;
  background-color: #bbb;
  align-items: center;
}
.popup-invisible {
  display: none;
}
.delete{
  margin-left: 24px;
  font-size: 20px;
  font-weight: bold;
}
.icon{
  height: 46px;
  width: 46px;
  background-image: url("../../../assets/icon/avatar-default.png");
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  border-radius: 23px;
  margin-left: 24px;
  margin-right: 10px;
  flex-shrink: 0;
}
</style>