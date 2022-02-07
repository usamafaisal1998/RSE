This is a project work from the Udacity Robotics Software Engineering Nanodegree. This project is a mapping and autonomous driving from a Rover simulator, as well as recognizing objects to pick up. Mapping result has been recorded in a short movie. You can find the Unity simulator here: https://s3-us-west-1.amazonaws.com/udacity-robotics/Rover+Unity+Sims/Mac_Roversim.zip.

**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook).
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands.
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./misc/rover_image.jpg
[image2]: ./calibration_images/example_grid1.jpg
[image3]: ./calibration_images/example_rock1.jpg

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.

Run the functions on my data and it appears more than 700+ images will throw index error, assuming it's the limit of how many images for the video creation. So I just recorded 100+ images to test.

RGB parameter will be fed into the color_thresh function as input and varies with the values of obstacles and rock samples. Different threshold values are being tested and chosen as the values in the process_image function.


#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result.

As the comments in code suggest, the process_image() function is taking into consideration of everything covered in the module. First need to to a perspective transform to see the map from top down, then the color threshold function is used to distinguish between navigable world and obstacles, as well as rock samples. After we have 3 different sets of worlds from top down view, we'll need to transform the view to rover view using the rover_coords function. Lastly, we'll switch the rover coordinates to world coordinates in order to update the wolrd map.

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

perception_step() function is basically the same as the above process_image() function but inherited from a rover class. In addition, we'll do a translation to polar coordinates at the end so we can use the polar coordinates values to decide our autonomous driving.

decision_step() function is more from my empirical analysis which doesn't really have a theoretical explanation. I want the rover to keep more or less straight until it slows down because of obstacles, and back off a bit until it sees a new navigable terrain. Steering is just a random choice when backing off so rover doesn't get stuck. It works OK.


#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

The challenge I face eventually is how to steer smartly to enable a fully autonomous rover without hitting obstacles and adjust every time. Seems to me that we can do better with just slowing down when navigable terrain is disappearing (< go_forward), instead we could figure out which direction the navigable terrain is disappearing and which direction the obstacles are appearing to enable smarter steering. From the simulation seems that rover needs to adjust multiple times to get out of a stuck situation, which is something I would improve.

I'm running the simulator with 1024*728 on my Mac OS. Rover is able to cover more than 80% of the map autonomously. At some point it did get stuck and manual intervention was needed, but it was one of those really weird obstacle that you couldn't back out of.
