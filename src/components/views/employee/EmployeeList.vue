<template>
  <div class="content">
    <div class="content-item">
      <div class="content-item-text">Danh sách nhân viên</div>
      <button class="btn-add" @click="btnAddClick">Thêm nhân viên</button>
    </div>
    <div class="content-search">
      <div class="content-search-item">
        <input type="text" class="input-search" placeholder="Tìm kiếm theo mã, tên nhân viên"/>
        <select class="select-margin" @click="handleClickDepartment($event.target.value)">
          <option value="">Tất cả phòng ban</option>
          <option value="469b3ece-744a-45d5-957d-e8c757976496">Phòng Nhân sự</option>
          <option value="17120d02-6ab5-3e43-18cb-66948daf6128">Phòng Đào tạo</option>
          <option value="142cb08f-7c31-21fa-8e90-67245e8b283e">Phòng Marketing</option>
        </select>
        <select class="select-margin" @click="handleClickPosition($event.target.value)">
          <option value="">Tất cả vị trí</option>
          <option value="3700cc49-55b5-69ea-4929-a2925c0f334d">Giám đốc</option>
          <option value="148ed882-32b8-218e-9c20-39c2f00615e8">Nhân viên Marketing</option>
          <option value="25c6c36e-1668-7d10-6e09-bf1378b8dc91">Thu ngân</option>
        </select>
      </div>
      <div class="search-item">
        <button class="btn-delete" :class="{'btn-invisible': valuebtnDelete}" @click="btnDeleteClick">Xóa</button>
        <button class="btn-refresh"></button>
      </div>
    </div>
    <div class="content-table">
        <table class="tblListEmployee" border="0" width="100%">
            <thead>
                <tr>
                    <th>Mã nhân viên</th>
                    <th>Họ và tên</th>
                    <th>Giới tính</th>
                    <th>Ngày sinh</th> 
                    <th>Điện thoại</th>
                    <th>Email</th>
                    <th>Chức vụ</th>
                    <th>Phòng ban</th>
                    <th>Mức lương cơ bản</th>
                    <th>Tình trạng công việc</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in employees" :key="employee.EmployeeId" 
                :class="{'onColor': (employee.EmployeeId == recordId)}"
                @click="rowTableClick(employee.EmployeeId, employee.EmployeeCode)"
                @dblclick="dblClickTable(employee.EmployeeId)"
                >
                    <td>{{employee.EmployeeCode}}</td>
                    <td>{{employee.FullName}}</td>
                    <td>{{employee.Gender | showGender}}</td>
                    <td>{{employee.DateOfBirth | dateFormatDDMMYY}}</td>
                    <td>{{employee.PhoneNumber}}</td>
                    <td>{{employee.Email}}</td>
                    <td>{{employee.PositionId | showPosition}}</td>
                    <td>{{employee.DepartmentId | showDepartment}}</td>
                    <td>{{employee.Salary | formatMoney}}</td>
                    <td>{{employee.WorkStatus | showWorkStatus}}</td>
                </tr>
            </tbody>
        </table>
        <div class="content-navpage">
            <div class="content-navpage-text-left">Hiển thị 1-10/1000 Khách hàng</div>
            <div class="content-navpage-button">
                <button class="btn-navpages btn-first"></button>
                <button class="btn-navpages btn-prev"></button>
                <button class="btn-numb" @click="loadPage(1)">1</button>
                <button class="btn-numb" @click="loadPage(2)">2</button>
                <button class="btn-numb" @click="loadPage(3)">3</button>
                <button class="btn-numb" @click="loadPage(4)">4</button>
                <button class="btn-navpages btn-next"></button>
                <button class="btn-navpages btn-last"></button>
            </div>
            <div class="content-navpage-text-right right">10 Khách hàng/trang</div>
        </div>
    </div>
    <EmployeeDialog :state="isShow" @hideDialog = "hideDialog" :employee="selectedEmployee" :flag="value"/>
    <Popup :popupState="valuepopup" :employeeCode="recordCode" @hidePopup = "hidePopup" :employeeId="recordId"/>
  </div>
</template>

