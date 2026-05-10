<script setup lang="ts">
import { open } from "@tauri-apps/plugin-shell";
import IconMdiPuzzleOutline from "~icons/mdi/puzzle-outline";
import IconMdiDownloadOutline from "~icons/mdi/download-outline";
import IconMdiCursorDefaultClick from "~icons/mdi/cursor-default-click";
import IconMdiShieldCheckOutline from "~icons/mdi/shield-check-outline";
import IconMdiWeb from "~icons/mdi/web";
import IconMdiOpenInNew from "~icons/mdi/open-in-new";
import { useI18n } from "vue-i18n";

const { t } = useI18n();

const REPO_FOLDER_URL = "https://github.com/imsyy/yt-dlp-gui/tree/master/browser-extension";
const REPO_LATEST_URL = "https://github.com/imsyy/yt-dlp-gui/releases/latest";

const SUPPORTED_SITES = [
  "YouTube", "Bilibili", "Twitch", "Vimeo", "Dailymotion", "Niconico",
  "Twitter / X", "Instagram", "TikTok", "Facebook", "Reddit",
  "SoundCloud", "Bandcamp", "Crunchyroll",
];

const sections = [
  {
    icon: IconMdiDownloadOutline,
    color: "#18a058",
    bg: "rgba(24,160,88,0.10)",
    titleKey: "browserExt.installHeading",
    items: [
      "browserExt.install1",
      "browserExt.install2",
      "browserExt.install3",
      "browserExt.install4",
    ],
    ordered: true,
  },
  {
    icon: IconMdiCursorDefaultClick,
    color: "#2080f0",
    bg: "rgba(32,128,240,0.10)",
    titleKey: "browserExt.usageHeading",
    items: [
      "browserExt.usage1",
      "browserExt.usage2",
      "browserExt.usage3",
    ],
    ordered: false,
  },
  {
    icon: IconMdiShieldCheckOutline,
    color: "#f0a020",
    bg: "rgba(240,160,32,0.10)",
    titleKey: "browserExt.privacyHeading",
    items: ["browserExt.privacyNote"],
    ordered: false,
  },
];

const openUrl = (url: string) => open(url).catch(() => {});
</script>

<template>
  <n-flex vertical :size="12">
    <n-card size="small" class="hero">
      <n-flex align="flex-start" :size="14" :wrap="false">
        <div class="hero-icon">
          <n-icon :size="22" color="#ff0033">
            <icon-mdi-puzzle-outline />
          </n-icon>
        </div>
        <n-flex vertical :size="4" style="flex: 1; min-width: 0">
          <n-flex align="center" :size="8">
            <n-text strong style="font-size: 15px">{{ t("browserExt.title") }}</n-text>
            <n-tag size="small" round :bordered="false" type="info">
              {{ t("browserExt.tagBeta") }}
            </n-tag>
          </n-flex>
          <n-text depth="3" style="font-size: 12.5px; line-height: 1.6">
            {{ t("browserExt.intro") }}
          </n-text>
        </n-flex>
      </n-flex>
    </n-card>

    <n-card v-for="(sec, i) in sections" :key="i" size="small">
      <n-flex align="flex-start" :size="12" :wrap="false">
        <div class="sec-icon" :style="{ background: sec.bg }">
          <n-icon :size="18" :color="sec.color">
            <component :is="sec.icon" />
          </n-icon>
        </div>
        <n-flex vertical :size="6" style="flex: 1; min-width: 0">
          <n-text strong>{{ t(sec.titleKey) }}</n-text>
          <component
            :is="sec.ordered ? 'ol' : 'ul'"
            class="sec-list"
            :class="{ 'sec-list-bullet': !sec.ordered }"
          >
            <li v-for="(key, j) in sec.items" :key="j">
              <n-text depth="2" style="font-size: 12.5px; line-height: 1.6">
                {{ t(key) }}
              </n-text>
            </li>
          </component>
        </n-flex>
      </n-flex>
    </n-card>

    <n-card size="small">
      <n-flex align="flex-start" :size="12" :wrap="false">
        <div class="sec-icon" :style="{ background: 'rgba(139,92,246,0.10)' }">
          <n-icon :size="18" color="#8b5cf6">
            <icon-mdi-web />
          </n-icon>
        </div>
        <n-flex vertical :size="6" style="flex: 1; min-width: 0">
          <n-text strong>{{ t("browserExt.supportedHeading") }}</n-text>
          <n-flex :size="6" :wrap="true">
            <n-tag v-for="s in SUPPORTED_SITES" :key="s" size="small" round :bordered="false">
              {{ s }}
            </n-tag>
          </n-flex>
        </n-flex>
      </n-flex>
    </n-card>

    <n-flex :size="8">
      <n-button type="primary" @click="openUrl(REPO_FOLDER_URL)">
        <template #icon>
          <n-icon><icon-mdi-open-in-new /></n-icon>
        </template>
        {{ t("browserExt.openSource") }}
      </n-button>
      <n-button strong secondary @click="openUrl(REPO_LATEST_URL)">
        <template #icon>
          <n-icon><icon-mdi-download-outline /></n-icon>
        </template>
        {{ t("browserExt.openReleases") }}
      </n-button>
    </n-flex>
  </n-flex>
</template>

<style scoped lang="scss">
.hero-icon,
.sec-icon {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  background: rgba(255, 0, 51, 0.08);
}

.sec-list {
  margin: 0;
  padding-left: 18px;

  li {
    margin: 0;
    padding: 2px 0;
  }

  &.sec-list-bullet {
    list-style: disc;
  }
}
</style>
