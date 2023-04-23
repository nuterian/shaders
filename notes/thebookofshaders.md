# The Book of Shaders
https://thebookofshaders.com

## What is a shader?
A function that gets run by each compute unit (thread) of GPU in parallel for determining how to color each pixel on the screen. GPUs, unlike CPUs which have a few big and powerful microprocessors, have several tiny microprocessors running in parallel.

Shaders are painful to work with because the shader function is *blind* and *memoryless*. All data must flow in the  same direction. It is impossible to check the result of another thread, modify the input data, or pass the outcome of a thread into another thread. Allowing thread-to-thread communications puts the integrity of the data at risk.

## Hello World
```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;

void main() {
	gl_FragColor = vec4(1.0,0.0,1.0,1.0);
}
```
- `#ifdef GL_ES … #endif` is used to initialize the global level of precision to use for float typed variables on mobile devices and browsers.
- `precision mediump float` specifies the level of precision. Lower precision means faster rendering but at the cost of quality. We can choose to set them to low (`precision lowp float`) or high (`precision highp float`)
- GLSL specs don’t guarantee that the variables will be automatically casted. Make sure to assign float values (instead of say integer values) to float variables to avoid bugs.

## Uniforms
These are input values sent from the CPU to each parallel GPU thread. The values are uniform across all threads and are set as read-only. Examples:

```glsl
uniform vec2 u_resolution;  // Canvas size (width,height)
uniform vec2 u_mouse;       // mouse position in screen pixels
uniform float u_time;       // Time in seconds since load
```

The GPU has hardware accelerated angle, trigonometric and exponential functions.

## gl_FragCoord
Just like how GLSL has a default output, i.e., `vec 4 gl_FragColor`, it has a default input, `vec4 gl_FragCoord`, which holds the pixel of screen fragment that the active thread is working on. It is called a **varying** because the value will be different from thread to thread.
