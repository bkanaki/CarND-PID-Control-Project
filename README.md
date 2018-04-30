# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---
### Reflection
In this relatively simpler project, the main goal is to implement a PID controller that takes in the cross-track error (CTE), and returns a steering angle so that a car is able to drive itself smoothly on a given track. The CTE in our scenario can be the distance between the center of the car and one of the lane lines.

The main idea with the PID controller is quite intuitive, starting with the *P* (proportional) term that is directly proportional to CTE. As one would expect, with such a naive solution, the car will definitely oscillate and thus, fall off the track very quickly. This is visualised below:

[![P](http://img.youtube.com/vi/csjbgmhC1tk/0.jpg)](http://www.youtube.com/watch?v=csjbgmhC1tk)

Thus, one must take the previous CTE into account to dampen these oscillations as quickly as possible. This is introduced by a *D* (differential) term. So, the constant multiplier for P term should be relatively lower as compared to the D term. This is visualised below:

[![PD](http://img.youtube.com/vi/iisAz_6mmXw/0.jpg)](http://www.youtube.com/watch?v=iisAz_6mmXw)

Decent enough! Without any tuning, with a high value for D multiplier (10) and low value of P multiplier (1), the car fiished the track without falling off. But, it oscillated largely at turns. 

On a side note, it is important to know that there may be some drift due to wear and tear and manufacturing limitations. If the drift is too high, then the car stabilize, but possible far away from the center, leading to high CTE throughout the track. In order to overcome this proble, an *I* (integral) term must be incorporated. Moreover, as a very large drift error would deem a car useless for driving, the multipler should be very low, of the order of 1/1000.

### Parameter Selection
As described above, the parameter selection must be done keeping in ind the effect of each of the multiplier. There are certain automatic ways to determine the parameters, like Twiddle. However, for this project, I used an empirical rulw of thumb, described on [stakoverflow](https://robotics.stackexchange.com/questions/167/what-are-good-strategies-for-tuning-pid-loops) (This link was found from Udacity discussion forums).

This method follows the intuition described above closely and tried to break them each multiplier sequentially.

Using this method, I ended up with the following parameters:

```
[Kp, Ki, Kd] = [0.8, 0.0008, 16]
```

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

