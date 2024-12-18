<template>
  <div>
    <div class="top">
      <button class="btn">
        <img src="./assets/menu.svg" />
      </button>
      <div class="mid">
        <input
          id="lookup"
          class="lookup"
          v-model="username"
          type="text"
          placeholder="Type / to search"
        />
        <button id="submitbtn" class="btn" @click="fetchGitHubProfile">
          <img src="./assets/search.svg" />
        </button>
        <div class="li"></div>
        <button class="btn"><img src="./assets/plus.svg"></button>
        <button class="btn"><img src="./assets/issues.svg" /></button>
        <button class="btn"><img src="./assets/pull.svg" /></button>
        <button class="btn"><img src="./assets/inbox.svg" /></button>
      </div>
    </div>

    <div class="grid">
      <div class="column0">
        <div class="div">
          <h2>Top repositories</h2>
          <input
            class="lookup"
            type="text"
            placeholder="Find a repository..."
            disabled
          />
          <ul>
            <li>lnlnlnnln/</li>
            <li>lnlnlnnln/</li>
            <li>lnlnlnnln/</li>
            <li>lnlnlnnln/</li>
          </ul>
        </div>
      </div>

      <div class="column1">
        <div class="div">
          <h1>Home</h1>
          <div id="profileDiv" class="profileDiv">
            <template v-if="profile">
              <div class="div0">
                <img :src="profile.avatar_url" alt="Profile Image" />
                <h>{{ profile.name || "No name provided" }}</h>
              </div>
              <br />
              <div class="div1">
                <p>User: {{ profile.login }}</p>
                <p>Followers: {{ profile.followers }}</p>
                <p>Public Repositories: {{ profile.public_repos }}</p>
                <a :href="profile.html_url" target="_blank">
                  <button class="linkbtn">Visit Profile</button>
                </a>
              </div>
            </template>
            <template v-else-if="error">
              <p>{{ error }}</p>
            </template>
          </div>
        </div>
      </div>
    </div>

    <div class="bottom"></div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import fetch from "node-fetch";

const username = ref<string>("");
const profile = ref<null | Record<string, any>>(null);
const error = ref<string | null>(null);

const fetchGitHubProfile = async () => {
  profile.value = null;
  error.value = null;

  try {
    const response = await fetch(`https://api.github.com/users/${username.value}`);
    if (!response.ok) throw new Error("Profile not found");

    profile.value = await response.json();
  } catch (err) {
    error.value = (err as Error).message;
  }
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  font-family: system-ui;
  color: #59656f;
  box-sizing: border-box;
}

/* ---------- #TOP ---------- */

.top {
  display: flex;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 10%;
  font-size: 14px;
  border: 1px solid #d6ddd4;
  background: #f6f8fa;
  padding: 10px;
}

.btn {
  display: flex;
  align-items: center;
  width: 29px;
  height: 29px;
  border: 1px solid #d6ddd4;
  border-radius: 4px;
  background: #f6f8fa;
  margin-left: 10px;
}

.top img {
  width: 80%;
  height: 80%;
  filter: invert(40%) sepia(8%) saturate(767%) hue-rotate(165deg)
    brightness(90%) contrast(84%);
  stroke-width: 1%;
  margin: 0 auto;
}

.mid {
  display: flex;
  align-items: center;
  position: fixed;
  right: 20px;
  display: flex;
  height: 24px;
  line-height: 24px;
  margin: 0 auto;
  margin-top: auto;
  margin-bottom: auto;
}

.lookup {
  width: 220px;
  height: 120%;
  border: 1px solid #d6ddd4;
  border-radius: 4px;
  background: #f6f8fa;
  padding-left: 5px;
}

input::placeholder {
  color: #59656f;
}

.li {
  height: 20px;
  border-left: 1px solid #d6ddd4;
  margin-left: 10px;
}

/* ---------- #BOTTOM ---------- */

.bottom {
  position: fixed;
  left: 0;
  bottom: 0;
  height: 10%;
}

/* ---------- #GRID ---------- */

.grid {
  display: grid;
  grid-template-columns: 2fr 6fr;
  width: 100vw;
  height: 100vh;
  gap: 10px;
  margin-top: 10vh;
}

h2 {
  font-size: 12px;
}

.column0 {
  display: flex;
  border-right: 1px solid #d6ddd4;
  padding: 0 20px;
}

.column0 .lookup {
  width: 90%;
  height: 29px;
  line-height: 24px;
  border: 1px solid #d6ddd4;
  border-radius: 4px;
  background: #f6f8fa;
  margin-top: 10px;
  padding-left: 4.9px;
}

.div {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100px;
  margin: 0 auto;
  margin-top: 40px;
}

ul {
  list-style-type: none;
  font-size: 12px;
}

li {
  margin-top: 10px;
  margin-left: 4px;
}

.profileDiv {
  position: relative;
  width: 90%;
  font-size: 12px;
  border: 1px solid #d6ddd4;
  border-radius: 4px;
  margin-top: 10px;
  margin-left: 40px;
  padding: 10px;
  padding-top: 20px;
}

.profileDiv img {
  width: 49px;
  height: 122.9%;
  border: none;
  border-radius: 10px;
  margin-left: 10px;
}

.div0 {
  display: flex;
  height: 20%;
  background: #ffffff;
}

.div1 {
  border: 1px solid #f6f8fa;
  border-radius: 2px;
  background: #f6f8fa;
  margin-top: 20px;
}

.linkbtn {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 70px;
  height: 29px;
  border: 1px solid #d6ddd4;
  border-radius: 4px;
  background: #f6f8fa;
  color: #59656f;
}

h1 {
  margin-left: 40px;
}

h {
  margin: 10px;
  margin-top: 14.9px;
  margin-bottom: auto;
}

p {
  margin: 10px;
}

a {
  text-decoration: none;
}
</style>