<script>
import EmployeeDialog from './EmployeeDialog.vue'
import Popup from './Popup.vue'
import axios from 'axios'
export default {
  components:{
    EmployeeDialog,
    Popup,
  },
  data() {
    return {
      employees: [],          // mảng nhân viên 
      isShow: true,           // Giá trị để hiển thị dialog
      recordId: null,         // Giá trị của employeeId khi ClickTable  
      recordCode: null,       // Giá trị của employeeCode khi ClickTable  
      selectedEmployee: {},   //Object employee khi dblClick
      valuepopup: true,       // Giá trị để hiển thị popup
      valuebtnDelete: true,   // Giá trị để hiển thị nút xóa
      value: null,            // Giá trị cờ để kiểm tra nút sửa hay là nút thêm mới  
      departmentId: '',       //Giá trị id phòng ban
      positionId: '',         // Giá trị id vị trí
    }
  },
  created() {
    axios.get('http://api.manhnv.net/v1/Employees').then(res =>{
      this.employees = res.data;
      console.log(res);
    }).catch(res =>{
      console.log(res);
      alert('getData that bai');
    }),

    //Kiểm tra click ngoài phạm vi nút xóa thì ẩn nút
    window.addEventListener('click', (e) => {
      if (!this.$el.contains(e.target)){
        this.valuebtnDelete = true;
        this.recordId = null;  // Xoa class onColor sau khi click ra ngoài phạm vi content
      }
    })
  },
  methods:{
    // Xử lý lọc Employee theo Phòng ban, vị trí--------------------------//
    // Lọc trên data hiện tại do API thiếu ----------------------------//
    filteredDepartment(){
      return this.employees.filter((employee) =>{       
        if(employee.DepartmentId == this.departmentId)
          return employee;
      }) 
    },
    handleClickDepartment(ev){      
      this.departmentId = ev;
      if(this.departmentId == ""){
        this.loadData();
      }else{
        // this.loadData();
        this.employees = this.filteredDepartment();
      }
    },
    filteredPosition(){
      return this.employees.filter((employee) =>{      
        if(employee.PositionId == this.positionId)
          return employee;
      }) 
    },
    handleClickPosition(ev){     
      this.positionId = ev;
      if(this.positionId == ""){
        this.loadData();
      }else{
        // this.loadData();
        this.employees = this.filteredPosition();
      }
    },
    //---------------------------------------------------------------------//

    // Phân trang------------ Lỗi----------------------------------//
    loadPage(value){           //Phân trang có lỗi
      axios.get("http://api.manhnv.net/v1/Employees/Filter?pageNumber="+ value).then(response =>{
        if(response.data){
          console.log(response);
          this.employees = response.data;  
        }else{
          this.employees = [];
        }
      }).catch(response =>{
        console.log(response);
    })
    },
    //---------------------------------------------------------------------//  
    
    // loadData từ API-----------------------------------------------//
    loadData(){
      axios.get("http://api.manhnv.net/v1/Employees").then(response =>{
        console.log(response);
        this.employees = response.data;      
      }).catch(response =>{
        console.log(response);
    })
    },

    //Mở dialog bằng btnAdd-----------------------------------------//
    btnAddClick(){
      this.isShow = false;
      this.value = 'add';
      // this.selectedEmployee = {};

      axios.get('http://api.manhnv.net/v1/Employees/NewEmployeeCode').then(response =>{
       console.log(response.data);
        var increCode = response.data;    
        increCode = increCode.substring(3);
        this.selectedEmployee.EmployeeCode = 'NV-' + (Number(increCode) + 1);
      }).catch(response =>{
        console.log(response);
    })
      
      // hàm created gán data vào mảng employees
      //Từ mảng lấy ra phần tử cuối cùng với employeeCode
      // var increCode = Number(this.employees[0].EmployeeCode.substring(3)); //this.employees.length - 1
      // this.selectedEmployee.EmployeeCode = 'NV-' + (increCode + 1);
      // console.log(increCode);
      //Thực hiện fomat mã nhân viên và trả về 1 giá trị mã NV max
    },

    //Đóng dialog bằng nút đóng bên Dialog-------------------------//
    hideDialog(){
      this.isShow = true;
      this.loadData();
    },

    // dblClick vào 1 dòng của bảng để lấy dữ liệu, sau đó fill vào dialog
    dblClickTable(id){
      axios.get("http://api.manhnv.net/v1/Employees/"+id).then(response =>{
        console.log(response.data.Gender);
        this.selectedEmployee = response.data;
        //---------Format data để binding vào dialog-------------------//
        this.selectedEmployee.DateOfBirth = this.dateFormatYYMMDD(response.data.DateOfBirth);
        this.selectedEmployee.IdentityDate = this.dateFormatYYMMDD(response.data.IdentityDate);
        this.selectedEmployee.JoinDate = this.dateFormatYYMMDD(response.data.JoinDate);
        // this.selectedEmployee.Salary = this.formatMoney(response.data.Salary) ;
        if(this.selectedEmployee.Salary == null){
          this.selectedEmployee.Salary = 0;
        }
        //-------------------------------------------------------------//
      }).catch(response =>{
        console.log(response);
      })
      this.value = 'edit';  // gán cờ để kiếm tra nút lưu 
      this.isShow = false;  // Mở dialog
    },
    
    //Click vào 1 dòng của table lấy id, và hiện nút xóa. gán id và code lấy được vào record
    rowTableClick(id, code){
      this.recordId = id;             // Lấy id khi click vào 1 dòng
      this.recordCode = code;         // Lấy luôn cả code để truyền sang popup
      this.valuebtnDelete = false;    // Hiển thị nút xóa khi bấm vào 1 dòng
    },

    // khi click nút xóa thì hiện popup, sau đó lấy mã nhân viên đưa vào 1 biến lưu trong data.
    btnDeleteClick(){
      this.valuepopup = false;        // Hiển thị dialog khi click nút xóa
    },
    
    // Ẩn popup.
    hidePopup(){
      this.valuepopup = true;
      this.loadData();
    },


    // formatDate để truyền cào dialog--------------------//
    dateFormatYYMMDD(date){
      var newDate = new Date(date);
      var day = newDate.getDate();
      var month = newDate.getMonth() + 1;
      var year = newDate.getFullYear();
      day = (day < 10) ? ('0' + day) : day;
      month = (month < 10) ? ('0' + month) : month;
      return `${year}-${month}-${day}`;
    },
    //----------------------------------------------------//
  },
  
  // data từ Api đổ về đi qua filter để hiển thị bên phía client. giá trị thật ko bị thay đổi.
  filters:{
    dateFormatDDMMYY(date){
      var newDate = new Date(date);
      var day = newDate.getDate();
      var month = newDate.getMonth() + 1;
      var year = newDate.getFullYear();
      day = (day < 10) ? ('0' + day) : day;
      month = (month < 10) ? ('0' + month) : month;
      return `${day}/${month}/${year}`;
    },
    formatMoney(money){
      if(money == null){
        return 0;
      }else{
        let val = (money).toFixed(0).replace('.', ',');
        return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
      } 
    },
    showDepartment(value){
      if(value == "469b3ece-744a-45d5-957d-e8c757976496"){
        return 'Phòng Nhân sự';
      }else if(value == "17120d02-6ab5-3e43-18cb-66948daf6128"){
        return 'Phòng Đào tạo';
      }else if(value == "142cb08f-7c31-21fa-8e90-67245e8b283e"){
        return 'Phòng Marketing';
      }
    },
    showPosition(value){
      if(value == "3700cc49-55b5-69ea-4929-a2925c0f334d"){
        return 'Giám đốc';
      }else if(value == "148ed882-32b8-218e-9c20-39c2f00615e8"){
        return 'Nhân viên Marketing';
      }else if(value == "25c6c36e-1668-7d10-6e09-bf1378b8dc91"){
        return 'Thu ngân';
      }
    },
    showWorkStatus(value){
      if(value == "1"){
        return 'Đang làm việc';
      }else if(value == "0"){
        return 'Đang thử việc';
      }else if(value == "3"){
        return 'Đã nghỉ hưu';
      }else if(value == "2"){
        return 'Đã nghỉ việc';
      }
    },
    showGender(value){
      if(value == "1"){
        return 'Nam';
      }else if(value == "0"){
        return 'Nữ';
      }else if(value == "2"){
        return 'Không xác định';
      }
    } 
  }
}
</script>
<style scoped>
.content .content-item {
  height: 40px;
  align-items: center;
  display: flex;
}
.content .content-item .content-item-text {
  font-size: 27px;
  color: black;
  background-size: contain;
  background-position: center;
  font-weight: bold;
}
.content .content-search {
  height: 40px;
  align-items: center;
  display: flex;
  margin-top: 10px;
  margin-bottom: 10px;
}
.content-search .content-search-item{
  display: flex;
  align-items: center;
}
.content-search .search-item{
  position: absolute;
  right: 0;
  top: 66px;
  width: 150px;
}
.content-table{
    height: calc(100% - 146px);
    overflow-y: auto;
}
.content .content-navpage {
    position: absolute;
    height: 50px;
    bottom: 0;
    right: 0;
    left: 16px;
    border-top: 6px solid #a29d9d;
    align-items: center;
    display: flex;
    justify-content: space-between;
    }

.content .content-navpage .content-navpage-text-left {
    margin-left: 10px;
            
}
.content .content-navpage .content-navpage-button {
    display: flex;   
}
.content .content-navpage .content-navpage-text-right {
    margin-right: 10px;
}
.select-margin{
  margin-left: 10px;
}
.onColor{
  background-color: #bbb;
}
</style>