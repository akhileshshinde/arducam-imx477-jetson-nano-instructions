# arducam-imx477-jetson-nano-instructions
To create a GitHub repository with instructions on connecting the Arducam IMX477 camera to the Jetson Nano and running it, you can follow these structured steps. This guide will help you compile the necessary information for your README file.

## Steps to Connect Arducam IMX477 to Jetson Nano

### ****1. Hardware Setup****

- **Locate the Camera Connector (CSI)**: 
  - The camera connector is on the side of the Jetson Nano carrier board, opposite the GPIO pins.
  
- **Connect the Camera**:
  - Gently pull up on the plastic edges of the camera port.
  - Insert the camera ribbon cable with the silver contacts facing towards the heatsinks. Ensure it is firmly inserted into the bottom of the connector.
  - Push the plastic connector down while holding the flex cable until it is secured in place.

### ****2. Software Preparation****

- **Check Compatibility**:
  - Ensure you are using an official NVIDIA Jetson Carrier Board and that your JetPack version is supported for the IMX477 camera. You can check supported versions in the documentation.

- **Download and Install Driver**:
  - Open a terminal and execute the following commands:

    ```bash
    cd ~
    wget https://github.com/ArduCAM/MIPI_Camera/releases/download/v0.0.3/install_full.sh
    chmod +x install_full.sh
    ./install_full.sh -m imx477

    ```

### ****3. Validate Camera Connection****

- **Check if Camera is Detected**:
  - After installation, run:

    ```bash
    ls /dev/video*
    ```

  - You should see new video devices listed (e.g., `/dev/video0`).

- **Install v4l-utils**:
  - To check supported video formats, install `v4l-utils`:

    ```bash
    sudo apt-get install v4l-utils
    ```

- **List Supported Formats**:
  - Run this command to see which formats and resolutions are supported by your camera:

    ```bash
    v4l2-ctl --list-formats-ext
    ```

### ****4. Capture video****

-
    ```bash
    SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
    FRAMERATE=60 # Framerate can go from 2 to 60 for 1920x1080 mode
    gst-launch-1.0 -e nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=1920,height=1080,framerate=$FRAMERATE/1" ! nvv4l2h264enc ! h264parse ! mp4mux ! filesink location=rpi_v3_imx477_cam$SENSOR_ID.mp4

    ```

### ****5. Running basic face detection****
 
- ****Clone the given repository****:
  - 
    ```bash
    git clone https://github.com/JetsonHacksNano/CSI-Camera.git
    ```

### ****6. Then try running basic codes in that repository****

- 
    ```bash
    cd CSI-Camera/
    cd python3 face_detect.py
    ```

  
- Refer to forums or documentation for specific error messages or troubleshooting steps if problems persist.
(https://docs.arducam.com/Nvidia-Jetson-Camera/Native-Camera/Quick-Start-Guide/)
(https://github.com/jetsonhacksnano/CSI-Camera)

