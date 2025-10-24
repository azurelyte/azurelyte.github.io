---
layout: default
title:  "Vox Machinae (2022)"
date:   2025-10-17 00:00:00 -0000
categories: projects
permalink: /posts/voxmachinae/
---
<style>
div.scroll-container {
  overflow: auto;
  white-space: nowrap;
}
div.scroll-container img {
  color:#ffffffac;
  padding: 10px;
}
</style>
# Vox Machinae
---
<iframe width="100%" height="400" src="https://www.youtube.com/embed/4qcfpOElmII?si=uWkf8ztunnWONbO4&autoplay=1&mute=1&loop=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Vox Machinae is a 80's inspired virtual reality multiplayer mech shooter that features a single player campaign and weighty combat.

Contributions: Gameplay and Principal programmer in a small indie team of 6. Worked across disciplines to design and program gameplay features, engine tools, and pipelines. Developed, reworked, and shipped various game modes.

Studio: <a href="http://www.space-bullet.com/">Space Bullet Dynamics Corporation</a>

Platform: <a href="https://store.steampowered.com/app/334540/Vox_Machinae/">Steam PC/VR</a>, <a href="https://www.meta.com/experiences/vox-machinae/1968697563211639/?srsltid=AfmBOooLgfds1kYONw0X2esI_7243vFpVbCz2X-Xe7dD8hLGoepr3Dz6">Meta Quest</a>

Reception: <a href = "https://www.metacritic.com/game/vox-machinae/">Metacritic</a> 8.5 User Score,
<a href="https://www.pcgamer.com/vr-mech-game-vox-machinae/">PC Gamer</a> (2019) - *<i>"The Best mech action game on PC right now"</i>*,
<a href="https://www.impulsegamer.com/vox-machinae-quest-2-review/">Impulse Gamer</a> (2022) - 4.5/5

Time spent on project: 04/2019 - 06/2024, 5 years 2 months.

Engine & Tools: Unity, custom PhysX implementation, proprietary scripting system, Wwise.

# Summary of Contributions

In the early access days of Vox Machinae I was the gameplay programmer responsible for scripting new game modes and prototyping new gameplay centric features. My job initially was to iterate on these new modes and features to get them into a fun and shippable state. Other work would also include working on the build and texture pipelines of the studio. Some of the software we used was severely out of date (think soft image/xsi, which was used for cgi on the original lord of the rings trilogy) and many of our rendering techniques required specialized texture packing to perform well on all platforms.

While these responsibilities always remained a part of my work, I would also inherit the responsibility of performance optimization and porting to platforms other than PC/VR. This also included lower level engine programming and interoperability between C/C++ and C#. In some cases re-writes of native C libraries were required for Meta Quest(Android) and Playstation VR.

In the final two years of my tenure after the full release of Vox Machinae the lead programmer/co-owner would gradually step back from the project. At which point it became my duty to handle all code and technical related issues until the end of the games development life cycle at the end of 2024. During this period of time as the lead/principal programmer I successfully shipped 4 major content updates amongst many smaller releases.

<h3>Gallery</h3>
<div class="scroll-container">
  <img src="/assets/images/voxmachinae/vm0.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm1.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm2.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm3.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm4.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm5.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm6.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm7.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm8.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm9.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm10.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm11.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm12.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm13.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm14.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm15.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm16.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm17.jpg" alt="">
  <img src="/assets/images/voxmachinae/vm18.jpg" alt="">
</div>

## Game Mode - Convoy

Convoy was the first multiplayer mode that I developed. The vision for this mode was to create a repeatable escort style co-operative mission that could be played on **any map**. 

First and foremost, the fundamentals of the mode needed to be created. Primarily, the issue of pathfinding an interesting route through any given level.
Vox's existing pathfinding solution required a layer on top which allowed for myself and any other designers a way to markup and generate interesting convoy routing
through a level.

This would take the form of nodes placed in worldspace that were then read into a directional graph structure. Nodes were marked up as *objectives*, *points of interest*, *goal/end*, etc..
and given one or more connections to neighbouring nodes. These connections were uni-directional such that when all this information is fed into a modified <a href="https://en.wikipedia.org/wiki/Breadth-first_search">BFS</a> /
<a href="https://en.wikipedia.org/wiki/A*_search_algorithm">A*</a> algorithm, a sensible path through the level is created.

