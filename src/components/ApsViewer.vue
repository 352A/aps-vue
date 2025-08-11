<template>
  <div>
    <div ref="viewerRef" style="width: 100%; height: 100vh"></div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const URN =
  "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6eWt2N2lzd2NyNzNiZ21qZTVrd3p1eXAwbHAzODduYnphN3YzYzNqYTZheHBnMGFsLWJhc2ljLWFwcC9idWlsZGluZ18wNC5mYng";

const viewerRef = ref(null);

const API_URL = import.meta.env.VITE_API_URL || "http://localhost:8080";

onMounted(async () => {
  try {
    const response = await fetch(`${API_URL}/api/auth/token`);
    const { access_token } = await response.json();

    const options = {
      env: "AutodeskProduction",
      accessToken: access_token,
    };

    const config = {
      extensions: [
        "Autodesk.Viewing.MarkupsCore",
        "Autodesk.Viewing.MarkupsGui",
      ],
    };

    Autodesk.Viewing.Initializer(options, () => {
      const viewer = new Autodesk.Viewing.GuiViewer3D(viewerRef.value, config);
      viewer.start();

      Autodesk.Viewing.Document.load(
        `urn:${URN}`,
        (doc) => {
          const defaultModel = doc.getRoot().getDefaultGeometry();
          viewer.loadDocumentNode(doc, defaultModel);
        },
        (err) => {
          console.error("Document load error:", err);
        }
      );
    });
  } catch (error) {
    console.error("Error initializing Autodesk Viewer:", error);
  }
});
</script>
