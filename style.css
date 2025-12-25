/* 
 * LaunchPad Network
 * 3D Scene & Form Logic
 */

import * as THREE from 'https://unpkg.com/three@0.160.0/build/three.module.js';

// --- 3D Scene Configuration ---
const initScene = () => {
    const container = document.getElementById('canvas-container');
    
    // Scene Setup
    const scene = new THREE.Scene();
    scene.fog = new THREE.FogExp2(0x050505, 0.002);

    // Camera
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 30;

    // Renderer
    const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    container.appendChild(renderer.domElement);

    // --- Geometry: Abstract Floating Network ---
    
    // 1. Central Wireframe Icosahedron
    const geometry = new THREE.IcosahedronGeometry(10, 1);
    const material = new THREE.MeshBasicMaterial({ 
        color: 0x4f7eff, 
        wireframe: true, 
        transparent: true, 
        opacity: 0.15 
    });
    const sphere = new THREE.Mesh(geometry, material);
    scene.add(sphere);

    // 2. Particle System
    const particlesGeometry = new THREE.BufferGeometry();
    const particlesCount = 700;
    const posArray = new Float32Array(particlesCount * 3);

    for(let i = 0; i < particlesCount * 3; i++) {
        posArray[i] = (Math.random() - 0.5) * 80; 
    }

    particlesGeometry.setAttribute('position', new THREE.BufferAttribute(posArray, 3));
    
    const spriteConfig = new THREE.PointsMaterial({
        size: 0.15,
        color: 0xffffff,
        transparent: true,
        opacity: 0.6,
        blending: THREE.AdditiveBlending
    });

    const particlesMesh = new THREE.Points(particlesGeometry, spriteConfig);
    scene.add(particlesMesh);

    // --- Lighting ---
    const ambientLight = new THREE.AmbientLight(0x404040, 2);
    scene.add(ambientLight);

    const pointLight = new THREE.PointLight(0x4f7eff, 2, 100);
    pointLight.position.set(10, 10, 10);
    scene.add(pointLight);

    // --- Interaction ---
    let mouseX = 0;
    let mouseY = 0;
    let targetX = 0;
    let targetY = 0;
    const windowHalfX = window.innerWidth / 2;
    const windowHalfY = window.innerHeight / 2;

    document.addEventListener('mousemove', (event) => {
        mouseX = (event.clientX - windowHalfX);
        mouseY = (event.clientY - windowHalfY);
    });

    window.addEventListener('resize', () => {
        const width = window.innerWidth;
        const height = window.innerHeight;
        renderer.setSize(width, height);
        camera.aspect = width / height;
        camera.updateProjectionMatrix();
    });

    // --- Animation ---
    const clock = new THREE.Clock();

    const animate = () => {
        requestAnimationFrame(animate);
        const elapsedTime = clock.getElapsedTime();

        sphere.rotation.y += 0.002;
        sphere.rotation.x += 0.001;

        particlesMesh.rotation.y = -elapsedTime * 0.05;
        particlesMesh.rotation.x = elapsedTime * 0.01;

        targetX = mouseX * 0.001;
        targetY = mouseY * 0.001;

        sphere.rotation.y += 0.05 * (targetX - sphere.rotation.y);
        sphere.rotation.x += 0.05 * (targetY - sphere.rotation.x);
        
        particlesMesh.position.y = Math.sin(elapsedTime * 0.5) * 0.5;

        renderer.render(scene, camera);
    };

    animate();
};

// --- Form Logic ---
const initForm = () => {
    // Slider Logic
    const slider = document.getElementById('profit-share');
    const output = document.getElementById('profit-value');

    if(slider && output) {
        // Event listener to update text dynamically when user slides
        slider.addEventListener('input', function() {
            output.innerHTML = this.value + "%";
        });
    }

    // AJAX Submission
    const form = document.getElementById('launchForm');
    const statusMsg = document.getElementById('form-status');

    if(form) {
        form.addEventListener('submit', async function(event) {
            event.preventDefault(); 
            
            const submitBtn = form.querySelector('.submit-button');
            const originalBtnText = submitBtn.innerText;
            
            submitBtn.innerText = "Transmitting...";
            submitBtn.disabled = true;
            statusMsg.innerText = "";
            statusMsg.className = "status-message";

            const data = new FormData(event.target);

            try {
                const response = await fetch(event.target.action, {
                    method: form.method,
                    body: data,
                    headers: {
                        'Accept': 'application/json'
                    }
                });

                if (response.ok) {
                    statusMsg.innerText = "Registration Confirmed. Protocol Initiated.";
                    statusMsg.classList.add("success");
                    form.reset();
                    // Reset slider display
                    if(output) output.innerHTML = "10%";
                    submitBtn.innerText = "Sent";
                } else {
                    const errorData = await response.json();
                    if (Object.hasOwn(errorData, 'errors')) {
                        statusMsg.innerText = errorData["errors"].map(error => error["message"]).join(", ");
                    } else {
                        statusMsg.innerText = "Transmission failed. Verify network.";
                    }
                    statusMsg.classList.add("error");
                    submitBtn.innerText = originalBtnText;
                    submitBtn.disabled = false;
                }
            } catch (error) {
                statusMsg.innerText = "Connection Error. Please retry.";
                statusMsg.classList.add("error");
                submitBtn.innerText = originalBtnText;
                submitBtn.disabled = false;
            }
        });
    }
};

document.addEventListener('DOMContentLoaded', () => {
    initScene();
    initForm();
});
