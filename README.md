<div align="center">
  <h1>Distributed SOLiD SLAM</h1>
  <a href=""><img src="https://img.shields.io/badge/-C++-blue?logo=cplusplus" /></a>
  <a href=""><img src="https://img.shields.io/badge/-Linux-grey?logo=linux" /></a>
  <a href=""><img src="https://badges.aleen42.com/src/docker.svg" /></a>
</div>

## What is Distributed SOLiD SLAM?
* Distributed SOLiD SLAM is a Distributed SOLiD-based LiDAR SLAM Framework, which is a modified version of [LIO-SAM](https://github.com/yeweihuang/LIO-SAM) and [DiSCo-SLAM](https://github.com/RobustFieldAutonomyLab/DiSCo-SLAM). ([Scan Context](https://github.com/gisbi-kim/scancontext.git) &rightarrow; [SOLiD](https://github.com/sparolab/solid.git))
* The information exchange between robots is made through ROS-based communication. (More detailed in [here](https://github.com/sparolab/Distributed-SOLiD-SLAM/blob/main/msg/context_info.msg)!!)
* SOLiD, which is a lightweight descriptor enables fast communication between robots.

## Dependencies
* Ubuntu 20.04
* [GTSAM (Develop version)](https://github.com/borglab/gtsam.git)
* [libnabo 1.0.7](https://github.com/norlab-ulaval/libnabo/tree/1.0.7) 

## How to use the Distributed SOLiD SLAM?
<details>
<summary>Linux</summary>
<div markdown="1">

```
    $ cd ~/catkin_ws/src
    $ git clone https://github.com/sparolab/Distributed-SOLiD-SLAM.git
    $ cd ..
    $ catkin_make
    $ source devel/setup.bash
    $ roslaunch lio_sam run.launch
    $ rosbag play (your dataset).bag
  ```

</div>
</details>

<details>
<summary>Docker</summary>
<div markdown="1">

```

$ cd ~/catkin_ws/src
$ git clone https://github.com/sparolab/Distributed-SOLiD-SLAM.git
$ docker pull cokr6901/distributed_solid_slam
$ docker run --privileged --gpus all \
-it --name distributed_solid_slam --ipc=host --shm-size=512M \
--device=/dev/video0:/dev/video0 -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
-e DISPLAY=unix$DISPLAY -v /root/.Xauthority:/root/.Xauthority \
--env="QT_X11_NO_MITSHM=1" \
-v ~/catkin_ws/src/:/home/test_ws/src -v (your dataset folder path)/:/home/test_ws/storage cokr6901/distributed_solid_slam:latest
$ cd /home/test_ws/
$ catkin_make
$ source devel/setup.bash
$ roslaunch lio_sam run.launch
$ rosbag play (your dataset).bag

```

</div>
</details>

## Parameters
<details>
<summary><a href="https://github.com/sparolab/Distributed-SOLiD-SLAM/blob/main/config/params.yaml">Extrinsic (LiDAR -> IMU) </a></summary>
<div markdown="1">
	
```

  extrinsicTrans: [0.0, 0.0, 0.0]
  extrinsicRot: [-1, 0, 0,
                  0, 1, 0,
                  0, 0, -1]
  extrinsicRPY: [0,  1, 0,
                 -1, 0, 0,
                  0, 0, 1]
		  
```
	  
</div>
</details>

<details>
<summary><a href="https://github.com/sparolab/Distributed-SOLiD-SLAM/blob/main/src/Distributed-SOLiD-SLAM/config/mapfusion.yaml">SOLiD</a></summary>
<div markdown="1">
	
	mapfusion:
	    solid:
	        knn_feature_dim: 40
	        max_range: 80
	        num_sector: 60
	        num_height: 64
	        num_nearest_matches: 50
	        num_match_candidates: 1
	        fov_up: 2.0
	        fov_down: -24.8

</div>
</details>

## Utils
<details>
<summary>Generate a multi-robot rosbag from a single-robot rosbag using a <a href="https://github.com/sparolab/Distributed-SOLiD-SLAM/blob/main/scripts/split_bag.py">Python script.</a></summary>
<div markdown="1">
	
	$ python3 split.bag
 
</div>
</details>

<details>
<summary>You can edit these lines.</summary>
<div markdown="1">
	
	topics = ['/points_raw', '/imu_raw', '/gps/fix']       # Rostopic names
	split_places = [90, 180, 290]		                  # 0(start)-90-180-290(final)
	robot_names =  ['/jackal0', '/jackal1', '/jackal2']    # Robot names (jackal0:0-90 / jackal1:90-180 / jackal2:180-290)
 
</div>
</details>

## To DO
* [ ] Save Transformed Path
* [ ] Change LiDAR-Odometry (LIO-SAM &rightarrow; FAST-LIO2)
* [ ] Add Outlier Rejection (e.g. RANSAC)

## Citation
  ```
	@article{kim2024narrowing,
	  title={Narrowing your FOV with SOLiD: Spatially Organized and Lightweight Global Descriptor for FOV-constrained LiDAR Place Recognition},
	  author={Kim, Hogyun and Choi, Jiwon and Sim, Taehu and Kim, Giseop and Cho, Younggun},
	  journal={IEEE Robotics and Automation Letters},
	  year={2024},
	  publisher={IEEE}
	}
  ```
## Contact
* Hogyun Kim (hg.kim@inha.edu)

## License
* For academic usage, the code is released under the BSD 3.0 license. For any commercial purpose, please contact the authors.

## Special Thanks
* We appreciate Prof. [Brendan Englot](https://scholar.google.com/citations?user=Nd6tX_kAAAAJ&hl=ko)'s RobustFieldAutonomyLab, particularly [Yewei Huang](https://scholar.google.com/citations?user=8g3U_tkAAAAJ&hl=ko&oi=sra), for publishing the DiSCo-SLAM.
