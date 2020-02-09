# EUFS Autonomous Datasets

**This repo has moved to gitlab - https://gitlab.com/eufs/datasets**

A collection of datasets gathered by the EUFS team whilist developing an autonomous formula student car.

All datasets come in a rosbag format and can be used directly in ROS. Otherwise, raw data can be extracted using [bag_tools](http://wiki.ros.org/bag_tools).

Repository also includes custom ROS messages featured in the datasets.

### Contents
1. [Sanderson car park](#sanderson)
2. [FS-AI 2018 Dataset](#fsai)

## Sanderson car park <a name="sanderson"></a>

Collected in a car park using a testbed with sensors attached at the appropriate positions.

- single lap - **[download](https://uoe-my.sharepoint.com/:u:/g/personal/s1404165_ed_ac_uk/EYKAOOqsEp9AsnacX1--dbsBv0vKKb9H6utxDwXAv0yRfA?e=qkFXlQ)**

- double lap - **[download](https://uoe-my.sharepoint.com/:u:/g/personal/s1404165_ed_ac_uk/Eetn4wQAI2FCtJAB-bvh49kBIEx2YlXofUp-S4ni1IAZuA?e=a2ZX4a)**

### Topics

| Topic | Type | Freq | Source | Notes |
| ----- | ---- | ---- | ------ | ----- |
| `/imu/data` | `sensor_msgs/Imu` | 10Hz | X-Sens MTi-G | 6 DoF IMU Data from X-Sens MTi-G INS with embedded Kalman Filter. Includes orientation data. | 
| `/imu/mag` | `sensor_msgs/MagneticField` | 10Hz | X-Sens MTi-G | |
| `/fix` | `sensor_msgs/NavSatFix` | 10Hz | X-Sens MTi-G | Unreliable as far as our tests go. |
| `/velodyne_points` | `sensor_msgs/PointCloud2` | 10Hz | Velodyne VLP-16 | Full 360 degree data. Testbed must be filtered out. | 
| `/left/image_rect_color/compressed ` | `sensor_msgs/CompressedImage ` | 30Hz | ZED Camera | 672 x 376 resolution |
| `/right/image_rect_color/compressed ` | `sensor_msgs/CompressedImage ` | 30Hz | ZED Camera | 672 x 376 resolution |
| `/odom` | `nav_msgs/Odometry` | 30Hz | ZED Camera | Visual odometry. Very good!! |
| `voxel_frid/output` | `sensor_msgs/PointCloud2` | 30Hz | ZED Camera | Slightly filtered 3D pointcloud from the stereo camera. |
| `/tf` | `tf2_msgs/TFMessage ` | 30Hz | - | Contains odom->base_link published by the ZED camera |
| `/tf_static` | `tf2_msgs/TFMessage ` | once | - | Transforms for all sensors on the car |

### Transforms [x, y, z, r, p, y]
base_frame is center of gravity aligned to the lowest position of the car.

- IMU: [0, 0, 0, 0, 0, 0]
- GPS: [0, 0, 0.82, 0, 0, 0]
- VLP-16: [1.541, 0, 0, 0, 1.5\*PI/180.0, 0]
- ZED camera: [0.2, 0, 0.471, 0, 0, 0]

Also embedded in `/tf_static`

### Notes
- IMU data is a bit unusable due to the low output frequency.
- Test ground was not flat.

## FS-AI 2018 Dataset<a name="fsai"></a>

Collected at FSUK 2018 using the ADS-DV vehicle provided by the IMechE. This should be a representative of sensory input from a mule vehicle in the DDT class of future FSUK competitions.

**[download](https://uoe-my.sharepoint.com/:u:/g/personal/s1404165_ed_ac_uk/EaLldW-JfcpOrs0Y8nqG7RgBqoPtNiQ98_Ce0DuccvlM5w?e=7nFPUY)**

### Topics

| Topic | Type | Freq | Source | Notes |
| ----- | ---- | ---- | ------ | ----- |
| `/imu/data` | `sensor_msgs/Imu` | 167Hz | X-Sens MTi-G | 6 DoF IMU Data from X-Sens MTi-G INS with embedded Kalman Filter. Includes orientation data. | 
| `/imu/mag` | `sensor_msgs/MagneticField` | 167Hz | X-Sens MTi-G | |
| `/fix` | `sensor_msgs/NavSatFix` | 167Hz | X-Sens MTi-G | Unreliable as far as our tests go. In reality changes at 10Hz |
| `/velodyne_points` | `sensor_msgs/PointCloud2` | 10Hz | Velodyne VLP-16 | Full 360 degree data. Wheels of the car must be filtered out. |
| `/velocity` | `geometry_msgs/TwistStamped` | 167Hz | X-Sens MTi-G | Velicty from Kalman Filter of the GPS. In reality changes at 10Hz. |
| `/left/image_rect_color/compressed ` | `sensor_msgs/CompressedImage ` | 32Hz | ZED Camera | 672 x 376 resolution |
| `/right/image_rect_color/compressed ` | `sensor_msgs/CompressedImage ` | 32Hz | ZED Camera | 672 x 376 resolution |
| `/odom` | `nav_msgs/Odometry` | 32Hz | ZED Camera | Visual odometry. Useless due to bad positioning of the camera. |
| `/tf_static` | `tf2_msgs/TFMessage ` | once | - |Transforms for all sensors on the car |
| `/ros_can/state_str` | `std_msgs/String` | 100Hz | Car ECU | High-level state of the car |
| `/ros_can/wheel_speeds` | `eufsa_msgs/wheelSpeeds` | 100Hz | Car ECU | Wheel RPM and steering feedback from the car. Custom message is simply a header and 5x `float32`; can be found in repositoty.| 

### Transforms
embedded into `/tf_static`

### Notes
- Full data was not collected due to lack of time and equipment.
- Due to issues with the car, the steering feedback, in reality, is only 1Hz. Use with caution. 
- High-frame-rate camera data was not recorded due to insufficient bandwidth of the hardware on the day. In reality, it runs at 100Hz.
- Visual odometry was rendered useless because 1/3 of the camera frame was the car itself. Take note.
- GPS data has not been tested.
- In our tests, we couldn't get a good wheel odometry to work due to the bad steering feedback. It should be fixed in the future and should output at 100Hz as well.
