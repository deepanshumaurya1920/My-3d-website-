I've created a premium, scroll-driven 3D website for a luxury clothing store. The experience begins with a closed, elegant storefront that dramatically opens to reveal an interior collection as you scroll.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>LUXE — The Atelier</title>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400&family=Montserrat:wght@200;300;400;500&display=swap" rel="stylesheet" />
    <style>
        /* ─── RESET & BASE ─── */
        *,
        *::before,
        *::after {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Montserrat', sans-serif;
            background: #0a0a0a;
            color: #f5f0eb;
            overflow-x: hidden;
        }

        /* ─── 3D CANVAS (fixed) ─── */
        #canvas-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: 0;
            pointer-events: none;
        }

        /* ─── SCROLL SPACER ─── */
        #scroll-container {
            position: relative;
            z-index: 1;
            pointer-events: none;
        }

        #scroll-container section {
            pointer-events: auto;
        }

        .scroll-spacer {
            height: 100vh;
        }

        /* ─── UI OVERLAY (fixed) ─── */
        #ui-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: 10;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 3rem 4rem;
            transition: opacity 0.8s ease;
        }

        #ui-overlay .top-left {
            align-self: flex-start;
        }

        #ui-overlay .top-left .brand {
            font-family: 'Cormorant Garamond', serif;
            font-size: 2.2rem;
            font-weight: 300;
            letter-spacing: 0.35em;
            color: #d4af37;
            text-transform: uppercase;
            text-shadow: 0 0 40px rgba(212, 175, 55, 0.15);
        }

        #ui-overlay .top-left .brand-sub {
            font-size: 0.65rem;
            letter-spacing: 0.6em;
            color: rgba(245, 240, 235, 0.4);
            text-transform: uppercase;
            margin-top: 0.15rem;
        }

        #ui-overlay .top-right {
            align-self: flex-end;
            display: flex;
            gap: 2.5rem;
            font-size: 0.7rem;
            letter-spacing: 0.15em;
            color: rgba(245, 240, 235, 0.5);
            text-transform: uppercase;
        }

        #ui-overlay .center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            transition: opacity 0.8s ease, transform 0.8s ease;
        }

        #ui-overlay .center .main-title {
            font-family: 'Cormorant Garamond', serif;
            font-size: 4rem;
            font-weight: 300;
            color: #f5f0eb;
            letter-spacing: 0.2em;
            text-shadow: 0 0 60px rgba(0, 0, 0, 0.8);
            margin-bottom: 0.75rem;
        }

        #ui-overlay .center .main-title .gold {
            color: #d4af37;
        }

        #ui-overlay .center .sub-title {
            font-size: 0.75rem;
            letter-spacing: 0.5em;
            color: rgba(245, 240, 235, 0.5);
            text-transform: uppercase;
        }

        #ui-overlay .center .scroll-hint {
            margin-top: 3rem;
            font-size: 0.6rem;
            letter-spacing: 0.3em;
            color: rgba(245, 240, 235, 0.3);
            text-transform: uppercase;
            animation: pulseHint 2.4s ease-in-out infinite;
        }

        #ui-overlay .center .scroll-hint .arrow {
            display: block;
            margin: 0.75rem auto 0;
            width: 1.2rem;
            height: 1.2rem;
            border-right: 1px solid rgba(212, 175, 55, 0.4);
            border-bottom: 1px solid rgba(212, 175, 55, 0.4);
            transform: rotate(45deg);
            animation: arrowBounce 2.4s ease-in-out infinite;
        }

        #ui-overlay .bottom-center {
            align-self: center;
            font-size: 0.55rem;
            letter-spacing: 0.4em;
            color: rgba(245, 240, 235, 0.15);
            text-transform: uppercase;
            padding-bottom: 0.5rem;
        }

        @keyframes pulseHint {
            0%,
            100% {
                opacity: 0.3;
            }
            50% {
                opacity: 0.7;
            }
        }

        @keyframes arrowBounce {
            0%,
            100% {
                transform: rotate(45deg) translate(0, 0);
            }
            50% {
                transform: rotate(45deg) translate(4px, 4px);
            }
        }

        /* ─── CONTENT SECTIONS ─── */
        #content-sections {
            position: relative;
            z-index: 2;
            pointer-events: none;
        }

        #content-sections .section {
            pointer-events: auto;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 6rem 4rem;
            max-width: 1400px;
            margin: 0 auto;
        }

        #content-sections .section .label {
            font-size: 0.6rem;
            letter-spacing: 0.5em;
            color: #d4af37;
            text-transform: uppercase;
            margin-bottom: 1rem;
            font-weight: 400;
        }

        #content-sections .section h2 {
            font-family: 'Cormorant Garamond', serif;
            font-size: 3.5rem;
            font-weight: 300;
            color: #f5f0eb;
            letter-spacing: 0.05em;
            line-height: 1.25;
            max-width: 700px;
        }

        #content-sections .section h2 .gold {
            color: #d4af37;
        }

        #content-sections .section p {
            font-size: 0.95rem;
            font-weight: 200;
            color: rgba(245, 240, 235, 0.6);
            line-height: 1.9;
            max-width: 520px;
            margin-top: 1.5rem;
            letter-spacing: 0.02em;
        }

        #content-sections .section .button-outline {
            display: inline-block;
            margin-top: 2.5rem;
            padding: 0.9rem 2.8rem;
            border: 1px solid rgba(212, 175, 55, 0.4);
            background: transparent;
            color: #d4af37;
            font-family: 'Montserrat', sans-serif;
            font-size: 0.6rem;
            font-weight: 400;
            letter-spacing: 0.35em;
            text-transform: uppercase;
            text-decoration: none;
            transition: all 0.5s ease;
            cursor: pointer;
        }

        #content-sections .section .button-outline:hover {
            background: rgba(212, 175, 55, 0.1);
            border-color: #d4af37;
        }

        /* Right-aligned sections */
        #content-sections .section.right {
            align-items: flex-end;
            text-align: right;
        }

        #content-sections .section.right h2,
        #content-sections .section.right p {
            max-width: 600px;
        }

        #content-sections .section.right p {
            margin-left: auto;
        }

        /* ─── RESPONSIVE ─── */
        @media (max-width: 1024px) {
            #ui-overlay {
                padding: 2rem 2.5rem;
            }
            #ui-overlay .center .main-title {
                font-size: 3rem;
            }
            #content-sections .section {
                padding: 4rem 2.5rem;
            }
            #content-sections .section h2 {
                font-size: 2.8rem;
            }
        }

        @media (max-width: 768px) {
            #ui-overlay {
                padding: 1.5rem 1.5rem;
            }
            #ui-overlay .top-left .brand {
                font-size: 1.4rem;
            }
            #ui-overlay .top-right {
                display: none;
            }
            #ui-overlay .center .main-title {
                font-size: 2.2rem;
            }
            #ui-overlay .center .sub-title {
                font-size: 0.6rem;
            }
            #ui-overlay .center .scroll-hint {
                margin-top: 2rem;
            }
            #content-sections .section {
                padding: 3rem 1.5rem;
                min-height: 80vh;
            }
            #content-sections .section h2 {
                font-size: 2rem;
            }
            #content-sections .section p {
                font-size: 0.85rem;
            }
            #content-sections .section .button-outline {
                padding: 0.7rem 2rem;
                font-size: 0.5rem;
            }
        }

        @media (max-width: 480px) {
            #ui-overlay .center .main-title {
                font-size: 1.6rem;
                letter-spacing: 0.1em;
            }
            #ui-overlay .center .sub-title {
                font-size: 0.5rem;
                letter-spacing: 0.3em;
            }
            #content-sections .section h2 {
                font-size: 1.5rem;
            }
            #content-sections .section {
                padding: 2rem 1.2rem;
                min-height: 70vh;
            }
            #content-sections .section p {
                font-size: 0.8rem;
            }
        }

        /* ─── PROGRESS BAR ─── */
        #progress-bar {
            position: fixed;
            bottom: 0;
            left: 0;
            height: 2px;
            background: linear-gradient(90deg, #d4af37, #f5e6a3);
            z-index: 20;
            width: 0%;
            transition: width 0.1s linear;
            pointer-events: none;
            opacity: 0.7;
        }

        /* ─── LOADING ─── */
        #loading {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: #0a0a0a;
            z-index: 100;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: opacity 1.2s ease;
        }

        #loading.hidden {
            opacity: 0;
            pointer-events: none;
        }

        #loading .spinner {
            width: 28px;
            height: 28px;
            border: 1px solid rgba(212, 175, 55, 0.15);
            border-top-color: #d4af37;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        #loading .load-text {
            margin-top: 1.5rem;
            font-size: 0.55rem;
            letter-spacing: 0.5em;
            color: rgba(245, 240, 235, 0.3);
            text-transform: uppercase;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }
    </style>
