<template>
    <div class="w-full h-screen relative mt-5">
        <div id="renderer" class="rend">
        </div>
    </div>
</template>

<script setup lang="ts">
import {
  Viewer,
  DefaultViewerParams,
  SpeckleLoader,
  UrlHelper,
  CameraController,
  SelectionExtension,
} from "@speckle/viewer";
import { onMounted, onUnmounted, ref } from 'vue';

const loading = ref(true);
let viewer: Viewer | null = null;

async function initViewer() {
  const container = document.getElementById("renderer");
  if (!container) return;

  const params = DefaultViewerParams;
  params.showStats = true;
  params.verbose = true;

  viewer = new Viewer(container, params);
  await viewer.init();

  viewer.createExtension(CameraController);
  viewer.createExtension(SelectionExtension);

  const urls = await UrlHelper.getResourceUrls(
    "https://app.speckle.systems/projects/7591c56179/models/32213f5381"
  );
  
  for (const url of urls) {
    const loader = new SpeckleLoader(viewer.getWorldTree(), url, "");
    await viewer.loadObject(loader, true);
  }
}

onMounted(() => {
  initViewer();
});

onUnmounted(() => {
  if (viewer) {
    viewer.dispose();
    viewer = null;
  }
});
</script>

<style scoped>
.rend {
    width:100%;
    height:100%;
    left:0px;
    top:0px;
    position:absolute
}
</style>
