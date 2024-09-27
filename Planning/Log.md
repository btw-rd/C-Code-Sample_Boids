## Log

#### Problem — Linear Algebra:
In order to represent a Boid in 3d space I will use three value vectors to hold its position and heading. I will need to perform some simple linear algebra operations on these vectors such as difference, normalization, and averaging. These linear algebra functions are not present in the C++ standard library.

I could write the necessary functions myself or I could include a 3rd party library such as Eigen or GLM. On cursory research, Eigen seems to be the most common, but GLM is much touted in the realm of realtime applications and claims compatibility with the CUDA compiler I plan to use for parallelizing my program.

Seeing as there are only a handful of operations I will need, and I know I will be limited to the 3rd dimension, I think the best option for me is to implement them myself and save the potential overhead and dependency of bringing in an outside library that is more generalized than I need.

#### Problem — Arrays vs Vectors:
When drafting my Boid class I initially defined the position and heading data members as vectors, thinking that vectors are generally simpler to use. However, when I began to write my linear algebra functions that would make use of these members I began to encounter some complications. My linear algebra functions are to be written for 3d vectors only, and as such I should assert the length of the container within the function. Because performance here is one of my key values, I would vastly prefer a `static_assert` over a runtime assert, which can be done on the size of an array but not the size of a vector. Result: I will use arrays for my position and heading data members in the Boid class.

Update: As I have been filling out my linear algebra library I have increasingly encountered advantages to vectors such as being able to cheaply return by value. I will stick with arrays, but perhaps the best of both worlds would have been achieved by instead ensuring all of my linear algebra functions are dimension agnostic.

Update 2: Upon delving slightly further I have decided to use GLM instead of writing my own linear algebra functions. I verified my decision by comparing performance of my distance function to GLM's distance function using the same methods outlined below and found GLM's solution to be equal or slightly faster.

#### Problem — Distance Function Performance:
The distance operation from my linear algebra functions will be repeated many times and has the potential to be a major performance bottleneck; therefore I wanted to test several different ways of writing it. The main variance I tested is whether to define separate float variables for the difference in each dimension or to include them all in a single array. Afterwards I also tested performing the power operation on each difference with `std::powf` vs simple multiplication.

I profiled performance using the `<chrono>` library. I repeated the function many times in order to accumulate a meaningful timescale to measure. Ultimately both variants of the main issue were found to be extremely close in performance, but the array version seemed marginally faster, so I decided to use it. No difference was found with the `powf` variant.

#### CUDA Adaptations:
* the kernel needs to return void
* need to have a place to put the updated boids to not create a race
* need to be able to update the initial boids reference to use the updated boids without copying values 
- `boids` is a pointer to a C-style array of type `Boid`
- can I modify `boids` at the end of an update to contain the address of the updated boids?
* change to c-style arrays
* moved constants inside device code
