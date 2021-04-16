<template>
    <div class="navbar">
        <div class="navbar-header">
            <div class="toggle"></div>
            <div class="logo"></div>
        </div>
        <div class="navbar-content">
            <div class="navbar-content-icon icon-document"></div>
            <div class="navbar-content-text">Tổng quan</div>
        </div>
        <div class="navbar-content">
            <div class="navbar-content-icon icon-report"></div>
            <div class="navbar-content-text">Báo cáo</div>
        </div>
        <div class="navbar-content">
            <div class="navbar-content-icon icon-dashboard"></div>
            <div class="navbar-content-text">Mua hàng</div>
        </div>
        <router-link class="navbar-content"  to="/employee">
            <div class="navbar-content-icon icon-employee"></div>
            <div class="navbar-content-text">Danh sách nhân viên</div>
        </router-link>
        <div class="navbar-content">
            <div class="navbar-content-icon icon-setting"></div>
            <div class="navbar-content-text">Thiết lập</div>
        </div>
    </div>
</template>
<style scoped>
.navbar{
    width: 226px;
    height: 100vh;
    border-right: 1px solid #bbb;
}
.navbar .navbar-header{
    height: 48px;
    display: flex;
    align-items: center;
}
.navbar .navbar-header .toggle{
    height: 24px;
    width: 24px;
    background-image: url("../../assets/img/toggle.png");
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    flex-shrink: 0;
    margin-left: 16px;
}
.navbar .navbar-header .logo{
    height: 24px;
    width: 100%;
    background-image: url("../../assets/img/cukcuk-logo.png");
    background-size: contain;
    background-repeat: no-repeat;
    background-position: left;
    flex-shrink: 0;
    margin-left: 10px;
}
.navbar .navbar-content{
    height: 44px;
    display: flex;
    align-items: center;
    text-decoration: none;
    color: black;
    cursor: pointer;
}
.navbar-content:hover{
    background-color: #019160;
    color: #ffffff;
}
.navbar .navbar-content .navbar-content-icon{
    height: 20px;
    width: 20px;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    flex-shrink: 0;
    margin-left: 16px;
}
.navbar .navbar-content .navbar-content-text{
    margin-left: 10px;
}
.icon-document {
    background-image: url('../../assets/icon/document-online.png');
}

.icon-report {
    background-image: url('../../assets/icon/report.png');
}

.icon-setting {
    background-image: url('../../assets/icon/setting.png');
}

.icon-dashboard {
    background-image: url('../../assets/icon/dashboard.png');
}

.icon-employee {
    background-image: url('../../assets/icon/dic-employee.png');
}
</style>