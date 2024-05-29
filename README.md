
# Lidar Camera Fusion ROS1
This package contains the nodes to  fuse the pointcloud with camera stream. For the experiment VLP 32 and FLIR Camera is used on ROS Melodic running on Intel NUC. For the fusion we require the extrinsic parameters between lidar and camera. To obtain them one can use [target-based-method](https://github.com/ankitdhall/lidar_camera_calibration) or [learning-based-method](https://github.com/OpenCalib/CalibAnything).

## Requisites
- ROS [Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu) or [Noetic](https://wiki.ros.org/noetic/Installation/Ubuntu)
- [PCL](https://pointclouds.org/) (tested with pcl 1.8)
- [Armadillo](http://arma.sourceforge.net/download.html) (11.0.1 or higher, tested with 11.1.1)
 
## Steps
1. Clone the repository
```
    cd ~/catkin_ws/src
    git clone https://github.com/codeck313/lidar_camera_fusion_ros1
    cd ..
    catkin_make --only-pkg-with-deps lidar_camera_fusion_ros1
```
2. Take a printout of [calibration matrix](https://wiki.ros.org/camera_calibration/Tutorials/MonocularCalibration?action=AttachFile&do=view&target=check-108.pdf). Then to find the intrinsic of your camera run the following command (make sure roscore is running in background).
```
rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.108 image:=<topic of the camera>
``` 

3. Obtain the extrensic parameter between lidar and camera using either of the methods [target-based-method](https://github.com/ankitdhall/lidar_camera_calibration) or [learning-based-method](https://github.com/OpenCalib/CalibAnything). Target based method provides higher accurarcy but takes time to assemble the setup. Learning based can be done immediately but provides lower accuracy in general. 

4. Change the extrensic parameter in the cfg file with the values obtained during calibration.
5. Change the point cloud topic and image topic in the fusion launch file and then launch it.
```
  roslaunch lidar_camera_fusion fusion.launch 
```


<p align='center'>
<img width="80%" src="/images/image_projected_on_pointcloud.png"/>
</p>
