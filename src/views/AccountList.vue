<template>
  <div id="AccountList">
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-user-solid"></i> 待审核用户列表
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <!--主体部分-->
    <div id="body">
      <el-row :gutter="20">
        <el-col :span="3">
          <el-select
            v-model="form.username"
            @change="selectChange"
            placeholder="请选择"
          >
            <el-option
              v-for="item in list"
              :key="item.username"
              :label="item.username"
              :value="item.username"
            >
            </el-option>
          </el-select>
        </el-col>
        <el-button
          type="success"
          icon="el-icon-check"
          circle
          @click="onSubmit"
        ></el-button>
      </el-row>
    </div>
    <!--页脚部分留白-->
    <!-- <div id="footer"></div> -->
  </div>
</template>

<script>
import { BACKEND } from "../utils/backend";
import request from "../utils/request";
export default {
  data() {
    return {
      currentDate: new Date(),
      form: {
        username: "",
      },
      list: {
        username: "",
      },
    };
  },
  created: function () {
    // if(this.$cookies.get("uuid") == null){
    // this.$message.error("您还未登录呢，快去登录吧");
    // this.$router.push("/");
    // }
    this.getList();
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },

    // hrefExit() {
    //   this.$cookies.remove("uuid");
    //   this.$router.push({ path: "/userservice/account" });
    // },

    // hrefReturn() {
    //   this.$router.push({ path: "/userservice/account" });
    // },
    onSubmit() {
      request({
        url:
          BACKEND +
          "/userservice/account/verify?username=" +
          this.form.username,
        method: "post",
      });
    },
    getList() {
      var that = this;
      request(
        {
          url: BACKEND + "/dataservice/account/accountList",
          method: "get",
        },
        function (error, response, body) {
          if (!error && response.statusCode === 200) {
            let data = JSON.parse(body);
            if (data.success) {
              that.list = data.data.list;
            }
          }
        }
      );
    },
  },
};
</script>
 
<style scoped>
.option-card {
  height: 250px;
  width: 230px;
}
#title {
  margin-top: 30px;
}

#upload {
  margin-top: 100px;
}

#main,
#body {
  position: absolute;
  top: 50px;
  left: 5%;
  right: 5%;
}

.hrefButton {
  position: absolute;
  right: 10px;
  bottom: 0px;
}

#body {
  position: absolute;
  top: 100px;
}

#header {
  position: absolute;
  right: 0px;
  left: 0px;
  top: 0px;
  height: 100px;
  background: url("../assets/img/北邮logo.png") no-repeat;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
}
</style>