# C++ Code Sample — BT Ward

### Prompt:

**Write a CPU implementation of an algorithm, then parallelize it using CPU threads or by implementing it using a compute shader on the GPU. e.g.**

Do an implementation of an AI behavior (like flocking) on the CPU (single threaded), multithreaded, and for a compute shader

### Response:

I have chosen to implement a version of the classic [Boids model by Craig Reynolds](https://www.red3d.com/cwr/boids/) as a representative example of a flocking algorithm. This is an ideal case study for parallelization because each member of the flock can be updated with a handful of simple instructions, but this task must be repeated many times to create the desired effect.

I judged my performance by the number of objects that could be included in the simulation while keeping the time for a single update less than 1/60th of a second (i.e. a 60 FPS video game). My single-threaded CPU example capped out around 2,000 boids, while my CUDA implementation achieved over 8,000 boids: at least 4x the performance!

This project is a proof of concept solely focused on performance. No method is included for rendering the simulation, and the values involved in are not verified to achieve convincing flocking visuals, but the types of calculations being run in each update are accurate and thus the performance should be representative of a properly-tuned simulation.

I have left a number of unused and unnessecary files in my repositories as artifacts of my development process, as I believe the greatest value of code samples as a recruitment tool is as a window into the development and decision-making process.

Both versions of the program require glm to build: https://glm.g-truc.net/

### Guide:

Included in this repository is both the single-threaded CPU as well as the CUDA version of my simulation. Releases for both versions can be found on the github repository for each individual variant. Also included is a planning folder which includes some of my notes and pseudocode outlines from the process and a log where I give additional insight into my decision-making at some pivotal turning points.