</head>
<body>

    <!-- ─── LOADING ─── -->
    <div id="loading">
        <div class="spinner"></div>
        <div class="load-text">LUXE</div>
    </div>

    <!-- ─── 3D CANVAS ─── -->
    <div id="canvas-container"></div>

    <!-- ─── UI OVERLAY ─── -->
    <div id="ui-overlay">
        <div class="top-left">
            <div class="brand">LUXE</div>
            <div class="brand-sub">The Atelier</div>
        </div>
        <div class="top-right">
            <span>Collection</span>
            <span>Boutique</span>
            <span>Atelier</span>
        </div>
        <div class="center" id="center-ui">
            <div class="main-title">L'<span class="gold">Art</span> de la Mode</div>
            <div class="sub-title">Haute Couture · Paris</div>
            <div class="scroll-hint">
                Défilez pour découvrir
                <span class="arrow"></span>
            </div>
        </div>
        <div class="bottom-center">— Since 1924 —</div>
    </div>

    <!-- ─── PROGRESS BAR ─── -->
    <div id="progress-bar"></div>

    <!-- ─── SCROLLABLE CONTENT ─── -->
    <div id="scroll-container">
        <!-- spacer so the scene has room to open -->
        <div class="scroll-spacer"></div>

        <div id="content-sections">
            <section class="section">
                <span class="label">L'ouverture</span>
                <h2>Where luxury<br />meets <span class="gold">artistry</span></h2>
                <p>
                    Step into our atelier — a sanctuary of impeccable tailoring and
                    timeless elegance. Each piece is sculpted by master artisans in
                    the heart of Paris.
                </p>
                <a class="button-outline">Explore la Collection</a>
            </section>

            <section class="section right">
                <span class="label">Savoir-Faire</span>
                <h2>The <span class="gold">golden</span> thread<br />of tradition</h2>
                <p>
                    For over a century, LUXE has defined the art of dressing well.
                    Our archives speak in silk, cashmere, and the quiet language of
                    refinement.
                </p>
                <a class="button-outline">Notre Histoire</a>
            </section>

            <section class="section">
                <span class="label">La Couture</span>
                <h2>Fall — Winter <span class="gold">2025</span></h2>
                <p>
                    A dialogue between structure and fluidity. sculptural silhouettes
                    meet whisper-soft fabrics in a palette drawn from the Parisian
                    twilight.
                </p>
                <a class="button-outline">Voir la Collection</a>
            </section>

            <section class="section right" style="min-height:80vh;">
                <span class="label">L'Atelier</span>
                <h2>Visit our <span class="gold">salon</span><br />on Rue Saint-Honoré</h2>
                <p>
                    By appointment only. Experience the art of bespoke tailoring
                    in an environment of quiet luxury.
                </p>
                <a class="button-outline">Réserver</a>
            </section>

            <div style="height:30vh;"></div>
        </div>
    </div>

    <!-- ─── THREE.JS ─── -->
    <script type="importmap">
        {
            "imports": {
                "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
                "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/"
            }
        }
    </script>

    <script type="module">
        import * as THREE from 'three';
        import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        //   DOM REFS
        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        const container = document.getElementById('canvas-container');
        const centerUI = document.getElementById('center-ui');
        const progressBar = document.getElementById('progress-bar');
        const loadingEl = document.getElementById('loading');

        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        //   THREE.JS SETUP
        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x0a0a0a);
        scene.fog = new THREE.Fog(0x0a0a0a, 18, 32);

        const camera = new THREE.PerspectiveCamera(38, container.clientWidth / container.clientHeight, 0.1, 60);
        camera.position.set(0, 1.8, 12);
        camera.lookAt(0, 0.6, 0);

        const renderer = new THREE.WebGLRenderer({
            antialias: true,
            alpha: false,
            powerPreference: 'high-performance',
        });
        renderer.setSize(container.clientWidth, container.clientHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 1.2;
        renderer.outputColorSpace = THREE.SRGBColorSpace;
        container.appendChild(renderer.domElement);

        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        //   LIGHTS
        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        const ambientLight = new THREE.AmbientLight(0x403a35, 0.35);
        scene.add(ambientLight);

        const mainLight = new THREE.DirectionalLight(0xffeedd, 2.2);
        mainLight.position.set(2, 8, 4);
        mainLight.castShadow = true;
        mainLight.shadow.mapSize.width = 2048;
        mainLight.shadow.mapSize.height = 2048;
        mainLight.shadow.camera.near = 0.5;
        mainLight.shadow.camera.far = 20;
        mainLight.shadow.camera.left = -8;
        mainLight.shadow.camera.right = 8;
        mainLight.shadow.camera.top = 8;
        mainLight.shadow.camera.bottom = -8;
        mainLight.shadow.bias = -0.001;
        scene.add(mainLight);

        const fillLight = new THREE.DirectionalLight(0x8899bb, 0.6);
        fillLight.position.set(-3, 2, -4);
        scene.add(fillLight);

        const backLight = new THREE.DirectionalLight(0xd4af37, 0.4);
        backLight.position.set(0, 1, -5);
        scene.add(backLight);

        const rimLight = new THREE.DirectionalLight(0xffeedd, 0.8);
        rimLight.position.set(-2, 3, 6);
        scene.add(rimLight);

        // Warm accent light (golden)
        const spotLight = new THREE.SpotLight(0xd4af37, 6, 14, Math.PI / 8, 0.6, 1.5);
        spotLight.position.set(0, 5, 0.5);
        spotLight.target.position.set(0, 0.4, 0);
        spotLight.castShadow = true;
        spotLight.shadow.mapSize.width = 1024;
        spotLight.shadow.mapSize.height = 1024;
        scene.add(spotLight);
        scene.add(spotLight.target);

        // secondary spot
        const spotLight2 = new THREE.SpotLight(0xf5e6a3, 2.5, 12, Math.PI / 10, 0.5, 1.2);
        spotLight2.position.set(1.8, 4, 1.2);
        spotLight2.target.position.set(0.6, 0.2, 0);
        scene.add(spotLight2);
        scene.add(spotLight2.target);

        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
        //   MATERIALS
        // ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        const goldMat = new THREE.MeshStandardMaterial({
            color: 0xd4af37,
            metalness: 0.8,
            roughness: 0.25,
            envMapIntensity: 1.4,
        });

        const darkGoldMat = new THREE.MeshStandardMaterial({
            color: 0xbf9a2a,
            metalness: 0.7,
            roughness: 0.35,
            envMapIntensity: 1.2,
        });

        const marbleMat = new THREE.MeshStandardMaterial({
            color: 0x1a1816,
            metalness: 0.05,
            roughness: 0.85,
        });

        const floorMat = new THREE.MeshStandardMaterial({
            color: 0x1a1816,
            metalness: 0.4,
            roughness: 0.55,
            envMapIntensity: 0.6,
        });

        const wallMat = new THREE.MeshStandardMaterial({
            color: 0x0d0d0d,
            metalness: 0.1,
            roughness: 0.7,
            side: THREE.DoubleSide,
        });

        const wallInteriorMat = new THREE.MeshStandardMaterial({
            color: 0x1a1816,
            metalness: 0.05,
            roughness: 0.8,
            side: THREE.DoubleSide,
        });

        const fabricMat = new THREE.MeshStandardMaterial({
            color: 0x2a2520,
            metalness: 0.0,
            roughness: 0.95,
        });

        const silkMat = new THREE.MeshStandardMaterial({
  
