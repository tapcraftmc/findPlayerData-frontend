<script setup lang="ts">
import { ref, reactive } from "vue";
import { ElMessage } from "element-plus";
import { Search } from "@element-plus/icons";
import axios from "axios";
import { API_HOST } from "~/config.js";

defineProps<{ msg: string }>();

const input = ref("");
const playerData = reactive({});

const search = () => {
  if (input.value === "") {
    // input is empty
    ElMessage.warning("Please enter a player name");
    return;
  }
  if (input.value.length < 3) {
    // input is too short
    ElMessage.warning("Please enter at least 3 characters");
    return;
  }
  // 要求输入为英文、数字、下划线：
  if (!/^[a-zA-Z0-9_]*$/.test(input.value)) {
    ElMessage.warning("Plear name only allows '/^[a-zA-Z0-9_]*$/'");
    return;
  }
  // input has a value
  axios.get(`${API_HOST}/findPlayerData/?name=${input.value}`).then((response) => {
    response = response.data;
    if (response.code === 200) {
      ElMessage.success(`Successfully searched ${Object.keys(response.data).length} result with '${input.value}'`);
      playerData.value = response.data;
    } else {
      ElMessage.error(`Error ${response.code}: ${response.message}`);
    }
  });
};
</script>

<template>
  <div class="my-2">
    <el-input class="m-2" v-model="input" size="large" placeholder="Player Name" style="width: 200px" clearable />
    <el-button :icon="Search" size="large" @click="search">搜索</el-button>
  </div>
  <el-row class="row-bg mx-2" justify="center">
    <el-col :xs="24" :sm="12" :md="12" :lg="12" :xl="12">
      <el-col class="py-2" v-for="(player, index) in playerData.value" :key="index">
        <el-descriptions :title="player.lastAccountName" column="1" border>
          <el-descriptions-item label="UUID">{{ index }}</el-descriptions-item>
          <el-descriptions-item label="lastAccountName">{{ player.lastAccountName }}</el-descriptions-item>
          <el-descriptions-item label="logoutlocation" v-if="player.logoutlocation">
            <el-descriptions column="1" border>
              <el-descriptions-item label="world">{{ player.logoutlocation.world }}</el-descriptions-item>
              <el-descriptions-item label="x">{{ Math.round(player.logoutlocation.x) }}</el-descriptions-item>
              <el-descriptions-item label="y">{{ Math.round(player.logoutlocation.y) }}</el-descriptions-item>
              <el-descriptions-item label="z">{{ Math.round(player.logoutlocation.z) }}</el-descriptions-item>
              <el-descriptions-item label="yaw">{{ Math.round(player.logoutlocation.yaw) }}</el-descriptions-item>
              <el-descriptions-item label="pitch">{{ Math.round(player.logoutlocation.pitch) }}</el-descriptions-item>
            </el-descriptions>
          </el-descriptions-item>
          <el-descriptions-item label="homes" v-if="player.homes">
            <el-descriptions class="py-1" column="1" border v-for="(home, homeName) in player.homes" :key="homeName">
              <el-descriptions-item label="homeName">{{ homeName }}</el-descriptions-item>
              <el-descriptions-item label="world">{{ home.world }}</el-descriptions-item>
              <el-descriptions-item label="x">{{ Math.round(home.x) }}</el-descriptions-item>
              <el-descriptions-item label="y">{{ Math.round(home.y) }}</el-descriptions-item>
              <el-descriptions-item label="z">{{ Math.round(home.z) }}</el-descriptions-item>
              <el-descriptions-item label="yaw">{{ Math.round(home.yaw) }}</el-descriptions-item>
              <el-descriptions-item label="pitch">{{ Math.round(home.pitch) }}</el-descriptions-item>
            </el-descriptions>
          </el-descriptions-item>
        </el-descriptions>
      </el-col>
    </el-col>
  </el-row>
</template>