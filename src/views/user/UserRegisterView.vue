<template>
  <div id="userRegisterView">
    <h2 style="margin-bottom: 16px">用户注册</h2>
    <a-form
      style="max-width: 480px; margin: 0 auto"
      label-align="left"
      auto-label-width
      :model="form"
    >
      <a-form-item field="userAccount" label="账号">
        <a-input v-model="form.userAccount" placeholder="请输入账号" />
      </a-form-item>
      <a-form-item field="userPassword" tooltip="密码不少于 8 位" label="密码">
        <a-input-password
          v-model="form.userPassword"
          placeholder="请输入密码"
        />
      </a-form-item>
      <a-form-item
        field="checkPassword"
        tooltip="请再次输入密码"
        label="确认密码"
      >
        <a-input-password
          v-model="form.checkPassword"
          placeholder="请再次输入密码"
        />
      </a-form-item>
      <a-form-item>
        <a-button type="primary" @click="handleSubmit" style="width: 120px">
          注册
        </a-button>
      </a-form-item>
      <a-form-item>
        <span>已有账号？</span>
        <router-link to="/user/login" style="color: #42b983"
          >去登录
        </router-link>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
import { UserControllerService, UserRegisterRequest } from "../../../generated";
import message from "@arco-design/web-vue/es/message";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import axios from "axios";

/**
 * 表单信息
 */
const form = reactive({
  userAccount: "",
  checkPassword: "", // 保留 confirmPassword 属性
  userPassword: "",
});

const router = useRouter();

/**
 * 提交表单
 * @param event
 */
const handleSubmit = async (event: Event) => {
  event.preventDefault(); // 阻止默认的表单提交行为

  if (form.userPassword !== form.checkPassword) {
    message.error("两次输入的密码不一致");
    return;
  }

  const registerRequest: UserRegisterRequest = {
    userAccount: form.userAccount,
    userPassword: form.userPassword,
  };

  try {
    const res = await UserControllerService.userRegisterUsingPost(form);

    // 注册成功，跳转到登录页面
    if (res.data.code === 0) {
      // await store.dispatch("user/getLoginUser");
      message.success("注册成功，请登录");
      router.push({
        path: "/user/login",
        replace: true,
      });
    } else {
      message.error("注册失败，" + res.data.message);
    }
  } catch (error) {
    message.error("请求失败，请稍后再试");
  }
};
</script>

<style scoped lang="less"></style>
