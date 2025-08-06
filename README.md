<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Sajid Mahmud Sayan — 3D Portfolio</title>
  <style>
    /* Full-bleed canvas */
    html,body { height:100%; margin:0; background:#05060a; font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial; color:#e6fbff; overflow:hidden; }
    #container { position:relative; width:100%; height:100vh; display:flex; align-items:center; justify-content:center; }
    /* UI overlay */
    .overlay {
      position:absolute; top:40px; left:40px; z-index:5;
      background:linear-gradient(135deg, rgba(255,255,255,0.02), rgba(0,230,255,0.02));
      border-radius:12px; padding:18px 22px; backdrop-filter: blur(6px);
      box-shadow: 0 10px 40px rgba(0,0,0,0.6);
      border:1px solid rgba(0,230,255,0.06);
    }
    h1 { margin:0; font-size:28px; letter-spacing:1px; color:#00e6ff; text-shadow: 0 8px 30px rgba(0,230,255,0.08); }
    .sub { margin-top:6px; color:rgba(230,251,255,0.85); font-size:14px }
    .cta { margin-top:12px; display:flex; gap:10px; }
    .btn { padding:8px 12px; border-radius:8px; font-weight:600; text-decoration:none; border:1px solid rgba(255,255,255,0.04); }
    .btn-ghost { background:transparent; color:#e6fbff; }
    .btn-neon { background:linear-gradient(90deg,#00e6ff22,#ff6bb822); color:#fff; box-shadow: 0 8px 30px rgba(0,230,255,0.06); border: 1px solid rgba(0,230,255,0.08); }
    /* Right info card */
    .card {
      position:absolute; right:40px; bottom:40px; z-index:5;
      background:linear-gradient(180deg, rgba(0,0,0,0.35), rgba(255,255,255,0.02));
      border-radius:14px; padding:18px; width:320px; backdrop-filter: blur(6px);
      border:1px solid rgba(255,255,255,0.03);
    }
    .lang { display:flex; gap:8px; flex-wrap:wrap; margin-top:8px; }
    .lang span { background:rgba(255,255,255,0.03); padding:6px 8px; border-radius:8px; font-size:13px }
    /* typed subtitle */
    .typed { font-weight:700; font-size:20px; color:#bffaff; text-shadow: 0 6px 30px rgba(0,230,255,0.06); }
    /* small responsive */
    @media (max-width:900px) {
      .overlay { left:20px; top:20px }
      .card { display:none }
    }
  </style>
</head>
<body>
  <div id="container"></div>

  <div class="overlay">
    <h1>🚀 Sajid Mahmud Sayan</h1>
    <div class="sub">Frontend Developer & High-UI Designer — pixel-perfect & interactive animations</div>
    <div style="height:8px"></div>
    <div class="typed" id="typed">Building beautiful UIs...</div>
    <div class="cta">
      <a class="btn btn-neon" href="https://github.com/sajidsayan" target="_blank">GitHub</a>
      <a class="btn btn-ghost" href="mailto:sajidsayan909@gmail.com">Email</a>
    </div>
  </div>

  <div class="card">
    <div style="display:flex; justify-content:space-between; align-items:center;">
      <div><strong>Skills</strong><br/><span style="font-size:13px;color:rgba(230,251,255,0.7)">Front-end, UI, Animations</span></div>
      <div style="font-size:24px; color:#00e6ff">✨</div>
    </div>
    <div class="lang">
      <span>HTML</span><span>CSS</span><span>JavaScript</span><span>Tailwind</span><span>Three.js</span><span>React</span>
    </div>
  </div>

  <!-- Three.js -->
  <script src="https://cdn.jsdelivr.net/npm/three@0.156.0/build/three.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.156.0/examples/js/controls/OrbitControls.js"></script>

  <script>
    // typed effect (simple)
    const phrases = ["Crafting high-UI frontends.", "3D-like visuals & parallax.", "Pixel-perfect interactions.", "Let's build something epic."];
    let idx = 0, p = 0, txt = '', forward = true;
    const el = document.getElementById('typed');
    function tick() {
      const full = phrases[idx];
      if (forward) { txt = full.slice(0, p++); if (p > full.length) { forward=false; setTimeout(tick, 900); return; } }
      else { txt = full.slice(0, p--); if (p < 0) { forward=true; idx=(idx+1)%phrases.length; p=0; } }
      el.textContent = txt;
      setTimeout(tick, forward ? 60 : 30);
    }
    tick();

    // three.js scene
    const container = document.getElementById('container');
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(45, innerWidth/innerHeight, 0.1, 1000);
    camera.position.set(0, 0, 60);

    const renderer = new THREE.WebGLRenderer({ antialias:true, alpha:true });
    renderer.setSize(innerWidth, innerHeight);
    renderer.setPixelRatio(window.devicePixelRatio ? window.devicePixelRatio : 1);
    container.appendChild(renderer.domElement);

    // particle background
    const particlesGeo = new THREE.BufferGeometry();
    const count = 1600;
    const positions = new Float32Array(count * 3);
    for (let i=0;i<count;i++){
      positions[i*3+0] = (Math.random()-0.5) * 400;
      positions[i*3+1] = (Math.random()-0.5) * 200;
      positions[i*3+2] = (Math.random()-0.5) * 400;
    }
    particlesGeo.setAttribute('position', new THREE.BufferAttribute(positions,3));
    const particlesMat = new THREE.PointsMaterial({ size: 0.9, color: 0x00e6ff, transparent:true, opacity:0.14 });
    const particles = new THREE.Points(particlesGeo, particlesMat);
    scene.add(particles);

    // 3D text wireframe (requires font loader — fallback: torus + extrude-like shape)
    // We'll create a fancy rotating torus group as name stand-in (fast & lightweight)
    const group = new THREE.Group();
    const torusMaterial = new THREE.MeshStandardMaterial({ color:0x00e6ff, metalness:0.7, roughness:0.2, emissive:0x002233, emissiveIntensity:0.6 });
    for (let i=0;i<6;i++){
      const t = new THREE.Mesh(new THREE.TorusGeometry(10 - i*1.4, 0.6 + i*0.1, 16, 120), torusMaterial);
      t.rotation.x = Math.random()*0.6;
      t.rotation.y = Math.random()*0.6;
      t.rotation.z = Math.random()*0.6;
      t.scale.setScalar(1 - i*0.08);
      group.add(t);
    }
    group.position.set(0,0,0);
    scene.add(group);

    // lights
    const amb = new THREE.AmbientLight(0xffffff, 0.15);
    scene.add(amb);
    const pLight = new THREE.PointLight(0x00e6ff, 1.0, 200);
    pLight.position.set(50,30,50);
    scene.add(pLight);

    // controls (subtle)
    const controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enablePan = false;
    controls.enableZoom = false;
    controls.autoRotate = true;
    controls.autoRotateSpeed = 0.18;
    controls.enableDamping = true;
    controls.dampingFactor = 0.08;

    // handle resize
    window.addEventListener('resize', ()=>{
      camera.aspect = innerWidth/innerHeight; camera.updateProjectionMatrix();
      renderer.setSize(innerWidth, innerHeight);
    });

    // mouse parallax
    let mouseX = 0, mouseY = 0;
    document.addEventListener('mousemove', (e)=>{
      mouseX = (e.clientX - innerWidth/2) / innerWidth;
      mouseY = (e.clientY - innerHeight/2) / innerHeight;
    });

    // animate
    const clock = new THREE.Clock();
    function animate(){
      requestAnimationFrame(animate);
      const t = clock.getElapsedTime();
      // parallax
      group.rotation.y += 0.003 + mouseX*0.03;
      group.rotation.x += 0.001 + mouseY*0.02;
      particles.rotation.y += 0.0012;
      controls.update();
      renderer.render(scene, camera);
    }
    animate();
  </script>
</body>
</html>
