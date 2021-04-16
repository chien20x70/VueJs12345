<template>
  <div class="dialog" :class="{'dialog-hide': state}">
    <div class="model"></div>
    <div class="dialog-box">
      <div class="dialog-box-title">
        <div class="dialog-box-text">THÔNG TIN NHÂN VIÊN</div>
        <button class="btn-X" @click="btnCloseClick"></button>
      </div>
      <div class="dialog-left">
        <div class="img"></div>
        <p>
          (Vui lòng chọn ảnh có định dạng<br />
          <strong>.jpg, .jpeg, .png, .gif. </strong>)
        </p>
      </div>
      <div class="dialog-content">
        <div class="content-left">
          <div class="content-title">A. THÔNG TIN CHUNG:</div>
          <hr />
          <form class="form-group">
            <label>Mã nhân viên (<p style="color: red; display: inline">*</p>)</label>
            <input type="text" style="width: 190px" v-model="employee.EmployeeCode"/>
            <span style="color: red;"></span>
          </form>
          <form class="form-group">
            <label>Ngày sinh</label><br />
            <input type="date" style="width: 190px" v-model="employee.DateOfBirth"/>
          </form>
          <form class="form-group">
            <label>Số CMTND/Căn cước (<p style="color: red; display: inline">*</p>)</label>
            <input type="text" style="width: 190px" v-model="employee.IdentityNumber" @blur="handleBlurId($event.target.value)"/>
            <span style="color: red;">{{messageId}}</span>
          </form>
          <form class="form-group">
            <label>Nơi cấp</label>
            <input type="text" style="width: 190px" v-model="employee.IdentityPlace"/>
          </form>
          <form class="form-group">
            <label>Email (<p style="color: red; display: inline">*</p>)</label>
            <input type="text" style="width: 190px" v-model="employee.Email" @blur="handleBlurEmail($event.target.value)"/>
            <span style="color: red;">{{messageEmail}}</span>  
          </form>
          <div class="content-title">B. THÔNG TIN CÔNG VIỆC:</div>
          <hr />
          <form class="margin">
            <label>Vị trí</label>
            <select style="width: 224px" v-model="employee.PositionId">
              <option value="3700cc49-55b5-69ea-4929-a2925c0f334d">Giám đốc</option>
              <option value="148ed882-32b8-218e-9c20-39c2f00615e8">Nhân viên Marketing</option>
              <option value="25c6c36e-1668-7d10-6e09-bf1378b8dc91">Thu ngân</option>
            </select>
          </form>
          <form class="margin">
            <label>Mã số thuế cá nhân</label>
            <input type="text" style="width: 190px" v-model="employee.PersonalTaxCode"/>
          </form>
          <form class="margin">
            <label>Ngày gia nhập công ty</label>
            <input type="date" style="width: 190px" v-model="employee.JoinDate"/>
          </form>
        </div>
        <div class="content-right">
          <form class="form-group">
            <label>Họ và tên (<p style="color: red; display: inline">*</p>)</label>
            <input type="text" style="width: 190px" v-model="employee.FullName" @blur="handleBlurName($event.target.value)"/>
            <span style="color: red;">{{messageName}}</span>
          </form>
          <form class="form-group">
            <label>Giới tính</label>
            <select style="width: 224px" v-model="employee.Gender">
              <option value="1">Nam</option>
              <option value="0">Nữ</option>
              <option value="2">Không xác định</option>
            </select>
          </form>
          <form class="form-group">
            <label>Ngày cấp</label>
            <input type="date" style="width: 190px" v-model="employee.IdentityDate"/>
          </form>
          <form class="form-group phone-margin">
            <label>Số điện thoại (<p style="color: red; display: inline">*</p>)</label>
            <input type="number" style="width: 190px" v-model="employee.PhoneNumber" @blur="handleBlurPhoneNumber($event.target.value)"/>
            <span style="color: red;">{{messagePhoneNumber}}</span>
          </form>
          <form class="room-margin">
            <label>Phòng ban</label>
            <select style="width: 224px" v-model="employee.DepartmentId">
              <option value="469b3ece-744a-45d5-957d-e8c757976496">Phòng Nhân sự</option>
              <option value="17120d02-6ab5-3e43-18cb-66948daf6128">Phòng Đào tạo</option>
              <option value="142cb08f-7c31-21fa-8e90-67245e8b283e">Phòng Marketing</option>
            </select>
          </form>
          <form class="margin">
            <label>Mức lương cơ bản</label>
            <input type="text" style="width: 190px" v-model="employee.Salary"/>
          </form>
          <form class="margin">
            <label>Tình trạng công việc</label>
            <select style="width: 224px" v-model="employee.WorkStatus">
              <option value="1">Đang làm việc</option>
              <option value="0">Đang thử việc</option>
              <option value="3">Đã nghỉ hưu</option>
              <option value="2">Đã nghỉ việc</option>
            </select>
          </form>
        </div>
      </div>
      <div class="dialog-footer">
        <button class="btn-save" @click="btnSaveClick">Lưu</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  props:{
    state:{type: Boolean, selector: false},   // Biến trạng thái hiển thị dialog
    employee:{type: Object, selector: null},  // đối tượng employee từ component cha truyền sang.
    flag: {type: String, selector: ''},       // Biến cờ kiểm tra giá trị nút lưu là thêm hay sửa.
  },
  data() {
    return {
      messageEmail: null,         // message validation các trường yêu cầu.
      messageName: null, 
      messagePhoneNumber: null,
      messageId: null,
      // employees: [],
    }
  },
  methods: {
    btnCloseClick(){
      this.$emit('hideDialog');
      this.messageEmail = null;
      this.messageName = null;
      this.messagePhoneNumber = null;
      this.messageId = null;
    },

  //  khi nút thêm được chọn thì EmployeeId ở dạng undefined thì mặc định khi thêm sẽ ko bị trùng ID.
  //  Khi sửa thì mặc định sửa thông tin nhân viên nên Id ko thay đổi. 
    btnSaveClick(){
      if(this.flag == 'add'){
          if(this.messageEmail == '' && this.messageName == '' && this.messagePhoneNumber == '' && this.messageId == ''){
            axios.post('http://api.manhnv.net/v1/Employees', this.employee).then(res =>{
              console.log(res.data);
              alert('Thêm thành công');
              this.$emit('hideDialog');
            }).catch(res =>{
              console.log(res.data);
              alert('Thêm thất bại');
              this.$emit('hideDialog');
            })
          }else{
            alert('Không được để trống các trường bắt buộc!');
          }
      }else if(this.flag == 'edit'){
        axios.put('http://api.manhnv.net/v1/Employees/'+ this.employee.EmployeeId, this.employee).then(res =>{
          console.log(res.data);
          alert('Sửa thành công');
          this.$emit('hideDialog');
        }).catch(res =>{
          console.log(res.data);
          alert('Sửa thất bại');
          this.$emit('hideDialog');
        })
      }
    },

    // Kiểm tra định dạng khi nhập các trường trong dialog---------------------//
    handleBlurEmail(ev){
      if (/^(([^<>()\\.,;:\s@"]+(\.[^<>()\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/.test(ev)) {
        this.messageEmail = '';
      } else if(ev == ''){
        this.messageEmail = "Bắt buộc nhập trường này!";
      }else{
        this.messageEmail = "Email Không đúng định dạng!";
      }
    },
    handleBlurName(ev){
      return this.messageName = (ev == '') ? (this.messageName = 'Bắt buộc nhập trường này!') : (this.messageName = '');
    },
    handleBlurPhoneNumber(ev){
      return this.messagePhoneNumber = (ev == '') ? (this.messagePhoneNumber = 'Bắt buộc nhập trường này!') : (this.messagePhoneNumber = '');
    },
    handleBlurId(ev){
      return this.messageId = (ev == '') ? (this.messageId = 'Bắt buộc nhập trường này!') : (this.messageId = '');
    },
    //-------------------------------------------------------------------------//
  },
}
</script>
<style scoped>
.dialog-hide{
  display: none;
}
.dialog {
  border: 1px solid #bbb;
}

.dialog .model {
  position: absolute;
  top: -48px;
  left: -226px;
  bottom: 0;
  right: -16px;
  background-color: black;
  opacity: 0.4;
}

.dialog .dialog-box {
  border-radius: 4px;
  width: 650px;
  height: 685px;
  position: absolute;
  top: 46%;
  left: 43%;
  transform: translate(-50%, -50%);
  padding: 15px;
  background-color: white;
}

.dialog .dialog-box .dialog-box-title {
  height: 35px;
}
.dialog .dialog-box .dialog-box-title .dialog-box-text {
  font-size: 20px;
  font-weight: bold;
}
.dialog .dialog-left {
  height: 210px;
  width: 160px;
}
.dialog-left .img {
  width: 160px;
  height: 160px;
  background-image: url("../../../assets/img/default-avatar.jpg");
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  border-radius: 90px;
  border: 1px solid #bbb;
}
.dialog-left p {
  text-align: center;
  font-size: 12px;
}
.dialog .dialog-box .dialog-content {
  position: absolute;
  top: 50px;
  bottom: 60px;
  left: 190px;
  right: 22px;
}
.dialog-content .content-title {
  font-weight: bold;
}
.dialog-content hr {
  width: 30%;
  height: 3px;
  background-color: #019160;
  border: none;
  margin-top: 5px;
}
.dialog-content .content-left {
  width: 50%;
  height: 100%;
}
.content-left .form-group{
  height: 70px;
  width: 100%;
}
.margin {
  margin-top: 10px;
}
.dialog-content .content-right{
  position: absolute;
  left: 50%;
  top: calc(100% - 580px);
  bottom: 0;
  right: 0;
}
.content-right .form-group{
  height: 70px;
  width: 100%;
}
.phone-margin{
  margin-top: calc(100% - 164px);
}
.room-margin{
  margin-top: calc(100% - 199px);
}
.dialog-footer{
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60px;
  background-color: #bbb;
  display: flex;
  align-items: center;
}
label{
  margin-bottom: 4px;
}
.form-group{
  height: 76px;
}

</style>