<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Digital Boutique</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/aframe/1.4.0/aframe.min.js"></script>
    
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
        }
        
        .info-panel {
            position: fixed;
            top: 20px;
            left: 20px;
            background: rgba(255, 255, 255, 0.9);
            padding: 15px;
            border-radius: 8px;
            max-width: 300px;
            z-index: 100;
            backdrop-filter: blur(10px);
        }
        
        .vr-button {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #007bff;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 14px;
            z-index: 100;
        }
        
        .vr-button:hover {
            background: #0056b3;
        }

        @media (max-width: 768px) {
            .info-panel {
                position: relative;
                margin: 10px;
            }
        }
    </style>
</head>
<body>
    <!-- Simple Info Panel -->
    <div class="info-panel">
        <h3> Digital Boutique</h3>
        <p><strong>How to navigate:</strong></p>
        <p>• Desktop: Click & drag to look, WASD to move</p>
        <p>• Mobile: Touch & drag to look around</p>
        <p>• VR: Use your headset controllers</p>
    </div>

    <!-- VR Entry Button -->
    <button class="vr-button" onclick="enterVR()">Enter VR</button>

    <!-- A-Frame Scene -->
    <a-scene 
        vr-mode-ui="enabled: true" 
        background="color: #87CEEB"
        embedded>
        
        <!-- Assets - This is where you'll add your 3D models -->
        <a-assets>
            <!-- YOUR ENVIRONMENT MODEL -->
            <!-- Replace "models/your-map.gltf" with the path to your map file -->
            <a-asset-item id="environment" src="models/your-map.gltf"></a-asset-item>
            
            <!-- YOUR CLOTHING MODELS -->
            <!-- Add as many as you need, just copy this pattern -->
            <a-asset-item id="clothing1" src="models/clothing-item-1.gltf"></a-asset-item>
            <a-asset-item id="clothing2" src="models/clothing-item-2.gltf"></a-asset-item>
            <a-asset-item id="clothing3" src="models/clothing-item-3.gltf"></a-asset-item>
            <!-- Add more clothing items here as needed -->
        </a-assets>

        <!-- Basic Lighting (adjust as needed) -->
        <a-light type="ambient" color="#404040" intensity="0.4"></a-light>
        <a-light type="directional" position="2 4 2" color="#ffffff" intensity="0.8"></a-light>
        <a-light type="point" position="-2 2 2" color="#4A90E2" intensity="0.3"></a-light>

        <!-- Your Environment Model -->
        <!-- This loads your custom map -->
        <a-gltf-model 
            src=https://github.com/Crescent-Dawn/test2.github.io/blob/main/Assets/gltf/Basic%20Boutique%20MK1.glb
            position="0 0 0" 
            scale="1 1 1">
        </a-gltf-model>

        <!-- Your Clothing Models -->
        <!-- Position these where you want them in your space -->
        <!-- Adjust position values based on your map layout -->
        
        <a-gltf-model 
            src="#clothing1" 
            position="-2 0 -1" 
            scale="1 1 1"
            class="clothing-item"
            animation="property: rotation; to: 0 360 0; dur: 20000; loop: true; easing: linear">
        </a-gltf-model>
        
        <a-gltf-model 
            src="#clothing2" 
            position="0 0 -1" 
            scale="1 1 1"
            class="clothing-item"
            animation="property: rotation; to: 0 360 0; dur: 18000; loop: true; easing: linear">
        </a-gltf-model>
        
        <a-gltf-model 
            src="#clothing3" 
            position="2 0 -1" 
            scale="1 1 1"
            class="clothing-item"
            animation="property: rotation; to: 0 360 0; dur: 22000; loop: true; easing: linear">
        </a-gltf-model>

        <!-- Add more clothing models here - just copy the pattern above -->
        
        <!-- Camera with Movement Controls -->
        <a-entity id="cameraRig" position="0 1.6 3">
            <a-camera 
                look-controls="pointerLockEnabled: false"
                wasd-controls="acceleration: 20"
                cursor="rayOrigin: mouse; fuse: false">
                
                <!-- VR Hand Controls -->
                <a-entity 
                    hand-tracking-controls="hand: left"
                    laser-controls="hand: left">
                </a-entity>
                
                <a-entity 
                    hand-tracking-controls="hand: right"
                    laser-controls="hand: right">
                </a-entity>
            </a-camera>
        </a-entity>
    </a-scene>

    <script>
        // Simple VR entry function
        function enterVR() {
            const scene = document.querySelector('a-scene');
            scene.enterVR();
        }

        // Optional: Add click interactions to clothing items
        document.addEventListener('DOMContentLoaded', function() {
            const clothingItems = document.querySelectorAll('.clothing-item');
            
            clothingItems.forEach((item, index) => {
                item.addEventListener('click', function() {
                    console.log(`Clicked clothing item ${index + 1}`);
                    // You can add custom interactions here
                    // For example: show product details, zoom in, etc.
                });
                
                // Optional hover effects
                item.addEventListener('mouseenter', function() {
                    this.setAttribute('animation__hover', 'property: scale; to: 1.1 1.1 1.1; dur: 300');
                });
                
                item.addEventListener('mouseleave', function() {
                    this.setAttribute('animation__hover', 'property: scale; to: 1 1 1; dur: 300');
                });
            });
        });
    </script>
</body>
</html>
