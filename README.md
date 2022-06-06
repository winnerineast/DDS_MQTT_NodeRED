# DDS_MQTT_NodeRED
This is a bridge between DDS and MQTT and tested with PX4(Fast DDS) and PahoMQTT for Node-RED.

## How to Setup
### Pre-requirements (just show what I'm using)
- One HP elitebook 1050 G1 with (32core 32BG with GTX1050 GPU)
- Ubuntu Linux 5.13.0-46-generic x86_64
### Step 1 - Install PX4
```
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
sudo snap install ant
```
### Step 2 - Install QgroundControl
```
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
sudo apt install libqt5gui5 -y
wget https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage
chmod +x ./QGroundControl.AppImage
```
### Step 3 - Fast DDS Installation
make sure that Java JDK 11 is installed.
make sure that Gradle is installed. It's 6.3 not 7.x.
```
git clone https://github.com/eProsima/foonathan_memory_vendor.git
cd foonathan_memory_vendor
mkdir build && cd build
cmake ..
sudo cmake --build . --target install

git clone --recursive https://github.com/eProsima/Fast-DDS.git -b v2.0.2 ~/FastDDS-2.0.2
cd ~/FastDDS-2.0.2
mkdir build
cd build
cmake -DTHIRDPARTY=ON -DSECURITY=ON -DCOMPILE_EXAMPLES=ON -DPERFORMANCE_TESTS=ON ..
make -j$(nproc --all)
sudo make install

git clone --recursive https://github.com/eProsima/Fast-DDS-Gen.git -b v1.0.4 ~/Fast-RTPS-Gen \
    && cd ~/Fast-RTPS-Gen \
    && gradle assemble \
    && sudo env "PATH=$PATH" gradle install
```
### Step 4 - Test out PX4 + QgroundControl with jMAVSim
```
./QGroundControl.AppImage (or double click)
make px4_sitl jmavsim (you may fail ude to lack of some python packages and install them based on cmake output information.)
```