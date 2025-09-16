<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Ashimmersion and Crescent Dawn's Showcase</title>
    <meta name="description" content="WebXR with A-Frame">
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-effects@^1.0.0/dist/aframe-effects.min.js"></script>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  </head>
  <body>
    <a-scene
      vr-mode-ui="enabled: true"
      webxr="optionalFeatures: hit-test, local-floor; requiredFeatures: local-floor"
      background="color: #001a33"
      shadow="type: pcsoft"
      effects="bloom"
      effects__bloom="strength: 1.5; radius: 0.8; threshold: 0.2"
    >
      <!-- Assets -->
      <a-assets>
        <a-asset-item id="db7" src="models/DB7 MOD1.glb"></a-asset-item>
        <a-asset-item id="ds11nt" src="models/DS-011 NEWTYPE.glb"></a-asset-item>
        <a-asset-item id="dt67" src="models/DT-67 Alpha MOD2.glb"></a-asset-item>
      </a-assets>
>
      <!-- Lighting -->
      <a-light type="ambient" color="#FF8C61" intensity="0.4"></a-light>
      <a-light type="directional" color="#ffffff" intensity="0.8" position="5 10 7" castShadow="true"></a-light>
      <a-light type="point" intensity="0.5" position="0 5 0" distance="30"></a-light>
>
      <!-- Sky -->
      <a-sky color="#985277"></a-sky>
>
      <!-- Ground -->
      <a-plane rotation="-90 0 0" width="100" height="100" color="#a9a9a9" shadow="receive: true"></a-plane>
>
      <!-- Camera -->
      <a-entity position="0 1.6 4">
        <a-camera wasd-controls-enabled="true" look-controls-enabled="true"></a-camera>
      </a-entity>
>
      <!-- Models -->
      <a-entity gltf-model="#db7" position="-1 0 0" scale="2 2 2"></a-entity>
      <a-entity gltf-model="#ds11nt" position="0 0 0" scale="2 2 2"></a-entity>
      <a-entity gltf-model="#dt67" position="1 0 0" scale="2 2 2"></a-entity>
    </a-scene>
  </body>
</html>
