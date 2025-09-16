<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My WebXR Scene</title>
    <meta name="description" content="WebXR with A-Frame">
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  </head>
  <body>
    <a-scene
      vr-mode-ui="enabled: true"
      webxr="optionalFeatures: hit-test, local-floor; requiredFeatures: local-floor"
      background="color: #0000fff"
    >
      <!-- Assets -->
      <a-assets>
        <a-asset-item id="model" src="replacemewiththestadium"></a-asset-item>
      </a-assets>
    >
      <!-- Lighting -->
      <a-light type="ambient" intensity=".25"></a-light>
      <a-light type="directional" intensity="0.5" position="0 10 0"></a-light>
    >
      <!-- Ground -->
      <a-plane rotation="-90 0 0" width="100" height="100" color="#a9a9a9"></a-plane>
    >
      <!-- Camera -->
      <a-entity position="0 1.6 4">
        <a-camera wasd-controls-enabled="true" look-controls-enabled="true"></a-camera>
      </a-entity>
    >
      <!-- Model -->
      <a-entity gltf-model="models/HYPNOSlower.glb" position="0 0 0" scale="2 2 2"></a-entity>
    </a-scene>
    >
      <!-- Model -->
      <a-entity gltf-model="models/DT-67 Alpha MOD2.glb" position="2 0 0" scale="2 2 2"></a-entity>
    </a-scene>
  </body>
</html>
