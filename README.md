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
      background="color: #ECECEC"
    >
      <!-- Assets -->
      <a-assets>
        <a-asset-item id="model" src="models/basicfrontmk1.glb"></a-asset-item>
      </a-assets>
    >
      <!-- Lighting -->
      <a-light type="ambient" intensity="0.25"></a-light>
      <a-light type="directional" intensity="0.8" position="0 10 0"></a-light>
    >
      <!-- Ground -->
      <a-plane rotation="-90 0 0" width="100" height="90" color="#ffc000"></a-plane>
    >
      <!-- Camera -->
      <a-entity position="0 1.6 4">
        <a-camera wasd-controls-enabled="true" look-controls-enabled="true"></a-camera>
      </a-entity>
    >
      <!-- Model -->
      <a-entity gltf-model="model" position="0 0 0" scale="1 1 1"></a-entity>
    </a-scene>
  </body>
</html>
