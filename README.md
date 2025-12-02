# navocr_projection

Standalone tools to project NavOCR detections to 3D world coordinates, without modifying the original NavOCR code.

## Components

### C++ Nodes (Recommended)
- **navocr_slam_cpp**: Integrated C++ node that subscribes to NavOCR detections, images, depth, and camera info to compute 3D world coordinates via rtabmap SLAM. Publishes RViz markers and saves CSV + annotated images.

## Inputs and Outputs

### C++ Node (navocr_slam_cpp)

Inputs:
- Image: `/camera/infra1/image_rect_raw` (sensor_msgs/Image)
- Depth: `/camera/depth/image_rect_raw` (sensor_msgs/Image, 16UC1 in mm)
- CameraInfo: `/camera/infra1/camera_info`
- Detections: `/navocr/detections` (vision_msgs/Detection2DArray)
- Odometry: `/odom` (nav_msgs/Odometry)
- TF: camera_infra1_optical_frame → camera_link → odom → map (via rtabmap)

Outputs:
- `/navocr/markers` (visualization_msgs/MarkerArray) - Green cubes and text labels in RViz
- CSV file: `results_cpp/detections_YYYYMMDD_HHMMSS.csv` with columns:
  - frame, bbox (x,y,w,h), confidence, timestamp (sec, nsec)
  - depth_m, camera (x,y,z), world (x,y,z), has_world_pos
- Images: `results_cpp/images/detection_XXXXXX.jpg` with drawn bounding boxes


## Install dependencies

Python deps (example):

```bash
pip install ultralytics opencv-python numpy
```

Make sure your ROS 2 workspace has cv_bridge and image_geometry installed (standard on ROS 2 Humble desktop). If using system Python, prefer a virtualenv/conda and make sure `ros2 run` resolves to the same environment.

## Build

From your workspace root (e.g., `/home/sehyeon/ros2_ws`):

```bash
colcon build --packages-select navocr_projection
source install/setup.bash
```

## Usage

### C++ Node with rtabmap SLAM (Recommended)

Full integrated system with rtabmap for accurate world coordinates:

**Terminal 1: Play rosbag with clock**
```bash
ros2 bag play ~/Downloads/rosbag2_2025_11_18-10_05_53 --clock
```

**Terminal 2: Run NavOCR detector (Python)**
```bash
cd ~/ros2_ws/src/NavOCR
conda activate navocr  # or your NavOCR environment
python3 run_navocr_ros.py
```

**Terminal 3: Launch integrated rtabmap + projection**
```bash
source ~/ros2_ws/install/setup.bash
ros2 launch navocr_projection navocr_slam_full.launch.py
```

**Terminal 4: RViz visualization (optional)**
```bash
rviz2 --ros-args -p use_sim_time:=true
```

Results:
- CSV: `results_cpp/detections_YYYYMMDD_HHMMSS.csv`
- Images: `results_cpp/images/detection_XXXXXX.jpg`
- RViz markers: `/navocr/markers` topic

