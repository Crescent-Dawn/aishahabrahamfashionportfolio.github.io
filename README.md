<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digital Boutique - WebXR Experience</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/aframe/1.4.0/aframe.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/aframe/1.4.0/aframe-ar.js"></script>
    
    <style>
        body {
            margin: 0;
            font-family: 'Arial', sans-serif;
            background: #000;
        }
        
        .ui-overlay {
            position: fixed;
            top: 20px;
            left: 20px;
            z-index: 1000;
            background: rgba(255, 255, 255, 0.9);
            padding: 15px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
            max-width: 300px;
        }
        
        .controls {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 1000;
            background: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 15px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
        }
        
        .control-btn {
            background: #4A90E2;
            color: white;
            border: none;
            padding: 10px 15px;
            margin: 5px;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .control-btn:hover {
            background: #357ABD;
            transform: translateY(-2px);
        }
        
        .loading {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 2000;
            background: rgba(0, 0, 0, 0.9);
            color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }
        
        .info-panel {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 1500;
            background: rgba(255, 255, 255, 0.95);
            padding: 20px;
            border-radius: 15px;
            max-width: 400px;
            display: none;
            backdrop-filter: blur(15px);
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .close-btn {
            position: absolute;
            top: 10px;
            right: 15px;
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: #666;
        }
        
        .enter-vr {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 1000;
            background: linear-gradient(45deg, #FF6B6B, #4ECDC4);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 20px rgba(0,0,0,0.3);
        }
        
        .enter-vr:hover {
            transform: translateX(-50%) translateY(-3px);
            box-shadow: 0 8px 25px rgba(0,0,0,0.4);
        }
        
        @media (max-width: 768px) {
            .ui-overlay, .controls {
                position: relative;
                margin: 10px;
                max-width: none;
            }
        }
    </style>
</head>
<body>
    <!-- Loading Screen -->
    <div id="loading" class="loading">
        <h2>Loading Digital Boutique...</h2>
        <p>Preparing your immersive fashion experience</p>
    </div>
    
    <!-- UI Overlay -->
    <div class="ui-overlay">
        <h3>🌟 Digital Boutique</h3>
        <p><strong>Navigation:</strong></p>
        <p>• Desktop: Click & drag to look around, WASD to move</p>
        <p>• Mobile: Touch & drag, tap to teleport</p>
        <p>• VR: Use controllers or gaze to interact</p>
        <p><strong>Click on items to view details!</strong></p>
    </div>
    
    <!-- Controls -->
    <div class="controls">
        <h4>Lighting</h4>
        <button class="control-btn" onclick="toggleLighting('day')">Day</button>
        <button class="control-btn" onclick="toggleLighting('night')">Night</button>
        <button class="control-btn" onclick="toggleLighting('spotlight')">Spotlight</button>
    </div>
    
    <!-- Enter VR Button -->
    <button class="enter-vr" onclick="enterVR()">Enter VR Experience</button>
    
    <!-- Item Info Panel -->
    <div id="info-panel" class="info-panel">
        <button class="close-btn" onclick="closeInfo()">&times;</button>
        <div id="info-content">
            <h3>Item Details</h3>
            <p>Click on clothing items to see details here!</p>
        </div>
    </div>

    <!-- A-Frame Scene -->
    <a-scene 
        vr-mode-ui="enabled: true" 
        embedded
        background="color: #1a1a2e"
        fog="type: exponential; color: #1a1a2e; density: 0.1"
        loading-screen="dotsColor: white; backgroundColor: #1a1a2e">
        
        <!-- Assets -->
        <a-assets>
            <!-- Environment Textures -->
            <a-asset-item id="floor-texture" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNkYPhfDwAChwGA60e6kgAAAABJRU5ErkJggg=="></a-asset-item>
            
            <!-- Sample Clothing Models (Replace with your GLTF files) -->
            <!-- <a-asset-item id="dress1" src="models/dress1.gltf"></a-asset-item> -->
            <!-- <a-asset-item id="jacket1" src="models/jacket1.gltf"></a-asset-item> -->
            <!-- <a-asset-item id="pants1" src="models/pants1.gltf"></a-asset-item> -->
            
            <!-- Audio -->
            <audio id="ambient-sound" src="data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBj+a2/LDciUFLIHO8tiJNwgZaLvt559NEAxQp+PwtmMcBjiR1/LMeSwFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAoUXrTp66hVFApGn+LyumAaBC2N1/LMeSsFJHfH8N2QQAo=" preload="auto"></audio>
        </a-assets>

        <!-- Lighting Setup -->
        <a-light id="ambient-light" type="ambient" color="#404040" intensity="0.6"></a-light>
        <a-light id="main-light" type="directional" position="5 10 5" color="#ffffff" intensity="0.8" castShadow="true"></a-light>
        <a-light id="fill-light" type="point" position="-3 5 -3" color="#4A90E2" intensity="0.4"></a-light>
        <a-light id="accent-light" type="spot" position="0 8 0" color="#FF6B6B" intensity="0.3" angle="45"></a-light>

        <!-- Environment -->
        <!-- Floor -->
        <a-plane 
            id="floor" 
            position="0 -0.1 0" 
            rotation="-90 0 0" 
            width="20" 
            height="20" 
            color="#2a2a3e" 
            shadow="receive: true"
            material="roughness: 0.8; metalness: 0.1">
        </a-plane>
        
        <!-- Walls -->
        <a-plane position="0 5 -10" width="20" height="10" color="#1e1e2e" material="roughness: 0.9"></a-plane>
        <a-plane position="-10 5 0" rotation="0 90 0" width="20" height="10" color="#1e1e2e" material="roughness: 0.9"></a-plane>
        <a-plane position="10 5 0" rotation="0 -90 0" width="20" height="10" color="#1e1e2e" material="roughness: 0.9"></a-plane>
        
        <!-- Ceiling -->
        <a-plane position="0 10 0" rotation="90 0 0" width="20" height="20" color="#0f0f1f" material="roughness: 0.9"></a-plane>

        <!-- Display Pedestals -->
        <a-cylinder 
            id="pedestal1" 
            position="-4 0.5 -2" 
            radius="1" 
            height="1" 
            color="#4A90E2" 
            shadow="cast: true; receive: true"
            material="roughness: 0.3; metalness: 0.7"
            class="interactive">
        </a-cylinder>
        
        <a-cylinder 
            id="pedestal2" 
            position="0 0.5 -2" 
            radius="1" 
            height="1" 
            color="#4A90E2" 
            shadow="cast: true; receive: true"
            material="roughness: 0.3; metalness: 0.7"
            class="interactive">
        </a-cylinder>
        
        <a-cylinder 
            id="pedestal3" 
            position="4 0.5 -2" 
            radius="1" 
            height="1" 
            color="#4A90E2" 
            shadow="cast: true; receive: true"
            material="roughness: 0.3; metalness: 0.7"
            class="interactive">
        </a-cylinder>

        <!-- Sample Clothing Items (Replace with your models) -->
        <!-- Item 1: Dress -->
        <a-box 
            id="item1" 
            position="-4 2 -2" 
            width="0.8" 
            height="1.5" 
            depth="0.3" 
            color="#FF6B6B" 
            shadow="cast: true"
            material="roughness: 0.2; metalness: 0.1"
            animation="property: rotation; to: 0 360 0; dur: 10000; loop: true; easing: linear"
            class="clothing-item"
            data-name="Elegant Evening Dress"
            data-description="A stunning evening dress perfect for special occasions. Made with premium materials and exquisite craftsmanship."
            data-price="$299">
        </a-box>
        
        <!-- Item 2: Jacket -->
        <a-box 
            id="item2" 
            position="0 2 -2" 
            width="1" 
            height="1.2" 
            depth="0.4" 
            color="#4ECDC4" 
            shadow="cast: true"
            material="roughness: 0.3; metalness: 0.2"
            animation="property: rotation; to: 0 360 0; dur: 12000; loop: true; easing: linear"
            class="clothing-item"
            data-name="Designer Blazer"
            data-description="A versatile blazer that combines style and comfort. Perfect for both professional and casual settings."
            data-price="$199">
        </a-box>
        
        <!-- Item 3: Pants -->
        <a-box 
            id="item3" 
            position="4 2 -2" 
            width="0.6" 
            height="1.4" 
            depth="0.3" 
            color="#FFD93D" 
            shadow="cast: true"
            material="roughness: 0.4; metalness: 0.1"
            animation="property: rotation; to: 0 360 0; dur: 8000; loop: true; easing: linear"
            class="clothing-item"
            data-name="Premium Trousers"
            data-description="High-quality trousers with a perfect fit. Designed for the modern professional who values comfort and style."
            data-price="$149">
        </a-box>

        <!-- Information Displays -->
        <a-text 
            position="-4 3.5 -1.5" 
            value="Evening Dress\n$299" 
            align="center" 
            color="#ffffff"
            font="dejavu"
            width="8">
        </a-text>
        
        <a-text 
            position="0 3.5 -1.5" 
            value="Designer Blazer\n$199" 
            align="center" 
            color="#ffffff"
            font="dejavu"
            width="8">
        </a-text>
        
        <a-text 
            position="4 3.5 -1.5" 
            value="Premium Trousers\n$149" 
            align="center" 
            color="#ffffff"
            font="dejavu"
            width="8">
        </a-text>

        <!-- Welcome Sign -->
        <a-text 
            position="0 6 -9" 
            value="WELCOME TO OUR DIGITAL BOUTIQUE\nExplore Our Latest Collection" 
            align="center" 
            color="#4A90E2"
            font="dejavu"
            width="12"
            animation="property: components.text.material.color; to: #FF6B6B; dur: 3000; dir: alternate; loop: true">
        </a-text>

        <!-- Camera with Controls -->
        <a-entity id="cameraRig" position="0 1.6 5" rotation="0 0 0">
            <a-camera 
                id="camera"
                look-controls="pointerLockEnabled: false"
                wasd-controls="acceleration: 20"
                cursor="rayOrigin: mouse; fuse: false"
                raycaster="objects: .interactive, .clothing-item">
                
                <!-- VR Controllers -->
                <a-entity 
                    id="leftHand" 
                    hand-tracking-controls="hand: left"
                    laser-controls="hand: left"
                    raycaster="objects: .interactive, .clothing-item; showLine: true; lineColor: #4A90E2">
                </a-entity>
                
                <a-entity 
                    id="rightHand" 
                    hand-tracking-controls="hand: right"
                    laser-controls="hand: right"
                    raycaster="objects: .interactive, .clothing-item; showLine: true; lineColor: #4A90E2">
                </a-entity>
            </a-camera>
        </a-entity>

        <!-- Particle Effects -->
        <a-entity 
            id="particles"
            position="0 5 0"
            particle-system="preset: snow; particleCount: 100; color: #4A90E2, #FF6B6B; size: 0.5; accelerationValue: 0 -0.1 0; velocityValue: 0 0 0; accelerationSpread: 2 0 2; velocitySpread: 1 0 1">
        </a-entity>

        <!-- Background Music -->
        <a-entity 
            id="ambient-audio"
            sound="src: #ambient-sound; autoplay: false; loop: true; volume: 0.1"
            position="0 0 0">
        </a-entity>
    </a-scene>

    <script>
        let currentScene;
        let vrButton;
        let isVRSupported = false;

        // Initialize scene
        document.addEventListener('DOMContentLoaded', function() {
            setTimeout(() => {
                document.getElementById('loading').style.display = 'none';
            }, 2000);

            // Check VR support
            if (navigator.xr) {
                navigator.xr.isSessionSupported('immersive-vr').then((supported) => {
                    isVRSupported = supported;
                    if (!supported) {
                        document.querySelector('.enter-vr').textContent = 'VR Not Available - Desktop Experience';
                    }
                });
            }

            // Add click listeners to clothing items
            const clothingItems = document.querySelectorAll('.clothing-item');
            clothingItems.forEach(item => {
                item.addEventListener('click', function() {
                    showItemInfo(this);
                });
            });

            // Add hover effects
            clothingItems.forEach(item => {
                item.addEventListener('mouseenter', function() {
                    this.setAttribute('animation__hover', 'property: scale; to: 1.1 1.1 1.1; dur: 300');
                });
                
                item.addEventListener('mouseleave', function() {
                    this.setAttribute('animation__hover', 'property: scale; to: 1 1 1; dur: 300');
                });
            });
        });

        // Show item information
        function showItemInfo(item) {
            const panel = document.getElementById('info-panel');
            const content = document.getElementById('info-content');
            
            const name = item.getAttribute('data-name');
            const description = item.getAttribute('data-description');
            const price = item.getAttribute('data-price');
            
            content.innerHTML = `
                <h3>${name}</h3>
                <p><strong>Price: ${price}</strong></p>
                <p>${description}</p>
                <div style="margin-top: 15px;">
                    <button class="control-btn" onclick="contactForItem('${name}')">Contact for Details</button>
                    <button class="control-btn" onclick="viewIn3D('${item.id}')">Focus View</button>
                </div>
            `;
            
            panel.style.display = 'block';
        }

        // Close info panel
        function closeInfo() {
            document.getElementById('info-panel').style.display = 'none';
        }

        // Contact for item
        function contactForItem(itemName) {
            alert(`Thanks for your interest in ${itemName}! In a real implementation, this would open a contact form or redirect to your contact page.`);
        }

        // Focus view on item
        function viewIn3D(itemId) {
            const camera = document.getElementById('cameraRig');
            const item = document.getElementById(itemId);
            
            if (item) {
                const itemPos = item.getAttribute('position');
                camera.setAttribute('animation', `property: position; to: ${itemPos.x} 1.6 ${itemPos.z + 3}; dur: 2000`);
                camera.setAttribute('animation__look', `property: rotation; to: 0 ${itemPos.x < 0 ? 15 : itemPos.x > 0 ? -15 : 0} 0; dur: 2000`);
            }
            
            closeInfo();
        }

        // Toggle lighting
        function toggleLighting(mode) {
            const ambientLight = document.getElementById('ambient-light');
            const mainLight = document.getElementById('main-light');
            const fillLight = document.getElementById('fill-light');
            const accentLight = document.getElementById('accent-light');
            
            switch(mode) {
                case 'day':
                    ambientLight.setAttribute('intensity', '0.8');
                    mainLight.setAttribute('intensity', '1.0');
                    fillLight.setAttribute('intensity', '0.3');
                    accentLight.setAttribute('intensity', '0.2');
                    break;
                case 'night':
                    ambientLight.setAttribute('intensity', '0.2');
                    mainLight.setAttribute('intensity', '0.3');
                    fillLight.setAttribute('intensity', '0.8');
                    accentLight.setAttribute('intensity', '0.6');
                    break;
                case 'spotlight':
                    ambientLight.setAttribute('intensity', '0.1');
                    mainLight.setAttribute('intensity', '0.2');
                    fillLight.setAttribute('intensity', '0.1');
                    accentLight.setAttribute('intensity', '1.2');
                    break;
            }
        }

        // Enter VR
        function enterVR() {
            const scene = document.querySelector('a-scene');
            if (isVRSupported) {
                scene.enterVR();
            } else {
                alert('VR is not supported on this device, but you can still explore in desktop mode!');
            }
        }

        // Keyboard shortcuts
        document.addEventListener('keydown', function(e) {
            switch(e.key) {
                case '1':
                    toggleLighting('day');
                    break;
                case '2':
                    toggleLighting('night');
                    break;
                case '3':
                    toggleLighting('spotlight');
                    break;
                case 'Escape':
                    closeInfo();
                    break;
            }
        });
    </script>
</body>
</html>
