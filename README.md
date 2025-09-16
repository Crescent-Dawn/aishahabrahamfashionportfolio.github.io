<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive WebXR Map</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
        }
        
        #container {
            position: relative;
            width: 100vw;
            height: 100vh;
        }
        
        #ui {
            position: absolute;
            top: 20px;
            left: 20px;
            z-index: 100;
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 12px;
            color: white;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .btn {
            background: linear-gradient(45deg, #667eea 0%, #764ba2 100%);
            border: none;
            color: white;
            padding: 12px 24px;
            margin: 5px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        
        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }
        
        #loading {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: white;
            font-size: 18px;
            text-align: center;
            z-index: 200;
            background: rgba(0, 0, 0, 0.8);
            padding: 30px;
            border-radius: 12px;
            backdrop-filter: blur(10px);
        }
        
        .spinner {
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top: 3px solid white;
            width: 30px;
            height: 30px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        #info {
            position: absolute;
            bottom: 20px;
            left: 20px;
            color: white;
            background: rgba(0, 0, 0, 0.7);
            padding: 15px;
            border-radius: 8px;
            font-size: 12px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
    </style>
</head>
<body>
    <div id="container">
        <div id="loading">
            <div class="spinner"></div>
            Loading 3D Map...
        </div>
        
        <div id="ui">
            <h3 style="margin-top: 0;">WebXR Interactive Map</h3>
            <button id="vrButton" class="btn">Enter VR</button>
            <button id="arButton" class="btn">Enter AR</button>
            <br>
            <button id="resetView" class="btn">Reset View</button>
            <button id="toggleWireframe" class="btn">Toggle Wireframe</button>
            <br>
            <label for="modelPath" style="color: white;">GLB Model Path:</label>
            <input type="text" id="modelPath" placeholder="./model.glb" value="./model.glb" 
                   style="width: 200px; padding: 8px; margin: 5px 0; border-radius: 4px; border: none;">
            <button id="loadModel" class="btn">Load Model</button>
        </div>
        
        <div id="info">
            Controls:<br>
            • Mouse: Look around<br>
            • WASD: Move<br>
            • Scroll: Zoom<br>
            • Click objects to interact
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        // Global variables
        let scene, camera, renderer, controls;
        let loadedModel = null;
        let raycaster, mouse;
        let isWireframe = false;
        
        // Initialize the 3D scene
        function init() {
            // Create scene
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x87ceeb);
            scene.fog = new THREE.Fog(0x87ceeb, 10, 1000);
            
            // Create camera
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 10, 20);
            
            // Create renderer
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.shadowMap.enabled = true;
            renderer.shadowMap.type = THREE.PCFSoftShadowMap;
            renderer.toneMapping = THREE.ACESFilmicToneMapping;
            renderer.toneMappingExposure = 1.2;
            renderer.xr.enabled = true;
            
            document.getElementById('container').appendChild(renderer.domElement);
            
            // Setup controls (simple mouse controls)
            setupControls();
            
            // Setup lighting
            setupLighting();
            
            // Setup raycaster for interaction
            raycaster = new THREE.Raycaster();
            mouse = new THREE.Vector2();
            
            // Add ground plane
            addGroundPlane();
            
            // Setup WebXR
            setupWebXR();
            
            // Event listeners
            setupEventListeners();
            
            // Hide loading screen
            document.getElementById('loading').style.display = 'none';
            
            // Start render loop
            animate();
        }
        
        function setupControls() {
            // Simple first-person style controls
            const keys = {};
            const moveSpeed = 0.5;
            const lookSpeed = 0.002;
            
            let pitch = 0;
            let yaw = 0;
            
            document.addEventListener('keydown', (e) => keys[e.code] = true);
            document.addEventListener('keyup', (e) => keys[e.code] = false);
            
            document.addEventListener('mousemove', (e) => {
                if (document.pointerLockElement === renderer.domElement) {
                    yaw -= e.movementX * lookSpeed;
                    pitch -= e.movementY * lookSpeed;
                    pitch = Math.max(-Math.PI/2, Math.min(Math.PI/2, pitch));
                    
                    camera.rotation.set(pitch, yaw, 0);
                }
            });
            
            renderer.domElement.addEventListener('click', () => {
                renderer.domElement.requestPointerLock();
            });
            
            // Movement update function
            window.updateControls = () => {
                const direction = new THREE.Vector3();
                
                if (keys['KeyW']) direction.z -= 1;
                if (keys['KeyS']) direction.z += 1;
                if (keys['KeyA']) direction.x -= 1;
                if (keys['KeyD']) direction.x += 1;
                
                if (direction.length() > 0) {
                    direction.normalize();
                    direction.multiplyScalar(moveSpeed);
                    direction.applyQuaternion(camera.quaternion);
                    camera.position.add(direction);
                }
            };
        }
        
        function setupLighting() {
            // Ambient light
            const ambientLight = new THREE.AmbientLight(0x404040, 0.6);
            scene.add(ambientLight);
            
            // Directional light (sun)
            const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
            directionalLight.position.set(50, 100, 50);
            directionalLight.castShadow = true;
            directionalLight.shadow.mapSize.width = 2048;
            directionalLight.shadow.mapSize.height = 2048;
            directionalLight.shadow.camera.near = 0.5;
            directionalLight.shadow.camera.far = 200;
            directionalLight.shadow.camera.left = -50;
            directionalLight.shadow.camera.right = 50;
            directionalLight.shadow.camera.top = 50;
            directionalLight.shadow.camera.bottom = -50;
            scene.add(directionalLight);
            
            // Point lights for atmosphere
            const pointLight1 = new THREE.PointLight(0xff6b6b, 0.5, 50);
            pointLight1.position.set(-20, 15, -20);
            scene.add(pointLight1);
            
            const pointLight2 = new THREE.PointLight(0x4ecdc4, 0.5, 50);
            pointLight2.position.set(20, 15, 20);
            scene.add(pointLight2);
        }
        
        function addGroundPlane() {
            const geometry = new THREE.PlaneGeometry(100, 100);
            const material = new THREE.MeshLambertMaterial({ 
                color: 0x90ee90,
                transparent: true,
                opacity: 0.7
            });
            const ground = new THREE.Mesh(geometry, material);
            ground.rotation.x = -Math.PI / 2;
            ground.receiveShadow = true;
            scene.add(ground);
        }
        
        function setupWebXR() {
            // Check for WebXR support
            if ('xr' in navigator) {
                navigator.xr.isSessionSupported('immersive-vr').then((supported) => {
                    document.getElementById('vrButton').disabled = !supported;
                });
                
                navigator.xr.isSessionSupported('immersive-ar').then((supported) => {
                    document.getElementById('arButton').disabled = !supported;
                });
            }
            
            // VR Button
            document.getElementById('vrButton').addEventListener('click', () => {
                if (renderer.xr.getSession()) {
                    renderer.xr.getSession().end();
                } else {
                    navigator.xr.requestSession('immersive-vr', {
                        requiredFeatures: ['local-floor']
                    }).then((session) => {
                        renderer.xr.setSession(session);
                    }).catch((err) => {
                        console.warn('VR not available:', err);
                        alert('VR not available on this device');
                    });
                }
            });
            
            // AR Button  
            document.getElementById('arButton').addEventListener('click', () => {
                if (renderer.xr.getSession()) {
                    renderer.xr.getSession().end();
                } else {
                    navigator.xr.requestSession('immersive-ar', {
                        requiredFeatures: ['local-floor'],
                        optionalFeatures: ['dom-overlay'],
                        domOverlay: { root: document.body }
                    }).then((session) => {
                        renderer.xr.setSession(session);
                        scene.background = null; // Transparent background for AR
                    }).catch((err) => {
                        console.warn('AR not available:', err);
                        alert('AR not available on this device');
                    });
                }
            });
        }
        
        function setupEventListeners() {
            // Window resize
            window.addEventListener('resize', onWindowResize);
            
            // Mouse interaction
            renderer.domElement.addEventListener('dblclick', onMouseClick);
            
            // UI buttons
            document.getElementById('resetView').addEventListener('click', () => {
                camera.position.set(0, 10, 20);
                camera.rotation.set(0, 0, 0);
            });
            
            document.getElementById('toggleWireframe').addEventListener('click', () => {
                isWireframe = !isWireframe;
                if (loadedModel) {
                    loadedModel.traverse((child) => {
                        if (child.isMesh && child.material) {
                            child.material.wireframe = isWireframe;
                        }
                    });
                }
            });
            
            document.getElementById('loadModel').addEventListener('click', () => {
                const path = document.getElementById('modelPath').value;
                loadGLBModel(path);
            });
        }
        
        function loadGLBModel(path) {
            document.getElementById('loading').style.display = 'block';
            
            const loader = new THREE.GLTFLoader();
            loader.load(
                path,
                (gltf) => {
                    // Remove previous model
                    if (loadedModel) {
                        scene.remove(loadedModel);
                    }
                    
                    loadedModel = gltf.scene;
                    loadedModel.traverse((child) => {
                        if (child.isMesh) {
                            child.castShadow = true;
                            child.receiveShadow = true;
                            
                            // Enhance materials
                            if (child.material) {
                                child.material.needsUpdate = true;
                            }
                        }
                    });
                    
                    // Scale and position the model
                    const box = new THREE.Box3().setFromObject(loadedModel);
                    const size = box.getSize(new THREE.Vector3()).length();
                    const center = box.getCenter(new THREE.Vector3());
                    
                    loadedModel.position.x += (loadedModel.position.x - center.x);
                    loadedModel.position.y += (loadedModel.position.y - center.y);
                    loadedModel.position.z += (loadedModel.position.z - center.z);
                    
                    // Scale to reasonable size
                    if (size > 50) {
                        loadedModel.scale.setScalar(50 / size);
                    }
                    
                    scene.add(loadedModel);
                    document.getElementById('loading').style.display = 'none';
                    
                    console.log('Model loaded successfully!');
                },
                (progress) => {
                    console.log('Loading progress:', (progress.loaded / progress.total * 100) + '%');
                },
                (error) => {
                    console.error('Error loading model:', error);
                    document.getElementById('loading').style.display = 'none';
                    alert('Error loading model. Please check the file path.');
                }
            );
        }
        
        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        }
        
        function onMouseClick(event) {
            mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
            mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
            
            raycaster.setFromCamera(mouse, camera);
            
            if (loadedModel) {
                const intersects = raycaster.intersectObject(loadedModel, true);
                
                if (intersects.length > 0) {
                    const point = intersects[0].point;
                    
                    // Add a marker at clicked point
                    const markerGeometry = new THREE.SphereGeometry(0.5, 16, 16);
                    const markerMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
                    const marker = new THREE.Mesh(markerGeometry, markerMaterial);
                    marker.position.copy(point);
                    scene.add(marker);
                    
                    // Remove marker after 3 seconds
                    setTimeout(() => {
                        scene.remove(marker);
                    }, 3000);
                    
                    console.log('Clicked point:', point);
                }
            }
        }
        
        function animate() {
            renderer.setAnimationLoop(() => {
                // Update controls if not in XR mode
                if (!renderer.xr.getSession() && window.updateControls) {
                    window.updateControls();
                }
                
                renderer.render(scene, camera);
            });
        }
        
        // Initialize when page loads
        window.addEventListener('load', init);
        
        // Add GLTFLoader manually since it's not in the core Three.js build
        (function() {
            const script = document.createElement('script');
            script.src = 'https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/examples/js/loaders/GLTFLoader.js';
            script.onload = function() {
                console.log('GLTFLoader loaded');
            };
            document.head.appendChild(script);
        })();
    </script>
</body>
</html>
