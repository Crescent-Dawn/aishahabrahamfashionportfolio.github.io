<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Virtual Store</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/aframe/1.4.0/aframe.min.js"></script>
    
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
        }
        
        .instructions {
            position: fixed;
            top: 10px;
            left: 10px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 10px 15px;
            border-radius: 5px;
            font-size: 14px;
            z-index: 100;
            max-width: 250px;
        }
        
        .vr-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #007bff;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 20px;
            cursor: pointer;
            z-index: 100;
        }
    </style>
</head>
<body>
    <!-- Instructions -->
    <div class="instructions">
        <strong>Controls:</strong><br>
        Mouse: Look around<br>
        WASD: Move<br>
        VR: Use controllers
    </div>
    
    <!-- VR Button -->
    <button class="vr-btn" onclick="document.querySelector('a-scene').enterVR()">VR Mode</button>

    <!-- WebXR Scene -->
    <a-scene 
        vr-mode-ui="enabled: true" 
        background="color: skyblue"
        embedded>
        
        <!-- Load Your Store Model -->
        <a-assets>
            <!-- Replace "models/store.glb" with your actual file path -->
            <a-asset-item id="store" src="assets/gltf/Basic-Boutique-MK1.glb"></a-asset-item>
        </a-assets>

        <!-- Basic Lighting -->
        <a-light type="ambient" color="#ffffff" intensity="0.5"></a-light>
        <a-light type="directional" position="2 4 2" intensity="0.8"></a-light>

        <!-- Your Store Model -->
        <a-gltf-model 
            src="#store" 
            position="0 0 0" 
            scale="1 1 1">
        </a-gltf-model>

        <!-- Player Camera -->
        <a-entity 
            id="player" 
            position="0 1.6 3"
            movement-controls="fly: false; speed: 0.1">
            
            <a-camera 
                look-controls="pointerLockEnabled: false"
                wasd-controls="enabled: true; acceleration: 15">
                
                <!-- VR Controllers -->
                <a-entity 
                    hand-tracking-controls="hand: left"
                    vive-controls="hand: left"
                    oculus-touch-controls="hand: left">
                </a-entity>
                
                <a-entity 
                    hand-tracking-controls="hand: right"
                    vive-controls="hand: right"
                    oculus-touch-controls="hand: right">
                </a-entity>
            </a-camera>
        </a-entity>
    </a-scene>

    <script>
        // Log when the model loads or fails
        document.addEventListener('DOMContentLoaded', function() {
            const storeModel = document.querySelector('#store');
            
            storeModel.addEventListener('loaded', function() {
                console.log('✅ Store model loaded successfully!');
            });
            
            storeModel.addEventListener('error', function() {
                console.error('❌ Failed to load store model');
                console.error('Check: 1) File exists at models/store.glb, 2) File is not corrupted');
            });
        });
    </script>
</body>
</html>
