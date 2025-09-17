<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Ashimmersion and Crescent Dawn's Showcase!</title>
    <meta name="description" content="A-Frame Prototype">
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-effects@^1.0.0/dist/aframe-effects.min.js"></script>
    <script src="https://unpkg.com/aframe-extras@6.1.1/dist/aframe-extras.min.js"></script>
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
      effects__bloom="strength: 3; radius: 1; threshold: 0.6"
      fog="type: linear; color: #ffffff"
>
      <!-- Lighting -->
      <a-light type="ambient" color="#ffffff" intensity="0.2"></a-light>
      <a-light type="directional" color="#ffffff" intensity="0.8" position="5 10 7" castShadow="true"></a-light>
      <a-light type="point" intensity="0.5" position="0 5 0" distance="30"></a-light>
>
      <!-- Sky -->
      <a-sky src="models/Screenshot 2025-03-22 184201.png" rotation="0 -90 -90"></a-sky>
>
      <!-- Ground -->
      <a-plane rotation="-90 0 0" width="50" height="50" color="#a9a9a9" shadow="receive: true"></a-plane>
>      
      <!-- Player Rig (works for Desktop, Mobile & VR) -->
      <a-entity id="player" movement-controls position="0 1.6 4">
        <!-- Head / Camera -->
        <a-entity camera look-controls wasd-controls></a-entity>
>
        <!-- VR Controllers -->
        <a-entity laser-controls="hand: left"></a-entity>
        <a-entity laser-controls="hand: right"></a-entity>
      </a-entity>
>
      <!-- Camera -->
      <a-entity position="0 1.6 4">
        <a-camera wasd-controls-enabled="true" look-controls-enabled="true"></a-camera>
      </a-entity>
>
      <!-- Assets -->
      <a-assets>
        <a-asset-item id="db7" src="models/DB7 MOD1.glb"></a-asset-item>
        <a-asset-item id="ds11nt" src="models/DS-011 NEWTYPE.glb"></a-asset-item>
        <a-asset-item id="dt67" src="models/DT-67 Alpha MOD2.glb"></a-asset-item>
        <a-asset-item id="s1" src="models/sign1.glb"></a-asset-item>
        <a-asset-item id="hypnos" src="models/HYPNOSlower.glb"></a-asset-item>
        <a-asset-item id="arios" src="models/CD-ARIOSF2.glb"></a-asset-item>
        <a-asset-item id="dresst1" src="models/GC-AT-N1.glb"></a-asset-item>
      </a-assets>
>
      <!-- Models -->
      <a-entity gltf-model="#db7" position="-3 0 0" scale="2 2 2"></a-entity>
      <a-entity gltf-model="#ds11nt" position="0 0 0" scale="2 2 2"></a-entity>
      <a-entity gltf-model="#dt67" position="3 0 0" scale="2 2 2"></a-entity>
      <a-entity gltf-model="#s1" position="6 0 0" scale="1 1 1"></a-entity>
      <a-entity gltf-model="#hypnos" position="-6 0 0" scale="2 2 2"></a-entity>
      <a-entity gltf-model="#arios" position="-9 0 0" scale="2 2 2"></a-entity>
      <a-entity gltf-model="#dresst1" position="-12 0 0" scale="2 2 2"></a-entity>
    </a-scene>
  </body>
</html>
