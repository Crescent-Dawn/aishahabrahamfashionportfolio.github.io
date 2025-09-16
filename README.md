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
            <!-- Your store model - update the path as needed -->
            <a-asset-item id="store" src="Assets/gltf/Basic%20Boutique%20MK1.glb"></a-asset-item>
            
            <!-- Test model to verify A-Frame is working -->
            <a-asset-item id="test" src="https://cdn.aframe.io/test-models/models/glTF-2.0/Duck/glTF/Duck.gltf"></a-asset-item>
        </a-assets>

        <!-- Basic Lighting -->
        <a-light type="ambient" color="#ffffff" intensity="0.5"></a-light>
        <a-light type="directional" position="2 4 2" intensity="0.8"></a-light>

        <!-- Test Model (should appear as a spinning duck) -->
        <a-gltf-model 
            src="#test" 
            position="2 1 -2" 
            scale="0.5 0.5 0.5"
            animation="property: rotation; to: 0 360 0; dur: 10000; loop: true">
        </a-gltf-model>

        <!-- Your Store Model -->
        <a-gltf-model 
            src="#store" 
            position="0 0 0" 
            scale="1 1 1">
        </a-gltf-model>

        <!-- Backup floor in case your model doesn't have one -->
        <a-plane 
            position="0 0 0" 
            rotation="-90 0 0" 
            width="20" 
            height="20" 
            color="#cccccc"
            visible="true">
        </a-plane>

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
        // Debug: Check what's happening
        document.addEventListener('DOMContentLoaded', function() {
            console.log('🔍 Debug Info:');
            console.log('Current page URL:', window.location.href);
            console.log('Looking for model at:', window.location.origin + window.location.pathname.replace('index.html', '') + 'Assets/gltf/Basic%20Boutique%20MK1.glb');
            
            const storeAsset = document.querySelector('#store');
            const testAsset = document.querySelector('#test');
            
            // Check test model (duck)
            testAsset.addEventListener('loaded', function() {
                console.log('✅ Test duck model loaded - A-Frame is working!');
            });
            
            testAsset.addEventListener('error', function() {
                console.error('❌ Test model failed - internet connection issue');
            });
            
            // Check your store model
            storeAsset.addEventListener('loaded', function() {
                console.log('✅ Store model loaded successfully!');
            });
            
            storeAsset.addEventListener('error', function(e) {
                console.error('❌ Store model failed to load');
                console.error('Check these things:');
                console.error('1. File exists at: Assets/gltf/Basic Boutique MK1.glb');
                console.error('2. File is not corrupted');
                console.error('3. File size is under GitHub limits');
                console.error('4. Path is correct (case sensitive)');
                console.error('Error details:', e);
            });
        });
        
        // Check A-Frame scene loading
        document.querySelector('a-scene').addEventListener('loaded', function() {
            console.log('✅ A-Frame scene loaded');
        });
    </script>
</body>
</html>