<img src="/assets/images/voxmachinae/voxm_pathfinder.gif" alt="">

## Designing and Creating - The Aftershock (Mortar)

The mortar as a weapon was designed to fit the fantasy of long range artillery and provide a large damage payoff in exchange for its long travel time and relative inaccuracy. In designing this weapon, there were a few primary concerns.

### Player feedback and understanding when facing this weapon.

This is in the same realm of philosophy as first-person shooters, where the game indicates where damage originates from to eliminate player confusion. In Vox, there are several ways we indicate where damage comes from. Primarily, we use spatial audio sources using impact locations to aid with this. We also simulate the impact in the cockpit with visual elements shaking in relation to the strength and location of a hit. The mortar has the addition of both its projectiles appearing on the player’s radar and a slowly traversed firing arc that is both low enough to be within the player's periphery and gives ample time for the player to access and react to the incoming threat.

### Coding/Netcode

In order to facilitate the above behaviour, the Aftershock needed specialized firing/arcing logic. We use our own proprietary networking solution for complete control over how we trigger this to occur over the network. Due to the nature of network programming and the latency associated with it, I had to get creative. There is only one thing that is known when a projectile is fired in Vox. That is, the target position of the reticle of the person who fired it.

The solution was to use a fixed-size sphere to draw a curve from the relative start point on the shooter to the target reticle location. In fact, given a sphere of any size and two points whose distance is less than the diameter of the sphere, there exists a curve along the surface of the sphere which connects the two points. This is the solution space upon which there are an infinite number of answers, but where the correct one is the curve that is aligned to the worldspace up axis.

In this video, you can see how the behaviour of the projectile changes based on the distance. It has a lower trajectory when the target position is near, keeping the projectile visible to receiving players. At longer distances, the trajectory becomes much more exaggerated, giving it a higher flight time and a longer period of time for players to see the mortar shells on the radar.

<iframe width="100%" height="400" src="https://www.youtube.com/embed/6taoPtc4JaQ?si=Z9DpUgLCUAdGibfj&autoplay=0&mute=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Engine - Unity PhysX Replacement

The largest and most challenging task I was ever given was to replace the physics engine in Unity with our own version of PhysX compiled from Nvidia’s source code with a custom shader. This was needed because Unity made one very specific thread joining call that blocks the main render thread, causing a frame stutter when targeting the high frame rates that Vox needed to hit. (90fps on PCVR and 72 on Meta Quest) Frame stutters are **intolerable** in VR games, as even micro stutters that are not easily perceptible will, over time, induce motion sickness in players.

This task required:

- Compiling PhysX from source.
- Writing a custom physics shader to handle physics body collisions.
- Write code to handle non-blocking reading and writing to the PhysX scene.
- Ensure that the physics code operates completely independently of the main thread, yet synchronizes with the main loop.
- Write tools to convert existing physics rigs to PhsyX structures at runtime.
- Tweak settings to mimic Unity's PhysX implementation or better.

## Pipeline - Textures

One of the main blockers to launching Vox on Meta Quest was getting the complete packaged size of the game below the 4-gigabyte limit for the companion extension file. In other words, every asset needed to fit within this compressed file. In order to do that, we needed to cut down on the size of the main offending type of asset. Textures.

It was my responsibility to find a way to super compress the textures and index them such that the textures could be efficiently streamed into memory. We did this in a few ways.

- Identify the desired texture format in memory. In our case, this was typically <a href="https://learn.microsoft.com/en-us/windows/win32/direct3d11/bc7-format">BC7</a> or <a href="https://wikis.khronos.org/opengl/ASTC_Texture_Compression">ASTC4x4</a>.
- Find a way to super-compress these formats. Standard Unity projects will use their own <a href="https://unity.com/blog/engine-platform/crunch-compression-of-etc-textures">crunch compression</a>. This was insufficient for us as the files would not be bundled into an obb and, at the time, Unity did not provide a solution to this. Instead, we used the <a href="https://github.com/BinomialLLC/basis_universal">basis universal</a> format, which I would highly recommend looking at if you need to bring your game size down.
- Added a lookup hash table in the header of the extension file to enable fast O(n) lookups of textures.

I then created a custom orchestrator to push textures and models through a complex pipeline that would output these textures to a repository for each target platform.

