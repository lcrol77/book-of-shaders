## Uniforms

So far we have seen how the GPU manages large numbers of parallel threads, each one responsible for assigning the color to a fraction of the total image. Although each parallel thread is blind to the others, we need to be able to send some inputs from the CPU to all the threads. Because of the architecture of the graphics card those inputs are going to be equal (*uniform*) to all the threads and necessarily set as *read only*. In other words, each thread receives the same data which it can read but cannot change.

These inputs are called `uniform` and come in most of the supported types: `float`, `vec2`, `vec3`, `vec4`, `mat2`, `mat3`, `mat4`, `sampler2D` and `samplerCube`. Uniforms are defined with the corresponding type at the top of the shader right after assigning the default floating point precision.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;  // Canvas size (width,height)
uniform vec2 u_mouse;       // mouse position in screen pixels
uniform float u_time;       // Time in seconds since load
```

You can picture the uniforms like little bridges between the CPU and the GPU. The names will vary from implementation to implementation but in this series of examples I’m always passing: `u_time` (time in seconds since the shader started), `u_resolution` (billboard size where the shader is being drawn) and `u_mouse` (mouse position inside the billboard in pixels). I’m following the convention of putting `u_` before the uniform name to be explicit about the nature of this variable but you will find all kinds of names for uniforms. For example [ShaderToy.com](https://www.shadertoy.com/) uses the same uniforms but with the following names:

```glsl
uniform vec3 iResolution;   // viewport resolution (in pixels)
uniform vec4 iMouse;        // mouse pixel coords. xy: current, zw: click
uniform float iTime;        // shader playback time (in seconds)
```

Enough talking, let's see the uniforms in action. In the following code we use `u_time` - the number of seconds since the shader started running - together with a sine function to animate the transition of the amount of red in the billboard.
