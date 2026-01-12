<script>
    import { onMount, onDestroy } from "svelte";
    import * as THREE from "three";

    let container;
    let renderer, scene, camera, controls;
    let rafId;
    let sphere;

    onMount(() => {
        if (!container) return;

        let cleanupFn;

        const init = async () => {
            // Dynamic import to avoid build issues
            const { OrbitControls } = await import(
                "three/addons/controls/OrbitControls.js"
            );

            // 1. Scene
            scene = new THREE.Scene();

            // 2. Camera
            const width = container.clientWidth;
            const height = container.clientHeight;
            camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
            // @ts-ignore
            camera.position.set(0, 0, 0.1);

            // 3. Renderer
            // @ts-ignore
            renderer = new THREE.WebGLRenderer({
                antialias: true,
                alpha: true,
            });
            renderer.setSize(width, height);
            renderer.setPixelRatio(window.devicePixelRatio); // Use full device resolution
            // @ts-ignore
            container.appendChild(renderer.domElement);

            // 4. Geometry
            // @ts-ignore
            const geometry = new THREE.SphereGeometry(500, 60, 40);
            geometry.scale(-1, 1, 1);

            // 5. Texture
            const textureLoader = new THREE.TextureLoader();
            // @ts-ignore
            textureLoader.load("/images/arena_360.png", (texture) => {
                texture.colorSpace = THREE.SRGBColorSpace;

                // Enhance quality
                texture.minFilter = THREE.LinearFilter;
                texture.magFilter = THREE.LinearFilter;
                texture.generateMipmaps = false;
                texture.generateMipmaps = false;
                // @ts-ignore
                const maxAnisotropy = renderer.capabilities.getMaxAnisotropy();
                // @ts-ignore
                texture.anisotropy = maxAnisotropy;

                const material = new THREE.MeshBasicMaterial({ map: texture });
                sphere = new THREE.Mesh(geometry, material);
                scene.add(sphere);
            });

            // 6. Controls
            // @ts-ignore
            controls = new OrbitControls(camera, renderer.domElement);
            controls.enableZoom = false;
            controls.enablePan = false;
            controls.enableDamping = true;
            controls.dampingFactor = 0.05;
            controls.autoRotate = true;
            controls.autoRotateSpeed = 0.5;
            controls.rotateSpeed = -0.5;

            // 7. Animation
            const animate = () => {
                rafId = requestAnimationFrame(animate);
                controls.update();
                renderer.render(scene, camera);
            };
            animate();

            // 8. Resize
            const handleResize = () => {
                if (!container || !camera || !renderer) return;
                const w = container.clientWidth;
                const h = container.clientHeight;
                camera.aspect = w / h;
                camera.updateProjectionMatrix();
                renderer.setSize(w, h);
            };
            window.addEventListener("resize", handleResize);

            // Define cleanup function
            cleanupFn = () => {
                window.removeEventListener("resize", handleResize);
                cancelAnimationFrame(rafId);
                if (renderer) {
                    renderer.dispose();
                    if (sphere) {
                        sphere.geometry.dispose();
                        sphere.material.dispose();
                    }
                }
            };
        };

        init();

        return () => {
            if (cleanupFn) cleanupFn();
        };
    });
</script>

<div class="panorama-wrapper" bind:this={container}>
    <div class="interaction-hint">
        <span class="icon">↺</span> 360° VIEW
    </div>
</div>

<style>
    .panorama-wrapper {
        width: 100%;
        height: 100%;
        position: relative;
        cursor: grab;
    }
    .panorama-wrapper:active {
        cursor: grabbing;
    }

    .interaction-hint {
        position: absolute;
        bottom: 140px; /* Above title */
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0, 0, 0, 0.5);
        backdrop-filter: blur(4px);
        border: 1px solid rgba(255, 255, 255, 0.2);
        padding: 6px 12px;
        border-radius: 20px;
        color: white;
        font-size: 11px;
        font-weight: 700;
        font-family: monospace;
        pointer-events: none;
        z-index: 10;
        display: flex;
        align-items: center;
        gap: 6px;
    }

    .icon {
        font-size: 14px;
        animation: spin 3s linear infinite;
        display: inline-block;
    }

    @keyframes spin {
        from {
            transform: rotate(0deg);
        }
        to {
            transform: rotate(-360deg);
        }
    }
</style>
