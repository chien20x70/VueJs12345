<template>
  <div class="header">
    <div class="header-left">
      <select>
        <option>Nhà hàng Biển Đông&ensp;</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
      </select>
    </div>
    <div class="header-right">
      <div class="header-right-icon icon-avatar"></div>
      <div class="header-right-text">Nguyễn Xuân Chiến</div>
      <div class="header-right-icon icon-option"></div>
    </div>
  </div>
</template>

<style scoped>
.header {
  position: absolute;
  top: 0;
  left: 226px;
  height: 48px;
  right: 0px;
  border-bottom: 1px solid #bbb;
  display: flex;
}

.header .header-left {
  display: flex;
  align-items: center;
  height: 48px;
}

.header .header-left select {
  margin-left: 24px;
  font-weight: bold;
  color: black;
  border: none;
}

.header .header-right {
  position: absolute;
  display: flex;
  right: 16px;
  align-items: center;
  height: 48px;
  width: auto;
}

.header .header-right .header-right-icon {
  width: 20px;
  height: 20px;
  border-radius: 10px;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}

.header .header-right .header-right-text {
  margin-left: 10px;
  color: black;
}

.icon-avatar {
  background-image: url("../../assets/icon/avatar-default.png");
}
.icon-option {
  margin-left: 10px;
  background-image: url("../../assets/icon/option.png");
}
</style>