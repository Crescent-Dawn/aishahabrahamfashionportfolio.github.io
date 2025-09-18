<html>
  <head>
    <meta charset="utf-8">
    <title>Ashimmersion and Crescent Dawn's Showcase!</title>
    <meta name="description" content="A-Frame Prototype">
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-effects@^1.0.0/dist/aframe-effects.min.js"></script>
    <script src="https://unpkg.com/aframe-extras@6.1.1/dist/aframe-extras.min.js"></script>
    <script src="https://unpkg.com/aframe-effects@^1.0.0/dist/aframe-effects.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/aframe-environment-component/dist/aframe-environment-component.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/aframe-extras@6.1.1/dist/aframe-extras.min.js"></script>
    <script src="https://unpkg.com/aframe-material-override-component/dist/aframe-material-override-component.min.js"></script>

  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  </head>
  <body>
    <a-scene
      vr-mode-ui="enabled: true"
      webxr="optionalFeatures: hit-test, local-floor; requiredFeatures: local-floor"
      background="color: #001a33"
      shadow="type: pcsoft"
      effects="bloom"
      effects__bloom="strength: 1; radius: 1.5; threshold: 0.5"
      fog="type: linear; color: #ffffff"

>
      <!-- Lighting -->
      <!-- Ambient light (subtle base fill) -->
    <a-light type="ambient" color="#bef0ff" intensity="0.4"></a-light>

>
    <!-- Key light (main source, slightly warm) -->
    <a-light type="directional" color="#FFD6AA" intensity=".5"
             position="5 8 5" castShadow="true" shadow-camera-far="50"></a-light>

>
    <!-- Fill light (cooler, softer opposite side) -->
    <a-light type="directional" color="#88CCFF" intensity="0.2"
             position="-5 4 -3"></a-light>

>
    <!-- Rim/back light (adds glow edge to models) -->
    <a-light type="directional" color="#FFFFFF" intensity="0.8"
             position="0 6 -6"></a-light>

>
    <!-- Subtle point light near models for bloom highlights -->
    <a-light type="point" color="#FF88FF" intensity="0.5"
             distance="15" position="0 3 2"></a-light>

>
      <!-- Sky -->
      <a-sky src="models/360_F_506046531_GHFjciHXWqvrpBGzigN2fEMspKI2d0kp.jpg" rotation="0 0 0"></a-sky>

>
      <!-- Ground -->
      <a-plane rotation="-90 0 0" width="50" height="20" color="#a9a9a9" shadow="receive: true" visible="false"></a-plane>

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
      <!-- HUD text -->
      <a-entity position="0 -0.2 -1.2">
      <a-text
        value="Controls:\nWASD / Joystick = Move\nMouse / Look = Look around\nClick = Hide cursor\nVR Controllers = Point + Move"
        align="center"
        color="#FFF"
        width="2"></a-text>
      </a-entity>
      </a-entity>

>
      <!-- Assets -->
      <a-assets>
        <a-asset-item id="db7" src="models/DB7 MOD1.glb"></a-asset-item>
        <a-asset-item id="ds11nt" src="models/DS-011 NEWTYPE.glb"></a-asset-item>
        <a-asset-item id="dt67" src="models/DT-67 Alpha MOD3.glb"></a-asset-item>
        <a-asset-item id="s1" src="models/simbodysuit3.glb"></a-asset-item>
        <a-asset-item id="" src=""></a-asset-item>
        <a-asset-item id="arios" src="models/models/CD-ARIOSF2.glb"></a-asset-item>
        <a-asset-item id="" src=""></a-asset-item>
        <a-asset-item id="furcoat" src="models/furcoatblend.glb"></a-asset-item>
        <a-asset-item id="fulgor" src="models/FOLGORBT.glb"></a-asset-item>
        <a-asset-item id="map" src="models/mainmap.glb"></a-asset-item>
        <a-asset-item id="greyu" src="models/greyupper.glb"></a-asset-item>
        <a-asset-item id="greyl" src="models/greylower.glb"></a-asset-item>
        <a-asset-item id="whitel" src="models/whitelower.glb"></a-asset-item>
        <a-asset-item id="whiteu" src="models/whiteupper.glb"></a-asset-item>
        <a-asset-item id="Dress1" src="models/dress.glb"></a-asset-item>
        <a-asset-item id="coat" src="models/coat.glb"></a-asset-item>
        <a-asset-item id="lighting" src="models/Lighting.glb"></a-asset-item>
        <a-asset-item id="diamonds" src="models/Flooraccents.glb"></a-asset-item>
        <a-asset-item id="" src=""></a-asset-item>
      </a-assets>

>
      <!-- Models -->
      <a-entity gltf-model="#fulgor" position="-3 0 0" scale="2.25 2.25 2.25" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#ds11nt" position="-6 0 0" scale="2 2 2" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#dt67" position="-9 0 0" scale="2 2 2" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#s1" position="6 0 0" scale="3 3 3" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#" position="12 0 0" scale="2 2 2" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#arios" position="9 0 0" scale="2 2 2" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#" position="0 0 0" scale="2 2 2" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#furcoat" position="3 0 0" scale="3 3 3" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#db7" position="-12 0 0" scale="2 2 2" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#map" position="0 -1 0" scale="1 1 1" rotation="0 0 0" material-override="color: #D3D3D3; metalness: 0.4; roughness: 0.15; sphericalEnvMap: #skyTexture"></a-entity>
      <a-entity gltf-model="#greyu" position="0 0 0" scale="2.25 2.25 2.25" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#greyl" position="0 0 0" scale="2.25 2.25 2.25" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#whitel" position="0 0 -2" scale="2.25 2.25 2.25" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#whiteu" position="0 0 -2" scale="2.25 2.25 2.25" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#Dress1" position="15 0 0" scale="2.25 2.25 2.25" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#coat" position="17 0 0" scale="2.25 2.25 2.25" rotation="0 0 0"></a-entity>
      <a-entity gltf-model="#lighting" position="0 0 0" scale="1 1 1" rotation="0 0 0" material-override="emissive: #AAD8E6; emissiveIntensity: 4">></a-entity>
      <a-entity gltf-model="#diamonds" position="0 0 0" scale="1 1 1" rotation="0 0 0" material-override="color: #ffffff; metalness: 1; roughness: 0"></a-entity> 
    </a-scene>
  </body>
</html>
