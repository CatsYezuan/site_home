<template>
  <button class="friends-entry" type="button" aria-haspopup="dialog" @click="openFriends">
    <span class="entry-icon" aria-hidden="true">♧</span>
    <span class="entry-copy">
      <strong>友情链接</strong>
      <small>去朋友的小站坐坐</small>
    </span>
    <span class="entry-tail">遇见同好 <span aria-hidden="true">›</span></span>
  </button>

  <Teleport to="body">
    <dialog
      ref="friendsDialog"
      class="friends-dialog"
      aria-labelledby="friends-title"
      @click="closeOnBackdrop"
    >
      <div class="friends-panel">
        <div class="panel-head">
          <div>
            <div class="eyebrow">FRIENDS &amp; NEIGHBORS</div>
            <h2 id="friends-title">友情链接</h2>
          </div>
          <button
            class="close"
            type="button"
            aria-label="关闭友情链接"
            @click="friendsDialog.close()"
          >
            ×
          </button>
        </div>
        <p class="subtitle">互联网很大，很高兴在这里遇见你。</p>
        <p class="section-label">去朋友的小站坐坐</p>
        <div class="friends-list">
          <a
            v-for="friend in friendLinks"
            :key="friend.url"
            class="friend"
            :href="friend.url"
            target="_blank"
            rel="noopener noreferrer"
          >
            <img
              class="avatar"
              :class="{ 'avatar-logo': friend.logo }"
              :src="friend.avatar"
              alt=""
              width="45"
              height="45"
            />
            <div class="friend-copy">
              <h3>{{ friend.name }}</h3>
              <p>{{ friend.description }}</p>
            </div>
            <span class="out" aria-hidden="true">↗</span>
          </a>
        </div>
      </div>
    </dialog>
  </Teleport>
</template>

<script setup>
import { ref } from "vue";
import friendLinks from "@/assets/friendLinks.json";

const friendsDialog = ref(null);
const openFriends = () => friendsDialog.value.showModal();
const closeOnBackdrop = (event) => {
  const dialog = friendsDialog.value;
  if (event.target !== dialog) return;
  const rect = dialog.getBoundingClientRect();
  if (
    event.clientX < rect.left ||
    event.clientX > rect.right ||
    event.clientY < rect.top ||
    event.clientY > rect.bottom
  ) {
    dialog.close();
  }
};
</script>

<style lang="scss" scoped>
.friends-entry {
  width: 100%;
  margin-top: 20px;
  padding: 18px 20px;
  display: flex;
  align-items: center;
  gap: 16px;
  text-align: left;
  font: inherit;
  color: #f3f7f6;
  background: #071d235e;
  border: 1px solid #ffffff0c;
  border-radius: 8px;
  backdrop-filter: blur(12px);
  cursor: pointer;
  transition: background 180ms ease;
  &:hover {
    background: #123d43a8;
  }
  .entry-icon {
    width: 42px;
    height: 42px;
    display: grid;
    place-items: center;
    background: #b3e5de15;
    border-radius: 50%;
    font-size: 24px;
    flex-shrink: 0;
  }
  .entry-copy {
    flex: 1;
    strong {
      display: block;
      font-size: 16px;
    }
    small {
      font-size: 12px;
      color: #d6e7e4b8;
    }
  }
  .entry-tail {
    font-size: 12px;
    color: #d6e7e4bd;
    span {
      margin-left: 8px;
      font-size: 23px;
    }
  }
}
.friends-entry:focus-visible,
.friends-dialog button:focus-visible,
.friend:focus-visible {
  outline: 2px solid #b9f8e6;
  outline-offset: 5px;
}
.friends-dialog {
  margin: auto;
  padding: 0;
  width: min(700px, calc(100% - 32px));
  max-height: calc(100dvh - 48px);
  overflow: auto;
  color: #f3f7f6;
  line-height: 1.6;
  border: 1px solid #ffffff20;
  border-radius: 18px;
  background: #193c40ed;
  backdrop-filter: blur(24px);
  box-shadow: 0 28px 100px #00101466;
  &::backdrop {
    background: #00141580;
    backdrop-filter: blur(5px);
  }
  &[open] {
    animation: friends-enter 220ms cubic-bezier(0.23, 1, 0.32, 1);
  }
  .friends-panel {
    padding: 32px;
  }
  .panel-head {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
  }
  .eyebrow {
    margin-bottom: 7px;
    font-size: 10px;
    letter-spacing: 3px;
    color: #a1c5bf;
  }
  h2 {
    font-size: 25px;
    letter-spacing: 1px;
    font-weight: 600;
  }
  .close {
    width: 34px;
    height: 34px;
    border: 0;
    border-radius: 50%;
    background: #ffffff0c;
    font: inherit;
    font-size: 23px;
    line-height: 1;
    cursor: pointer;
    &:hover {
      background: #ffffff20;
    }
  }
  .subtitle {
    margin: 10px 0 26px;
    font-size: 13px;
    color: #c2d8d5;
  }
  .section-label {
    margin-bottom: 12px;
    font-size: 12px;
    color: #bdd2ce;
  }
  .friends-list {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }
  .friend {
    padding: 18px;
    display: flex;
    align-items: center;
    gap: 13px;
    min-width: 0;
    border: 1px solid #ffffff10;
    border-radius: 10px;
    background: #081e273d;
    text-decoration: none;
    transition:
      background 180ms ease,
      border-color 180ms ease;
    &:hover {
      background: #123d43a8;
      border-color: #ffffff30;
    }
    .avatar {
      width: 45px;
      height: 45px;
      flex-shrink: 0;
      border-radius: 50%;
      object-fit: cover;
      &.avatar-logo {
        background: transparent;
        border-radius: 0;
        object-fit: contain;
      }
    }
    .friend-copy {
      min-width: 0;
      overflow-wrap: anywhere;
    }
    h3 {
      margin-bottom: 4px;
      font-size: 15px;
      font-weight: 500;
    }
    p {
      font-size: 11px;
      color: #b5cbc8;
    }
    .out {
      margin-left: auto;
      color: #8daaa5;
      font-size: 16px;
    }
  }
}
@keyframes friends-enter {
  from {
    opacity: 0;
    transform: translateY(8px) scale(0.98);
  }
  to {
    opacity: 1;
    transform: none;
  }
}
@media (max-width: 720px) {
  .friends-entry {
    margin-top: 12px;
    padding: 12px 16px;
  }
  .friends-dialog .friends-panel {
    padding: 24px 20px;
  }
}
@media (max-width: 600px) {
  .friends-dialog .friends-list {
    grid-template-columns: 1fr;
  }
}
@media (prefers-reduced-motion: reduce) {
  .friends-entry,
  .friends-dialog,
  .friends-dialog * {
    animation: none !important;
    transition: none !important;
  }
}
</style>
