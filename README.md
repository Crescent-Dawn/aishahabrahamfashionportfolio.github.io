<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Virtual Store - Three.js WebXR</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    
    <style>
        body {
            margin: 0;
            padding: 0;
            background: #000;
            font-family: Arial, sans-serif;
            overflow: hidden;
        }
        
        #container {
            width: 100vw;
            height: 100vh;
        }
        
        .ui {
            position: fixed;
            top: 10px;
            left: 10px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 15px;
            border-radius: 8px;
            z-index: 100;
            font-size: 14px;
            max-width: 300px;
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
            display: none;
        }
        
        .vr-button:hover {
            background: #0056b3;
        }
        
        .loading {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: white;
            font-size: 18px;
            z-index: 200;
        }
        
        .error {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: #ff6b6b;
            background: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            z-index: 200;
            max-width: 400px;
        }
    </style>
</head>
<body>
    <!-- UI Panel -->
    <div class="ui">
        <h3>🏪 Virtual Store</h3>
        <div id="status">Loading...</div>
        <div style="margin-top: 10px;">
            <strong>Controls:</strong><br>
            • Mouse: Look around<br>
            • WASD: Move<br>
            • VR: Use headset controllers
        </div>
    </div>
    
    <!-- VR Button -->
    <button id="vr-button" class="vr-button">Enter VR</button>
    
    <!-- Loading Message -->
    <div id="loading" class="loading">
        Loading your store...
    </div>
    
    <!-- Error Message -->
    <div id="error" class="error" style="display: none;">
        <h3>⚠️ Could not load store model</h3>
        <p>Please check:</p>
        <ul style="text-align: left;">
            <li>File exists at: <code>Assets/gltf/Basic Boutique MK1.glb</code></li>
            <li>File is not corrupted</li>
            <li>Internet connection is working</li>
        </ul>
    </div>
    
    <!-- Three.js Canvas Container -->
    <div id="container"></div>

    <script>
        let scene, camera, renderer, controls;
        let clock = new THREE.Clock();
        let moveForward = false, moveBackward = false, moveLeft = false, moveRight = false;
        let velocity = new THREE.Vector3();
        let direction = new THREE.Vector3();
        
        // Initialize the 3D scene
        function init() {
            console.log('🚀 Initializing Three.js WebXR store...');
            
            // Create scene
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x87CEEB); // Sky blue
            
            // Create camera
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 1.6, 3);
            
            // Create renderer
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.shadowMap.enabled = true;
            renderer.shadowMap.type = THREE.PCFSoftShadowMap;
            renderer.xr.enabled = true; // Enable WebXR
            
            document.getElementById('container').appendChild(renderer.domElement);
            
            // Add lighting
            const ambientLight = new THREE.AmbientLight(0x404040, 0.6);
            scene.add(ambientLight);
            
            const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
            directionalLight.position.set(5, 10, 5);
            directionalLight.castShadow = true;
            directionalLight.shadow.mapSize.width = 2048;
            directionalLight.shadow.mapSize.height = 2048;
            scene.add(directionalLight);
            
            // Add a backup floor
            const floorGeometry = new THREE.PlaneGeometry(50, 50);
            const floorMaterial = new THREE.MeshLambertMaterial({ color: 0xcccccc });
            const floor = new THREE.Mesh(floorGeometry, floorMaterial);
            floor.rotation.x = -Math.PI / 2;
            floor.receiveShadow = true;
            scene.add(floor);
            
            // Load your store model
            loadStoreModel();
            
            // Setup controls
            setupControls();
            
            // Setup WebXR
            setupWebXR();
            
            // Start render loop
            renderer.setAnimationLoop(animate);
            
            // Hide loading message
            document.getElementById('loading').style.display = 'none';
            document.getElementById('status').textContent = 'Ready! Use WASD to move.';
        }
        
        function loadStoreModel() {
            console.log('📦 Loading store model...');
            
            const loader = new THREE.GLTFLoader();
            
            // Add this script tag for GLTFLoader
            const script = document.createElement('script');
            script.src = 'https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/examples/js/loaders/GLTFLoader.js';
            script.onload = function() {
                const loader = new THREE.GLTFLoader();
                
                loader.load(
                    // Your model path - update this if needed
                    'Assets/gltf/Basic%20Boutique%20MK1.glb',
                    
                    // Success callback
                    function(gltf) {
                        console.log('✅ Store model loaded successfully!');
                        scene.add(gltf.scene);
                        
                        // Enable shadows on the model
                        gltf.scene.traverse(function(child) {
                            if (child.isMesh) {
                                child.castShadow = true;
                                child.receiveShadow = true;
                            }
                        });
                        
                        document.getElementById('status').textContent = 'Store loaded! Use WASD to explore.';
                    },
                    
                    // Progress callback
                    function(progress) {
                        const percent = Math.round((progress.loaded / progress.total) * 100);
                        console.log(`📥 Loading progress: ${percent}%`);
                        document.getElementById('status').textContent = `Loading store... ${percent}%`;
                    },
                    
                    // Error callback
                    function(error) {
                        console.error('❌ Error loading store model:', error);
                        document.getElementById('loading').style.display = 'none';
                        document.getElementById('error').style.display = 'block';
                        document.getElementById('status').textContent = 'Failed to load store model.';
                    }
                );
            };
            
            script.onerror = function() {
                console.error('❌ Failed to load GLTFLoader');
                document.getElementById('error').style.display = 'block';
                document.getElementById('loading').style.display = 'none';
            };
            
            document.head.appendChild(script);
        }
        
        function setupControls() {
            // Pointer lock for mouse look
            document.addEventListener('click', function() {
                document.body.requestPointerLock();
            });
            
            document.addEventListener('mousemove', function(event) {
                if (document.pointerLockElement === document.body) {
                    const sensitivity = 0.002;
                    camera.rotation.y -= event.movementX * sensitivity;
                    camera.rotation.x -= event.movementY * sensitivity;
                    camera.rotation.x = Math.max(-Math.PI/2, Math.min(Math.PI/2, camera.rotation.x));
                }
            });
            
            // Keyboard controls
            document.addEventListener('keydown', function(event) {
                switch(event.code) {
                    case 'KeyW': moveForward = true; break;
                    case 'KeyS': moveBackward = true; break;
                    case 'KeyA': moveLeft = true; break;
                    case 'KeyD': moveRight = true; break;
                }
            });
            
            document.addEventListener('keyup', function(event) {
                switch(event.code) {
                    case 'KeyW': moveForward = false; break;
                    case 'KeyS': moveBackward = false; break;
                    case 'KeyA': moveLeft = false; break;
                    case 'KeyD': moveRight = false; break;
                }
            });
        }
        
        function setupWebXR() {
            // Check for WebXR support
            if ('xr' in navigator) {
                navigator.xr.isSessionSupported('immersive-vr').then(function(supported) {
                    if (supported) {
                        const vrButton = document.getElementById('vr-button');
                        vrButton.style.display = 'block';
                        vrButton.addEventListener('click', function() {
                            renderer.xr.getSession() ? renderer.xr.getSession().end() : renderer.xr.getDevice().requestSession();
                        });
                    }
                });
            }
        }
        
        function updateMovement() {
            const delta = clock.getDelta();
            
            velocity.x -= velocity.x * 10.0 * delta;
            velocity.z -= velocity.z * 10.0 * delta;
            
            direction.z = Number(moveForward) - Number(moveBackward);
            direction.x = Number(moveRight) - Number(moveLeft);
            direction.normalize();
            
            if (moveForward || moveBackward) velocity.z -= direction.z * 10.0 * delta;
            if (moveLeft || moveRight) velocity.x -= direction.x * 10.0 * delta;
            
            // Apply movement relative to camera direction
            const forward = new THREE.Vector3(0, 0, -1);
            forward.applyQuaternion(camera.quaternion);
            forward.y = 0;
            forward.normalize();
            
            const right = new THREE.Vector3(1, 0, 0);
            right.applyQuaternion(camera.quaternion);
            right.y = 0;
            right.normalize();
            
            const moveVector = new THREE.Vector3();
            moveVector.addScaledVector(forward, -velocity.z);
            moveVector.addScaledVector(right, -velocity.x);
            
            camera.position.add(moveVector);
        }
        
        function animate() {
            updateMovement();
            renderer.render(scene, camera);
        }
        
        // Handle window resize
        window.addEventListener('resize', function() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
        
        // Start everything
        init();
    </script>
</body>
</html>
