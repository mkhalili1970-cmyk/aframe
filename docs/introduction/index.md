<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>TRPV1 & Capsaicin VR | M. Khalili</title>
    <meta name="description" content="Molecular Visualization of Pain">
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-environment-component@1.3.2/dist/aframe-environment-component.min.js"></script>
  </head>
  <body>
    <a-scene>
      <a-entity environment="preset: starry; groundColor: #050505; grid: cross"></a-entity>

      <a-light type="directional" position="1 1 1" intensity="0.8"></a-light>
      <a-light type="ambient" intensity="0.4"></a-light>

      <a-box position="0 1 -3" width="10" height="0.5" depth="10" color="#333" opacity="0.4" metalness="0.6">
        <a-text value="PHOSPHOLIPID BILAYER (CELL MEMBRANE)" position="0 -0.4 2" align="center" width="4"></a-text>
      </a-box>

      <a-entity id="trpv1-receptor" position="0 1 -3">
        
        <a-torus color="#4CC3D9" arc="360" radius="0.8" radius-tubular="0.1" position="0 1.5 0" rotation="90 0 0"></a-torus>
        
        <a-cylinder color="#00FFF2" height="2.5" radius="0.4" opacity="0.8" metalness="0.7">
          <a-text value="CENTRAL ION PORE" position="0 1.8 0" align="center" width="3" color="#00FFF2"></a-text>
        </a-cylinder>

        <a-cone color="#FFD700" height="1.2" radius-bottom="0.6" radius-top="0.2" position="0.9 0 0" opacity="0.6">
          <a-text value="VANILLOID BINDING SITE\n(S3-S4 Linker)" position="1 0.5 0" align="center" width="2.5"></a-text>
        </a-cone>

        <a-entity id="capsaicin-molecule">
          <a-sphere color="#FF4500" radius="0.15" position="3 2 1">
            <a-text value="CAPSAICIN" position="0 0.3 0" align="center" width="2" color="#FF4500"></a-text>
            <a-animation attribute="position" from="3 2 1" to="0.9 0 0" dur="4000" repeat="indefinite" easing="ease-in-out"></a-animation>
          </a-sphere>
        </a-entity>

        <a-sphere color="yellow" radius="0.05" position="0 2.5 0">
          <a-animation attribute="position" from="0 2.5 0" to="0 -2 0" dur="1500" repeat="indefinite"></a-animation>
        </a-sphere>
        <a-sphere color="white" radius="0.05" position="0.1 2.8 0.1">
          <a-animation attribute="position" from="0.1 2.8 0.1" to="0.1 -2 0.1" dur="1200" repeat="indefinite"></a-animation>
        </a-sphere>
        
        <a-text value="CATION INFLUX (Ca2+, Na+)\nPAIN SIGNAL INITIATED" position="0 -1.5 0.5" align="center" width="4" color="yellow"></a-text>
      </a-entity>

      <a-entity camera look-controls wasd-controls position="0 1.6 2">
        <a-cursor color="white"></a-cursor>
      </a-entity>

      <a-plane position="-2.5 2.5 -1.5" width="2" height="1" color="#000" opacity="0.8">
        <a-text value="TRPV1 ACTIVATION\nCapsaicin acts as an agonist,\nopening the pore for ions." align="center" width="1.8"></a-text>
      </a-plane>

    </a-scene>
  </body>
</html>
[super hands]: https://github.com/c-frame/aframe-super-hands-component
[teleportation]: https://github.com/jure/aframe-blink-controls

:runner: **Components**: Hit the ground running with A-Frame's core components
such as geometries, materials, lights, animations, models, raycasters, shadows,
positional audio, text, and controls for most major headsets. Get even further
from the hundreds of community components including [environment], [state], [particle
systems], [physics], [multiuser], [oceans], [teleportation], [super hands], and
[augmented reality].

:earth_americas: **Proven and Scalable**: A-Frame has been used by companies
such as Google, Disney, Samsung, Toyota, Ford, Chevrolet, Amnesty
International, CERN, NPR, Al Jazeera, The Washington Post, NASA. Companies such
as Google, Microsoft, Oculus, and Samsung have made contributions to A-Frame.

## Off You Go!

[Discord]: https://supermedium.com/discord

If it's your first time here, here's a plan for success for getting into
A-Frame:

1. Read through the documentation to get a grasp.
[Glitch](https://glitch.com/~aframe) is used as a recommended coding playground
and for examples.

2. [Join us on Discord][Discord] if you have any
questions, [search and ask on StackOverflow](http://stackoverflow.com/questions/ask/?tags=aframe),
and someone will try to get to you!

3. When you build something, share your project online on X with the
   `@aframevr` mention. You can also post it on the #self-promotion channel on
   [Supermedium Discord][Discord] and #a-frame channel on
   [WebXR Discord](https://discord.gg/jJxvuW97c4).

And it really helps to have a dig into the fundamentals on JavaScript and
[three.js](https://threejs.org/). Have fun!
